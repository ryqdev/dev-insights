# Effective Linux: Commands, Tools, and Configurations

## 1. Essential Linux Commands

### 1.1 `fd` - A Fast and User-Friendly File Search Tool

`fd` is a modern alternative to the `find` command. It provides a simpler syntax, faster performance, and a more intuitive output.

**Usage:**

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

### 1.2 `ripgrep` - A Fast Search Tool for Code and Text

`ripgrep` (`rg`) is a line-oriented search tool that recursively searches for regex patterns in the current directory or specified files. It is a faster alternative to `grep`.

**Usage:**

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

### 1.3 `curl` - Transfer Data from URLs

`curl` is a command-line tool for transferring data with URL syntax, supporting a variety of protocols.

**Usage:**

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

# Only display the HTTP status code
curl -w "%{http_code}\n" -o /dev/null -s https://www.example.com
```

### 1.4 `wc` - Word Count

`wc` is used to count lines, words, and characters in files.

**Usage:**

```shell
wc -l <file_name>  # Count lines in a file
```

### 1.5 `sed` - Stream Editor

`sed` is used for parsing and transforming text.

**Usage:**

```shell
echo "a b\nc d" | sed 's/a/aa/g'
# Output:
# aa b
# c d
```

### 1.6 `awk` - Pattern-Scanning and Processing Language

`awk` is a powerful tool for text processing and reporting.

**Usage:**

```shell
echo "a,b,c" | awk -F',' '{print $2}'
# Output:
# b
```

### 1.7 Print Real Timestamps with `tail` and `awk`

Print real-time logs with timestamps:

```shell
tail -f <log_file_path> | awk '{now=strftime("%F %T%z\t");sub(/^/, now);print}'
```

### 1.8 `lsb_release` - Get Distribution Information

`lsb_release` displays Linux distribution information.

**Usage:**

```shell
lsb_release -a
```

### 1.9 `grep` - Search Text Using Patterns

`grep` is a powerful search utility to find specific patterns within files.

**Usage:**

```shell
grep -r --include=*.go "<text>"
```

## 2. Shell Scripting

### 2.1 Variables

Define variables and access them in shell scripts:

```shell
a=1
echo $a  # or
echo ${a}
```

**Recommendation:** Use `${a}` to avoid ambiguity.

### 2.2 Executing Shell Commands

The difference between `.` and `source` is minimal, but `source` is more commonly recommended for readability.

More information: [Shell Tools](https://missing.csail.mit.edu/2020/shell-tools/)

## 3. System Configuration Commands

### 3.1 `ufw` - Uncomplicated Firewall

`ufw` is a simple firewall management tool for Linux.

**Usage:**

```shell
ufw status        # Check firewall status
ufw allow <Port>  # Allow traffic on a specific port
```

## 4. Arch Linux Tips

### 4.1 Map Ctrl to Caps Lock

Configure Caps Lock to function as Ctrl:

```bash
setxkbmap -option caps:ctrl_modifier
```

### 4.2 Sound Configuration

Adjust the sound volume using `amixer`:

```bash
amixer set Master playback 3+
```

### 4.3 Bluetooth Configuration

Manage Bluetooth devices:

```shell
bluetoothctl
# Then use the following commands:
# scan on
# pair <MAC ADDR>
```

### 4.4 i3 Status Configuration

Customize i3 window manager status bar:

```bash
i3status | while :
do
    read line
    value=$(cat /home/ryqdev/Projects/fetch-stock-price/data)
    echo "AAPL $value | $line" || exit 1
done
```

### 4.5 Set Proxy for `apt`

Configure a proxy for `apt` in `/etc/apt/apt.conf`:

```shell
Acquire::http::Proxy "http://127.0.0.1:<port>";   # Replace <port> with your port
Acquire::https::Proxy "http://127.0.0.1:<port>";  # Replace <port> with your port
```

### 4.6 Remote Desktop (RDP)

For connecting to Ubuntu from macOS via RDP, refer to this [guide on Ask Ubuntu](https://askubuntu.com/questions/893831/remote-desktop-connection-from-mac-to-ubuntu).

---