---
layout: post
title:  "User and login setup - Ubuntu remote server, Windows client"
date:   2021-11-21 11:32:01 -0500
categories: 
---

## Basics

This is an abbreviated, Windows-specific guide to setting up a (fairly) secure login process for a remote Ubuntu 20.04 server. More-detailed guides are at [Linode](https://www.linode.com/docs/guides/securing-your-server/) and Digital Ocean (see [server setup](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04) and [SSH setup](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-20-04)) and [linked below](#loose-ends-and-additional-resources).

First, we need to create an **ordinary user** that can **act as root** when needed. While logged in to the remote server via SSH as root (`ssh root@my.ip.address`) using password authentication, do:
```bash
adduser newuser # follow prompts, set password
usermod -aG sudo newuser # add newuser to sudo group
```

After testing to ensure you can login as `newuser` with this password and use `sudo`, next steps are to disable root SSH access entirely, to **enable key-based login** for the new user, and then disable password-based login. 

- Disable root login: on *server*, edit file `/etc/ssh/sshd_config` to say:
    ```bash
    PermitRootLogin no
    ```
- Reload `sshd` to make change active:
    ```bash
    systemctl reload sshd
    ```
- Create key pair on *local* machine (I'm on Windows, using PowerShell):
    ```bash
    ssh-keygen -b 4096
    ```
    - *Note*: If you use Windows/PowerShell, this key will end up in the normal Windows filesystem. Take note of the location!
    - *And*: I chose not to protect the private key with a password. This is obviously less secure than using a password. But I want to be able to use VS Code to remotely edit files on the server, and every file you open that way opens a new remote session. I didn't want to have to enter my password every time I opened a file. Plus, nobody but me uses my dev machine. 

- Create destination for public key on *server*, logged in to it as `newuser`:
    ```bash
    mkdir ~/.ssh && sudo chmod -R 700 ~/.ssh/
    ```
- Upload public key from *local* machine to server (where WINUSER is your Windows user name):
    ```bash
    scp /mnt/c/Users/WINUSER/.ssh/id_rsa.pub newuser@server.ip.address:~/.ssh/authorized_keys
    ```
    - *Query:* Why use WSL here, when I used Windows/PowerShell to generate the key? 
    - *Answer:* Because the `scp` command is available only in WSL, and I didn't want to figure out how to do the equivalent in Windows/PowerShell.
- Make key file on *server* even more secure by limiting read permissions to owner:
    ```bash
    chmod 600 ~/.ssh/authorized_keys
    ```
- Disable password-based authentication and require public/private-key authentication: on *server*, edit file `/etc/ssh/sshd_config` to say:
    ```bash
    PasswordAuthentication no
    ```
    - *Note*: You might see commented-out lines about `PubkeyAuthentication` and `AuthorizedKeysFile` and think, "I should uncomment these." You shouldn't have to. These are [already the default values, included in comments just informationally.](https://serverfault.com/questions/1057053/for-ssh-access-only-do-i-leave-pubkeyauthentication-yes-commented-out-in-ssh)
- Again, reload `sshd` to make change active:
    ```bash
    systemctl reload sshd
    ```

At this point, you will be able to use Windows/PowerShell to log in to your remote machine as `newuser` using the public/private keypair by doing:
```bash
ssh newuser@remote.ip.address
```

What if you want to use *other methods* on the same machine to log in to your server? Say, using Putty? Or using WSL? 

- Using WSL is easy. It's just a matter of copying your private key (`id_rsa`) from the user directory in Windows into the user directory in WSL, then [changing permissions](https://askubuntu.com/questions/134975/copy-ssh-private-keys-to-another-computer) on the key in WSL. Doing this within WSL should work (assuming your Windows user directory is `C:/Users/WINUSER/`):
    ```bash
    cp /mnt/c/Users/WINUSER/.ssh/id_rsa ~/.ssh/id_rsa
    cd ~/.ssh/
    chmod 600 id_rsa
    ```
    - You should now be able to SSH into your remote server from within WSL by doing:
    - `ssh newuser@remote.ip.address`
- Setting up Putty (I use [Putty Portable](https://portableapps.com/apps/internet/putty_portable)) requires an extra step, because Putty uses a different key format. To enable login using Putty:
    - Use `puttygen.exe` to convert the key into Putty's format (you'll find it in the `/App/putty/` subdirectory of the main Putty installation). In its GUI: 
        - `Load` the existing file `id_rsa` (you will need to show all files to see it), then 
        - save it as `whatever-you-want.ppk`. 
    - For simplicity's sake, you can save it in the same Windows user `.ssh` directory as the source `id_rsa` file. But since you will be telling Putty the location of this `.ppk` file, it doesn't really matter.
    - When using Putty to connect, in the Putty GUI:
        - in `Connection -> SSH -> Auth`, you'll find an option to `Browse` for a private key for authentication. 
        - Browse to the `.ppk` file you created. 
        - Open your connection to the remote server from the `Session` menu.

## Loose ends and additional resources

I do not know enough about managing *multiple users for the remote machine,* or about managing *multiple machines for a single Windows user.* Throughout this process, there are various steps where one might overwrite an existing key. How to **keep different keys side-by-side** is, for me, still an open question.

More can, and probably should, be done to secure a server. See:
  - [Linode's guide to advanced SSH security](https://www.linode.com/docs/guides/advanced-ssh-server-security/)
  - [Other Linode security guides](https://www.linode.com/docs/guides/security/basics/)
  - [Digital Ocean's SSH-hardening guide](https://www.digitalocean.com/community/tutorials/how-to-harden-openssh-client-on-ubuntu-20-04)
  - [Digital Ocean's general security guide](https://www.digitalocean.com/community/tutorials/recommended-security-measures-to-protect-your-servers)