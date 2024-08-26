## 1 Linux Commands
Some awesome Linux commands.

### 1.1 fd
`fd` is a fast and user-friendly command-line tool to find files and directories in a filesystem.

Alternative to `find` in Linux.

Usage:
```shell
# Basic usage of fd to search for a file or directory
fd <pattern>
# Search for a pattern ignoring case
fd -i <pattern>
# Search for a pattern in a specific directory
fd <pattern> /path/to/directory
# Search for a pattern excluding certain directories
fd <pattern> --exclude dir1 --exclude dir2
# Search for a pattern only in certain file types
fd <pattern> --type f
```

### 1.2 ripgrep
`ripgrep` is a line-oriented search tool that recursively searches your current directory for a regex pattern.

Alternative for `grep` in Linux

Usage:
```shell
# Basic usage of ripgrep to search for the term "pattern" in the current directory
rg <pattern>
# Search for "pattern" recursively in a specific directory
rg <pattern> /path/to/directory
# Search for "pattern" in all files with a .txt extension
rg <pattern> -g '*.txt'
# Search for "pattern" while ignoring case
rg -i <pattern>
# Search for "pattern" in all files while ignoring those listed in .gitignore
rg <pattern> --no-ignore
```

### 1.3 curl
`curl` is a command-line tool for transferring data specified with URL syntax.

Usage:
```shell
# Basic usage of curl to download a file
curl -O http://www.example.com/file.txt
# Use curl to send a GET request
curl http://www.example.com
# Use curl to send a POST request with data
curl -d "data=example" http://www.example.com
# Use curl to send a POST request with a file
curl -F "file=@/path/to/file" http://www.example.com
# Use curl to follow redirects
curl -L http://www.example.com
# Only display http status code
curl -w "%{http_code}\n" -o /dev/null -s https://www.example.com
```

### wc
```shell
wc -l <file_name>
```

### sed
```shell
echo "a b\nc d" | sed 's/a/aa/g'
# aa b
# c d
```

### awk
```shell
echo "a,b,c" | awk -F',' '{print $2}'

# b
```


### lsb_release 
Get the distro information
```shell
lsb_release -a
```

## 2 Shell Script
### 2.1 Variables
```shell
a=1
echo $a
#or
echo ${a}
```

`${a}` is recommended 

### 2.2 Executing Shell
<https://missing.csail.mit.edu/2020/shell-tools/>

`.` and `source` are similar, but `source` is recommended

## 3 System Configuration Commands
### 3.1 ufw
```shell
ufw status
ufw allow <Port>
```

## 4 Arch Linux

### map ctrl to caps lock

```bash
setxkbmap -option caps:ctrl_modifier
```

### Sound configuration

```bash
amixer set Master playback 3+
```

### Bluetooth configuration
```shell
bluetoothctl

# scan on
# pair <MAC ADDR>
```

### I3 status configuration

```bash
i3status | while :
do
    read line
	value=$(cat /home/ryqdev/Projects/fetch-stock-price/data)
	echo "AAPL $value | $line" || exit 1
done
```

### Set Proxy for Apt
Add in `/etc/apt/apt.conf`:
```shell
Acquire::http::Proxy "http://127.0.0.1:<port>"; # use your port
Acquire::https::Proxy "http://127.0.0.1:<port>"; # use your port
```

### RDP
https://askubuntu.com/questions/893831/remote-desktop-connection-from-mac-to-ubuntu