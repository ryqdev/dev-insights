# Can u catch the panic in Golang?

## Show Case
### Panic Catched
Case1
```golang
package main

import "fmt"

func main() {
    defer DeferRecover()()

    fmt.Println("biz logic")

    panic("Can u catch me?")
}

func DeferRecover() (x func()) {
    x = func() {
       if err := recover(); err != nil {
          fmt.Printf("Catch the panic: %+v", err)
       }
    }
    return x
}
```

case2
```golang
package main

import "fmt"

func main() {
    defer func() {
       if err := recover(); err != nil {
          fmt.Printf("Catch the panic: %+v\n", err)
       }
    }()

    fmt.Println("biz logic")

    panic("Can u catch me?")
}
```

case3
```golang
package main

import (
    "fmt"
    "time"
)

func main() {
    go func() {
       defer func() {
          if err := recover(); err != nil {
             fmt.Printf("Catch the panic: %+v", err)
          }
       }()
       fmt.Println("biz logic")
       panic("Can u catch me?")
    }()
    time.Sleep(time.Second)
}
```

### Panic Not Catched
Case1
```golang
package main

import "fmt"

func main() {
    defer func() {
       DeferRecover()()
    }()

    fmt.Println("biz logic")

    panic("Can u catch me?")
}

func DeferRecover() (x func()) {
    x = func() {
       if err := recover(); err != nil {
          fmt.Printf("Catch the panic: %+v", err)
       }
    }
    return x
}
```

Case2
```golang
package main

import "fmt"

func main() {
    defer func() {
       DeferRecover()()

       fmt.Println("biz logic")

       panic("Can u catch me?")
    }()

}

func DeferRecover() (x func()) {
    x = func() {
       if err := recover(); err != nil {
          fmt.Printf("Catch the panic: %+v", err)
       }
    }
    return x
}
```

Case3
```golang
package main

import (
    "fmt"
    "time"
)

func main() {
    go func() {
       defer DeferRecover()
       fmt.Println("biz logic")
       panic("Can u catch me?")
    }()
    time.Sleep(time.Second)
}

func DeferRecover() (x func()) {
    x = func() {
       if err := recover(); err != nil {
          fmt.Printf("Catch the panic: %+v", err)
       }
    }
    return x
}
```

Case4
```golang
package main

import "fmt"

func main() {
    defer DeferRecover()

    fmt.Println("biz logic")

    panic("Can u catch me?")
}

func DeferRecover() (x func()) {
    x = func() {
       if err := recover(); err != nil {
          fmt.Printf("Catch the panic: %+v", err)
       }
    }
    return x
}
```


## Investigation
### How does `defer` work?

Based on the `45446c867a` commit id in https://github.com/golang/go

```golang
// A _defer holds an entry on the list of deferred calls.
// If you add a field here, add code to clear it in deferProcStack.
// This struct must match the code in cmd/compile/internal/ssagen/ssa.go:deferstruct
// and cmd/compile/internal/ssagen/ssa.go:(*state).call.
// Some defers will be allocated on the stack and some on the heap.
// All defers are logically part of the stack, so write barriers to
// initialize them are not required. All defers must be manually scanned,
// and for heap defers, marked.
type _defer struct {
    heap      bool
    rangefunc bool    // true for rangefunc list
    sp        uintptr // sp at time of defer
    pc        uintptr // pc at time of defer
    fn        func()  // can be nil for open-coded defers
    link      *_defer // next defer on G; can point to either heap or stack!

    // If rangefunc is true, *head is the head of the atomic linked list
    // during a range-over-func execution.
    head *atomic.Pointer[_defer]
}
```
- link is the poiter of `_defer` struct. Therefore the `defer` is in the format of linked list


### How does panic work?
```golang
// A _panic holds information about an active panic.
//
// A _panic value must only ever live on the stack.
//
// The argp and link fields are stack pointers, but don't need special
// handling during stack growth: because they are pointer-typed and
// _panic values only live on the stack, regular stack pointer
// adjustment takes care of them.
type _panic struct {
    argp unsafe.Pointer // pointer to arguments of deferred call run during panic; cannot move - known to liblink
    arg  any            // argument to panic
    link *_panic        // link to earlier panic

    // startPC and startSP track where _panic.start was called.
    startPC uintptr
    startSP unsafe.Pointer

    // The current stack frame that we're running deferred calls for.
    sp unsafe.Pointer
    lr uintptr
    fp unsafe.Pointer

    // retpc stores the PC where the panic should jump back to, if the
    // function last returned by _panic.next() recovers the panic.
    retpc uintptr

    // Extra state for handling open-coded defers.
    deferBitsPtr *uint8
    slotsPtr     unsafe.Pointer

    recovered   bool // whether this panic has been recovered
    goexit      bool
    deferreturn bool
}
```
- argp is the pointer to arguments of deferred call run during panic

The proccess of handling panic

```golang
// The implementation of the predeclared function panic.
// The compiler emits calls to this function.
//
// gopanic should be an internal detail,
// but widely used packages access it using linkname.
// Notable members of the hall of shame include:
//   - go.undefinedlabs.com/scopeagent
//   - github.com/goplus/igop
//
// Do not remove or change the type signature.
// See go.dev/issue/67401.
//
//go:linkname gopanic
func gopanic(e any) {
    .
    .
    .
    var p _panic
    p.arg = e

    runningPanicDefers.Add(1)

    p.start(getcallerpc(), unsafe.Pointer(getcallersp()))
    for {
       fn, ok := p.nextDefer()
       if !ok {
          break
       }
       fn()
    }

    // If we're tracing, flush the current generation to make the trace more
    // readable.
    //
    // TODO(aktau): Handle a panic from within traceAdvance more gracefully.
    // Currently it would hang. Not handled now because it is very unlikely, and
    // already unrecoverable.
    if traceEnabled() {
       traceAdvance(false)
    }

    // ran out of deferred calls - old-school panic now
    // Because it is unsafe to call arbitrary user code after freezing
    // the world, we call preprintpanics to invoke all necessary Error
    // and String methods to prepare the panic strings before startpanic.
    preprintpanics(&p)

    fatalpanic(&p)   // should not return
    *(*int)(nil) = 0 // not reached
}
```

It will iterate the defer linked list, and execute the defer function.

### How does recover work?
```golang
// The implementation of the predeclared function recover.
// Cannot split the stack because it needs to reliably
// find the stack segment of its caller.
//
// TODO(rsc): Once we commit to CopyStackAlways,
// this doesn't need to be nosplit.
//
//go:nosplit
func gorecover(argp uintptr) any {
    // Must be in a function running as part of a deferred call during the panic.
    // Must be called from the topmost function of the call
    // (the function used in the defer statement).
    // p.argp is the argument pointer of that topmost deferred function call.
    // Compare against argp reported by caller.
    // If they match, the caller is the one who can recover.
    gp := getg()
    p := gp._panic
    if p != nil && !p.goexit && !p.recovered && argp == uintptr(p.argp) {
       p.recovered = true
       return p.arg
    }
    return nil
}
```
`p.recovered = true` will set the recovered flag to true, denoting that the panic has been recovered (but not actually recovered, the recover action is in the nextDefer).
```golang
if p.recovered {
    mcall(recovery) // does not return
    throw("recovery failed")
}
```


## Conclusion
1. In gorecover(argp uintptr), the argp is actually the address of the argument of the function that calls gorecover. Typically, it can be the address of the first argument of the deferred anonymous function.
2. The value of p.argp is the stack pointer (SP) of the caller plus an offset. The offset is MinFrameSize (8 bytes). The comment for p.argp also explains: "pointer to arguments of deferred call run during panic; cannot move - known to liblink", meaning that it is the address of the argument when the defer function is called during a panic.

```golang
p.startSP = unsafe.Pointer(getcallersp())
p.argp = add(p.startSP, sys.MinFrameSize)
```

## Best Practice
The rule we previously learned was that `recover` must be used inside a `defer`, but that’s not the whole story. 

From the source code, it is actually hard-coded that recover must be called within a function which is exactly one function call.

```golang
func xxxx() {
    defer func() {
       if err := recover(); err != nil {
          fmt.Printf("Catch the panic: %+v\n", err)
       }
    }()
    
    fmt.Println("biz logic")
    
    panic("Can u catch me?")
}
```


## Appendix
[1] https://bytetech.info/articles/6870369508184457223?searchId=2024012415304137CFFA43CB297503C2B2
[2] 左书祺(2021年).《Go 语言设计与实现》. 人民邮电出版社