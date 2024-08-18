# Async

## Rust

Synchronous version:

```rust
use chrono::Local;  
use std::thread;  
  
fn mock_io() {  
    println!("start in mock IO at: {}", Local::now().format("%F %T.%3f"));  
    thread::sleep(std::time::Duration::from_secs(2));  
    println!("end in mock IO at: {}", Local::now().format("%F %T.%3f"));  
}  
  
fn main() {  
    println!("start in main at: {}", Local::now().format("%F %T.%3f"));  
    mock_io();  
    mock_io();  
    println!("end in main at: {}", Local::now().format("%F %T.%3f"));  
}
```

Asynchronous version:

use `tokio::join`  to write async code in Rust

```rust
use chrono::Local;  
use tokio;  
use std::thread;  
  
  
async fn mock_io() {  
    println!("start in mock IO at: {}", Local::now().format("%F %T.%3f"));  
    tokio::time::sleep(tokio::time::Duration::from_secs(2)).await;  
    println!("end in mock IO at: {}", Local::now().format("%F %T.%3f"));  
}  
  
  
#[tokio::main]  
async fn main() {  
    println!("start in main at: {}", Local::now().format("%F %T.%3f"));  
    let task1 = mock_io();  
    let task2 = mock_io();  
    tokio::join!(task1, task2);  
    println!("end in main at: {}", Local::now().format("%F %T.%3f"));  
}
```

## Python

Synchronous  version:

``` python
import time  
  
  
def mock_io():  
    print(f"start in IO at {time.strftime('%X')}")  
    time.sleep(2)  
    print(f"end in IO at {time.strftime('%X')}")  
  
  
def main():  
    print(f"start in main at {time.strftime('%X')}")  
    mock_io()  
    mock_io()  
    print(f"end in main at {time.strftime('%X')}")  
  
  
if __name__ == "__main__":  
    main()

# output 
# start in main at 17:32:31  
# start in IO at 17:32:31  
# end in IO at 17:32:33  
# start in IO at 17:32:33  
# end in IO at 17:32:35  
# end in main at 17:32:35
```


Asynchronous  version:
```python

import time  
import asyncio  
  
  
async def mock_io():  
    print(f"start in IO at {time.strftime('%X')}")  
    await asyncio.sleep(2)  
    print(f"end in IO at {time.strftime('%X')}")  
  
  
async def main():  
    print(f"start in main at {time.strftime('%X')}")  
    await asyncio.gather(  
        mock_io(),  
        mock_io()  
    )  
    print(f"end in main at {time.strftime('%X')}")  
  
  
if __name__ == "__main__":  
    asyncio.run(main())

# output:
# start in main at 18:21:43  
# start in IO at 18:21:43  
# start in IO at 18:21:43  
# end in IO at 18:21:45  
# end in IO at 18:21:45  
# end in main at 18:21:45
```

## Golang

Synchronous version:

```go
package main  
  
import (  
    "fmt"  
    "time")  
  
func mock_io() {  
    fmt.Printf("start in mock IO at %+v\n", time.Now())  
    time.Sleep(time.Second * 2)  
    fmt.Printf("end in mock IO at %+v\n", time.Now())  
}  
  
func main() {  
    fmt.Printf("start in main at %+v\n", time.Now())  
    mock_io()  
    mock_io()  
    fmt.Printf("end in main at %+v\n", time.Now())  
}
```


Asynchronous  version with channel and wait group

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

func mock_io() {
	fmt.Printf("start in mock IO at %+v\n", time.Now())
	time.Sleep(time.Second * 2)
	fmt.Printf("end in mock IO at %+v\n", time.Now())
}

func main() {
	fmt.Printf("start in main at %+v\n", time.Now())

	var wg sync.WaitGroup
	ch := make(chan struct{}, 3)

	for i := 0; i < 9; i++ {
		ch <- struct{}{}
		wg.Add(1)
		go func(i int) {
			defer func() { <-ch }()
			defer wg.Done()
			mock_io()
		}(i)
	}

	wg.Wait()
	fmt.Printf("end in main at %+v\n", time.Now())
}

```

Async with pool
```go

package main

import (
	"fmt"
	"sync"
	"time"
)

type Pool struct {
	queue chan struct{}
	wg    sync.WaitGroup
}

func NewPool(size int) Pool {
	return Pool{
		queue: make(chan struct{}, size),
		wg:    sync.WaitGroup{},
	}
}

func (p *Pool) Add(count int) {
	for i := 0; i < count; i++ {
		p.queue <- struct{}{}
	}
	p.wg.Add(count)
}

func (p *Pool) Done() {
	<-p.queue
	p.wg.Done()
}

func (p *Pool) Wait() {
	p.wg.Wait()
}

func main() {
	p := NewPool(5)
	for i := 0; i <= 10; i++ {
		p.Add(1)
		go func(i int) {
			defer p.Done()
			time.Sleep(time.Second)
			fmt.Println(i)
		}(i)
	}
	p.Wait()
	fmt.Println("Done")
}

```


Producer-Consumer Model:
```go
package main

import (
	"fmt"
	"sync"
	"time"
)

const MAX_CHAN_ITEM = 5
const MAX_ITEM = 10

type Msg struct {
	value int
}

func producer(ch chan<- Msg, wg sync.WaitGroup) {
	defer wg.Done()
	for i := 0; i < MAX_ITEM; i++ {
		fmt.Printf("Producing %d\n", i)
		ch <- Msg{
			value: i,
		}
		time.Sleep(time.Second)
	}
}

func consumer(ch <-chan Msg, wg sync.WaitGroup) {
	defer wg.Done()
	for msg := range ch {
		fmt.Printf("Consuming msg: %+v\n", msg)
	}
}

func main() {
	var wg sync.WaitGroup
	ch := make(chan Msg, MAX_CHAN_ITEM)
	defer close(ch)

	wg.Add(1)
	go producer(ch, wg)

	wg.Add(1)
	go consumer(ch, wg)

	wg.Wait()
}

```