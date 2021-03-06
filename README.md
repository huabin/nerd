# nerd
Some configurations on my development machine

I used to use a VM on [Google Cloud](https://cloud.google.com) as my development machine, but now I switched to [AWS Lightsail](https://lightsail.aws.amazon.com).

#### 💻OS

Debian 9.5

Linux nerd 4.9.0-12-amd64 #1 SMP Debian 4.9.210-1+deb9u1 (2020-06-07) x86_64 GNU/Linux

#### 🌟Name

Its name is `nerd`, there are two places that need to be changed.

First, hostname

```
sudo vi /etc/hostname
```

Second, hosts

```
sudo vi /etc/cloud/templates/hosts.debian.tmpl
```

You should change this file, not `/etc/hosts`. Add `127.0.0.1 nerd`

#### 👦User

There are two users, `root` and `admin`. But I additionally created a user `nerdone` with sudo permission to log in with a password.

```
adduser nerdone
usermod -aG sudo nerdone
```

Set `PasswordAuthentication`'s value to `yes` in the file `sshd_config`

```
sudo sed -ri 's/^#?(PasswordAuthentication)\s+(yes|no)/\1 yes/' /etc/ssh/sshd_config
sudo service sshd restart
```

#### 🐳Docker

Installed via [Dockerman](https://github.com/tourcoder/dockerman)

#### 🦇Git

Installed via command `sudo apt install git`

#### 🐿️Go

Installed via [glv](https://github.com/glv-go/glv)

#### 🦨NodeJS

Installed via [nvm-sh](https://github.com/nvm-sh/nvm)

#### 🐚Shell

I use [ZSH](https://en.wikipedia.org/wiki/Z_shell) and [Oh my ZSH](https://ohmyz.sh)

```
sudo apt install zsh -y
zsh --version
```

Install it and check the version

```
whereis zsh
sudo usermod -s /usr/bin/zsh $(whoami)
```

Set ZSH as the default login shell for the logged in user

```
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Install Oh-my-zsh via the above command. 

Restart the server and done.

#### 🔗How to connect to Nerd

Usually, I use two ways to connect to Nerd, [Mosh](https://mosh.org) and [VSCode](https://code.visualstudio.com) with extension [Remote-SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh).
