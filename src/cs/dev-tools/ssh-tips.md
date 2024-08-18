# SSH

## key-pairs configuation

```shell
ssh-keygen -t rsa -f <file path> -C [user name]
```

### Config file example

```
Host XXXX 
  HostName XX.XXX.XXX.126
  User XXXX
  Port XXXX
  Identityfile ~/.ssh/[YOUR KEY]
```

### Execute remote commands with ssh
```shell
#e.g.
ssh <hostname>@<ip> ls
```

### How to put public key to server?

Find `authorized_keys` in `~/.ssh/` of server. Add public key in this file.

```
....
....
​
Your Public key..............
...............
...............
```

However, if we are using servers from google cloud, authorized\_keys will be recovered by daemon. Solution:[Managing SSH keys in metadata](https://cloud.google.com/compute/docs/instances/adding-removing-ssh-keys). add ssh public key in metadata rather than modify authorized\_keys file.

## ssh gateway script

`spawn`: launch a new process

`send`: send string to process(similar with typing in the terminal)

`expect`: accecpt string from the process

`exp_continue`: accecpt string from the process, and continue expect

`interact` let user interact with terminal

```shell
#!/usr/bin/expect
set salt [lindex $argv 0];
set gateway1Username [Your gateway1 host]; 
set gateway1Pass [Your gateway1 password]; 
set gateway2Username [Your gateway2 host];
set gateway2Pass [your gateway2 password]; 
set server [Your target server host]; 
set serverPass [Your target server password]; 
set timeout -1
# connect gateway1
spawn ssh $gateway1Username
expect {
    "*yes/no*" {
        send "yes\n";
        exp_continue;
    }
    "password:" {
        send "$gateway1Pass\n";
    }
}
# connect gateway2
expect {
    "*something your should write*" {
        send "ssh $gateway2Username\r";
        exp_continue;
    }
    "*password:" {
        send "$gateway2Pass\r";
    }
}
# connect target
expect {
    "*something your should write*" {
        send "ssh $server\r";
        exp_continue;
    }
     "*yes/no*" {
        send "yes\n";
        exp_continue;
    }
    "*password:" {
        send "$serverPass\r";
    }
}
interact
```
