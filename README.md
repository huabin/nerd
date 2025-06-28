# nerd
Some configurations on my development machine

I used to use a VM on [Google Cloud](https://cloud.google.com) as my development machine, but now I switched to [AWS Lightsail](https://lightsail.aws.amazon.com).

#### 💻OS

Debian 11

Linux nerd 5.10.0-22-cloud-amd64 #1 SMP Debian 5.10.178-3 (2023-04-22) x86_64 GNU/Linux

#### 🌟Name

Its name is `nerd`, there are two places that need to be changed. 

```
echo 'nerd'>/etc/hostname
echo '127.0.0.1 nerd'>>/etc/cloud/templates/hosts.debian.tmpl //optional since Debian 10
```

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

#### 🔑Authentication Key

Sometimes a connection with an authentication key is more stable and secure than a connection with a password.

```
ssh-keygen -t rsa -C "code@tourcoder.com" -f "~/.ssh/id_rsa_nerd"
```

Highly recommended to set a password for your key.

```
cat id_rsa.pub >> authorized_keys
chmod 600 authorized_keys
chmod 700 ~/.ssh
```

Download the file `id_rsa_nerd` and keep it.

#### 🔐Safe

In any case, remote login with root should be disabled. Change `PermitRootLogin` to `no`.

```
sudo sed -E -i 's/^(#\s*)?PermitRootLogin\s+\b(yes|no)\b/PermitRootLogin no/' /etc/ssh/sshd_config
```

**UFW**

```
apt install ufw -y
ufw default deny incoming //Set the default policy to deny
ufw default allow outgoing //But treat existing connections conservatively
ufw app list //allowed list
ufw allow app_name //allowing an application to pass, such as OpenSSH
ufw allow port //allowing port to pass, don't forget 22
ufw status //before enable, must check if 22/ssh/openssh exist
ufw enable
```

**fail2ban**

```
apt install fail2ban -y
systemctl enable fail2ban
```

```
cd /etc/fail2ban
cp jail.conf jail.local
vi jail.local
```

Edit the local jail file, and set

```
bantime = 86400 // ban for 24 hours
banaction = ufw // use what ufw allows
```

```
service fail2ban restart
fail2ban-client ping
```

Restart and check if the configuration is working. 

#### 🐳Docker

Installed via [Dockerman](https://github.com/tourcoder/dockerman)

#### 🦇Git

Installed via command `sudo apt install git`

#### 🐿️Go

Installed via [glv](https://github.com/glv-go/glv)

#### 🦨NodeJS

Installed via [fnm](https://fnm.vercel.app)

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

#### 🔗How to connect to nerd

Usually, I use two ways to connect to nerd, [Mosh](https://mosh.org) and [VSCode](https://code.visualstudio.com) with extension [Remote-SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh).

- Mosh

  **Install mosh on nerd**

  ```
  apt install mosh -y
  ```

  **Install mosh on macOS**

  ```
  brew install mosh
  ```

  **Connect**
  
  Connect with a password
  
  ```
  mosh nerdone@nerd
  ```
  
  or connect with authentication key
  
  ```
  mosh -i id_rsa_nerd nerdone@nerd
  ```
  
  Need to open UDP port `60000-65535`.
  
