# SSH Notes: Key Configurations and Scripting Practices

## Introduction to SSH

SSH (Secure Shell) is a protocol used to securely connect to remote servers, transfer files, and execute commands. SSH uses key-based authentication and encryption to provide a secure way of accessing remote systems.

## Key-Pair Configuration

To create an SSH key pair, use the `ssh-keygen` command. An SSH key pair consists of a public key and a private key. The public key is added to the remote server, and the private key remains on your local machine.

**Generate an SSH key pair:**

```shell
ssh-keygen -t rsa -f <file path> -C [user name]
```

- `-t rsa`: Specifies the type of key to create (RSA).
- `-f <file path>`: Specifies the file path to save the key.
- `-C [user name]`: Adds a comment (e.g., username) to the key.

## SSH Config File

The SSH `config` file allows you to simplify SSH connections by defining shortcuts for hosts. This is particularly useful if you frequently connect to different servers.

### Config File Example

Create or edit the `~/.ssh/config` file to add the following configuration:

```plaintext
Host myserver 
  HostName <ip address>
  User <user name>
  Port <port>
  IdentityFile <path to private key file>
```

- **Host**: Alias for the server (e.g., `myserver`).
- **HostName**: The server's IP address or domain.
- **User**: The username to use when connecting.
- **Port**: The port number for SSH (default is `22`).
- **IdentityFile**: The path to your private key.

After adding this configuration, you can simply use `ssh myserver` to connect instead of typing the full command.

## Executing Remote Commands with SSH

SSH allows you to run commands on a remote server directly from your local machine:

```shell
ssh <user>@<hostname> <command>
```

**Example:**

```shell
ssh myuser@ip ls
```

## Adding a Public Key to a Server

To enable key-based authentication, you need to add your public key to the remote server's `authorized_keys` file.

1. **Locate the `authorized_keys` file**: On the remote server, this file is usually located at `~/.ssh/authorized_keys`.
2. **Add your public key**: Open the `authorized_keys` file in a text editor and paste your public key (from `~/.ssh/id_rsa.pub` or wherever your key is located).

**Example:**

```plaintext
ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAr... your_public_key ...== user@hostname
```

### Note for Google Cloud Users

On Google Cloud, the `authorized_keys` file may be managed by a daemon and can be overwritten. Instead of editing `authorized_keys` directly, you should manage SSH keys in the [metadata settings](https://cloud.google.com/compute/docs/instances/adding-removing-ssh-keys).

## SSH Gateway Script

When connecting to a target server through multiple gateways, you can automate the process using an `expect` script. This script handles the interaction needed to pass through each gateway.

### SSH Gateway Script Example

The `expect` command is used to automate interactions with processes, making it ideal for multi-step SSH logins.

#### Key Commands in `expect`:

- **`spawn`**: Launches a new process.
- **`send`**: Sends a string to the process (like typing in the terminal).
- **`expect`**: Waits for a specific string from the process.
- **`exp_continue`**: Continues waiting for strings even after a match is found.
- **`interact`**: Allows the user to interact with the terminal after automation.

### Example Script

```shell
#!/usr/bin/expect
set gateway1Username [Your gateway1 host] 
set gateway1Pass [Your gateway1 password] 
set gateway2Username [Your gateway2 host]
set gateway2Pass [Your gateway2 password] 
set server [Your target server host] 
set serverPass [Your target server password] 
set timeout -1

# Connect to Gateway 1
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

# Connect to Gateway 2
expect {
    "*prompt to send from gateway1*" {
        send "ssh $gateway2Username\r";
        exp_continue;
    }
    "*password:" {
        send "$gateway2Pass\r";
    }
}

# Connect to Target Server
expect {
    "*prompt to send from gateway2*" {
        send "ssh $server\r";
        exp_continue;
    }
    "*yes/no*" {
        send "yes\n";
        exp_continue;
    }
    "password:" {
        send "$serverPass\r";
    }
}

interact
```

This script automates connecting through two gateways to reach a target server, handling passwords and confirmations along the way.

## Conclusion

These SSH notes provide a practical guide for setting up key-based authentication, configuring SSH for easier connections, executing remote commands, adding public keys, and creating scripts for complex SSH operations. SSH is a powerful tool for securely managing remote systems, and these practices help optimize and automate its use.

## References

- [SSH Config Documentation](https://linux.die.net/man/5/ssh_config)
- [Managing SSH Keys in Google Cloud Metadata](https://cloud.google.com/compute/docs/instances/adding-removing-ssh-keys)

---