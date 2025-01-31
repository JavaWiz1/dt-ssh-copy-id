# dt-ssh-copy-id

Command-line windows tool that loosely mimics the *nix ssh-copy-id command.


## Purpose

Linux systems have a handle tool called ssh-copy-id, which installs SSH keys on a target server as an authorized key. Its purpose is to provide access without requiring a password for each login.  This provides the ability to enable automated (passwordless) logins using the SSH protocol via an easy CLI interface.

Unfortunately, windows SSH does not supply this tool and setting up ssh keys is a more tedious process.

dt-ssh-copy-id mimics much of the *nix ssh-copy-id utility functions to allow for a similar experience.

## Features:

- Command line driven (loosely mimics the *nix syntax of [ssh-copy-id](https://manpages.debian.org/bookworm/openssh-client/ssh-copy-id.1.en.html))
- Multiple target hosts can be setup via single run
- Identifies if keys are already present on target host(s).

## Pre-requisites

- Python 3.10+ (may work with earlier version, not tested)
- [fabric](https://www.fabfile.org/) - layer over paramiko
- [paramiko](https://www.paramiko.org/) - facilitates SSH interactions with target servers
- [loguru](https://github.com/Delgan/loguru) - manages logging

## Setup

To install as a **CLI** (no source)
- [pipx](https://github.com/pypa/pipx) install dt-ssh-copy-id   
  - above command will create a virtual environment for the ssh-copy-id command.
  - to run simply type ssh-copy-id -h


To install **source code only**
- git clone https://github.com/JavaWiz1/dt-ssh-copy-id.git

If you use [Poetry](https://python-poetry.org/), a virtual environment will be created with the required dependencies.
- poetry install

else, you may install dependencies manually with - 
- pip install fabric loguru

**To run code:**

- poetry run python dt_ssh_copy_id.py -h

  or if installed via pip

- python dt_ssh_copy_id.py -h

## Syntax
```
usage: ssh-copy-id [-h] [-f] [-i FILE] [-p PORT] hostname [hostname ...]

ssh-copy-id - copy ssh public keys to target host

positional arguments:
  hostname              [user@]hostname

options:
  -h, --help            show this help message and exit
  -f, --force           Force copy, no existence check.
  -i FILE, --identity_file FILE
                        the identity file to be copied. If not specified, adds all keys.
  -p PORT, --port PORT  SSH port (default 22)
```

Note:: The above syntax is using the script entrypoint, to run the code directly, the command would be:

```
> cd <source-code-directory>
> poetry run python dt-ssh-copy-id.py <hostname>
```

## Examples

- Install all public keys for user tom onto server1

    ```> ssh-copy-id tom@server1```

- Install ONLY the rsa public key for user tom onto server1,2,3 and 4

    ```> ssh-copy-id -i ~/.ssh/id_rsa.pub tom@server1 tom@server2 tom@server3 tom@server4```

- Install all public keys for configured (.ssh/config) user onto server 'rasberrypi'
  
  ```> ssh-copy-id raspberypi```
---

NOTE::

  - If you have a [.ssh/config](https://www.ssh.com/academy/ssh/config) setup with defaults, those will be used (i.e. pre-defined usernames).
  - You will be prompted for a password upon first login.  
      - That password will be re-used for each server
      - If login fails for a server, you will be re-prompted to supply a valid password.
