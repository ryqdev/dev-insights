# Reverse proxy with frp

[frp](https://github.com/fatedier/frp) is a fast reverse proxy that allows you to expose a local server located behind a NAT or firewall to the Internet.

![image](https://github.com/ryqdev/missing/assets/50010920/cbe0c7bd-1ea2-4f7d-a384-883b68da978b)

## Installation
Access the [releases page](https://github.com/fatedier/frp/releases) and find the proper version for your devices.

e.g.: 
```shell
wget https://github.com/fatedier/frp/releases/download/v0.58.1/frp_0.58.1_linux_amd64.tar.gz

# unzip it
tar -xvf frp_0.58.1_linux_amd64.tar.gz 

# move to /usr/local
mkdir /usr/local/frp
mv frp_0.58.1_linux_amd64/* /usr/local/frp/
```

## Configurations
### On the server
Modify `frps.toml`
```shell
bindPort = aaaa # change it
```

### On the client
Modify `frpc.toml`

```shell
serverAddr = "xx.xx.xx.xxx" # change it
serverPort = aaaa  # change it

[[proxies]]
name = "test-tcp"
type = "tcp"
localIP = "127.0.0.1"
localPort = 22
remotePort = bbbb # change it
```


## Run frp
### Run the server
```shell
./frps -c frps.toml
```

### Run the client
```shell
./frpc -c frpc.toml
```

## Connect with SSH
```shell
ssh <client_user_name>@<server_public_ip_addr> -p <remotePort in frpc.toml>
```

## sshpass
```
sshpass -p <password> ssh <user>@<ip addr>
```

## Advanced: auto start with systemctl

### On the server
```shell
vim /usr/lib/systemd/system/frp.service
```

```
[Unit]
Description=frp
After=network.target remote-fs.target nss-lookup.target

[Service]
User=root
ExecStart=/usr/local/frp/frps -c /usr/local/frp/frps.toml

[Install]
WantedBy=multi-user.target
```


### On the client
```shell
vim /usr/lib/systemd/system/frp.service
```

```
[Unit]
Description=frp
After=network.target syslog.target
Wants=network.target

[Service]
Type=simple
User=root
ExecStart=/usr/local/frp/frpc -c /usr/local/frp/frpc.toml
Restart=always
RestartSec=1
StartLimitInterval=0

[Install]
WantedBy=multi-user.target
```


Reload systemclt daemon
```shell
systemctl daemon-reload
```

Here are some useful commands
```shell
systemctl start frp
systemctl stop frp
systemctl restart frp
systemctl status frp
systemctl enable frp # auto start
systemctl disable frp
```
