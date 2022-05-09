---
layout: post
title:  "Setting up a Windows dev machine in 2022 (part 1)"
date:   2022-04-27 11:02:01 -0500
categories:
---
I recently retired my 7-year-old laptop in favor of a (refurbished) beast of a Dell laptop that I got for software development and media creation. Here, I summarize setting up:

1. [Git](#git)
1. [Powershell and Windows Terminal](#powershell-and-windows-terminal)
1. [Windows Subsystem for Linux (WSL)](#wsl)
1. [VS Code](#vs-code).

## <a id="git"></a>Git

Setting up [Git for Windows](https://gitforwindows.org/) is simple: just run the installer. Note:

- To test your installation, run `git --version`
- Git for Windows provides Git at the command line from within Windows itself, from a Windows, Powershell, or Git Bash prompt (Git Bash comes with Git for Windows). 
- Once you install WSL, though (see below), you'll have access to a Linux version Git from inside WSL. That makes *two* different versions of Git.
- Related, but not essential, programs:
  - [Github Desktop](https://desktop.github.com/)
  - [Github CLI](https://cli.github.com/) 


## <a id="powershell-and-windows-terminal"></a>Powershell and Windows Terminal

### Powershell (*not* "Windows Powershell")

For mysterious reasons, Windows ships only with [Windows Powershell](https://docs.microsoft.com/en-us/powershell/scripting/windows-powershell/install/installing-windows-powershell?view=powershell-7.2), which is **not** the Powershell you want. You want just plain [Powershell](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.2). Install it.

### Windows Terminal

Grab the [Windows Terminal (Preview)]( https://apps.microsoft.com/store/detail/windows-terminal-preview/9N8G5RFZ9XK3) from the Microsoft app store (or the [non-preview edition](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701) if you're cautious). 

Next, set up **fancy prompts** on Windows (prompts for WSL are a slightly different issue; see next section):

- There are basically three steps: (1) install programs for customizing prompts, (2) install fonts, and (3) configure various terminals.
  - An [old blog post by Scott Hanselman](https://www.hanselman.com/blog/how-to-make-a-pretty-prompt-in-windows-terminal-with-powerline-nerd-fonts-cascadia-code-wsl-and-ohmyposh) motivates this process, but the post is quite outdated.
- Programs to install:
  - [posh-git](https://github.com/dahlbyk/posh-git#installation)
    - To enable it in Powershell<sup id="a1">[1](#f1)</sup> inside Windows Terminal, I edited `$profile.CurrentUserAllHosts` to add the line `Import-Module posh-git`
  - [oh-my-posh](https://ohmyposh.dev/docs/)
    - The [installation instructions](https://ohmyposh.dev/docs/installation/windows) and the [oh-my-posh Powershell FAQ](https://ohmyposh.dev/docs/migrating) will get you started. 
        - *Note:* This is *no longer* installed as a Powershell module; many online instructions are outdated.
    - Pick a [theme](https://ohmyposh.dev/docs/themes) - I like [montys](https://ohmyposh.dev/docs/themes#montys)
    - Edit a PowerShell profile of your choice to add a line like this (replace the theme name):
      ```PowerShell
      oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH\montys.omp.json" | Invoke-Expression
      ```
- Fonts: download and install a [nerd font](https://www.nerdfonts.com/font-downloads) you like. Note:
  - VS Code's integrated terminal only likes **monospace** fonts.
  - Not everyone likes ligatures (I don't).
  - Not every font has every icon your `oh-my-posh` theme might call for. If you see squares instead of icons in your prompt, try a different font.
  - You may want to install the font for **all users** to avoid potential problems. (I'm not sure this is needed; it's a guess based on posts I've seen.)
    - If you have a choice between TTF and OTF fonts, pick OTF. The easiest way to install fonts is from a font file's right-click menu. 
  - I went with [DejaVuSansMono Nerd Font](https://www.programmingfonts.org/#dejavu), which seems to have lots of icons. 
- Terminal and program configuration:
  - Edit the Windows Terminal `settings.json` file to specify the `fontFace`.<sup id="a2">[2](#f2)</sup> To set the font for all terminals, add it to the `defaults` section (you could also add a font for a specific terminal in the `list` section):
  ```JSON
          "defaults": 
        {
            "fontFace":  "DejaVuSansMono Nerd Font"
        },
  ```
  - Note: you must use the **font name** as shown in the Windows Fonts interface (*Control Panel → All Control Panel Items → Fonts*). The font's filename won't do it. 

To modify the list of prompts shown in Windows Terminal to **add an admin prompt**:

- Edit the Terminal's `settings.json` file so that the terminal of your choice has `"elevate": true`. You can infer the rest of the format from existing entries.


## <a id="wsl"></a>Windows Subsystem for Linux (WSL)<sup id="a3">[3](#f3)</sup> 

Since my laptop is brand new (to me), I expected WSL to be installed, or at least to be easy to install. It was neither. This [post on cloudbytes.dev by Rehan Haider](https://cloudbytes.dev/snippets/how-to-install-wsl2-on-windows-1011) has simple instructions on setting up WSL and would have saved me some time. 

### Installation

- On most new Windows systems, `wsl --install` should do the job.
  - See also [Microsoft's official instructions](https://docs.microsoft.com/en-us/windows/wsl/install)
- On my system, I had to:
  - Enable WSL and Virtual Machine Platform. 
    - With GUI: *Control panel → Programs → Turn Windows features on and off →* (1) check Windows Subsystem for Linux, (2) check Virtual Machine Platform
    - Or at command line (elevated prompt): 
    ```PowerShell
    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
    dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
    ```
- Install WSL
  - I *think* I was able to run `wsl --install` after enabling it, but I'm not sure. 
  - [Microsoft's instructions](https://docs.microsoft.com/en-us/windows/wsl/install-manual) tell you to download an installer for WSL. I'm sure that works too.
- Update WSL.
  - Even though the installation seemed fine, Docker complained, and I had to update WSL immediately after installation. Not sure if I used the command line or an installer.
- Install a Linux distro (Ubuntu 20.04) in WSL. I did *not* use the Microsoft store; I did it from the command line. 
- For all of this, **`wsl --help`** is your friend. 

### Prettifying WSL Prompts

There are *probably* three ways to do this: (1) using [Powerline-go](https://github.com/justjanne/powerline-go) within WSL, (2) using [regular Powerline](https://powerline.readthedocs.io/en/latest/index.html) within WSL ([tutorial](https://www.ricalo.com/blog/install-powerline-windows/)), or (3) using `oh-my-posh`, which has a Linux version, within WSL. 
- I say "probably" because I chose option (1), based on Scott Hanselman's outdated blog post, without really thinking it through. If I do this again, I'll try option (3). 
- Using [Powerline-go](https://github.com/justjanne/powerline-go):
  - First, **install Go**. I did *not* use `apt`, because the available Go version is outdated.
    - Great instructions are in [this blog post from Christos Matskas.](https://cmatskas.com/install-go-on-wsl-ubuntu-from-the-command-line/) (The [instructions from Go's documentation](https://go.dev/doc/install) are incomplete, as they don't tell you how to download the installer from the command line.)
    - Delete the tarball after installing Go.
  - Next, [install Powerline-go](https://github.com/justjanne/powerline-go#installation): 
  ```bash
  go install github.com/justjanne/powerline-go@latest
  ```
  - Then edit your `.bashrc` file per the installation instructions.
    - If you use VS Code, you can open `.bashrc` from the WSL prompt this way:<sup id="a4">[4](#f4)</sup>
    ```bash
    cd ~
    code ./.bashrc
    ```
    - There area lot of command-line options you can set to customize the prompt. I went with:
    ```bash
    function _update_ps1() {
        PS1="$($GOPATH/bin/powerline-go -error $? \
            -jobs $(jobs -p | wc -l) \
            -condensed \
            -newline \
            -truncate-segment-width 10 \
            -cwd-max-depth 4)"
    }
    ```

## <a id="vs-code"></a>VS Code

The terminal in VS Code uses its own fonts, so to get the same pretty prompts inside VS Code that you are getting elsewhere, you need to modify VS Code's `settings.json`:

- In the command palette (`Ctrl-Shift-P`), enter `JSON` and select `Preferences: Open Settings (JSON)`
- In `settings.json`, add an entry for your nerd font:
```json
    "terminal.integrated.fontFamily": "DejaVuSansMono Nerd Font"
```
- If you don't like ligatures, also add this entry:
```json
    "editor.fontLigatures": false
```

---
<sup id="f1">1</sup>There are four different PowerShell profiles, from the possible combinations of these two-element pairs: (`AllUsers, CurrentUsers`) and (`AllHosts, CurrentHost`). 
  - The `AllUsers` profiles are stored in the system Powershell directory; the `CurrentUsers` profiles are stored in the user's PowerShell directory. 
  - The `AllHosts` files are called `profile.ps1.` The `CurrentHost` files are called `Microsoft.PowerShell_profile.ps1`
  - If you use the `$profile` variable to refer to them, you don't really need to know (but can find out) where they are. [↩](#a1)

<sup id="f2">2</sup>VS Code gives you this warning about `fontFace` in `settings.json`: *[deprecated] Define 'face' within the 'font' object instead.* I am not sure what, if anything, to do about this.[↩](#a2)  

<sup id="f3">3</sup>Theoretically, there are two WSLs, WSL1 and WSL2. But as of this writing, WSL2 is the only one that matters. [↩](#a3)  


<sup id="f4">4</sup>I think that `code ~/.bashrc` should also work from anywhere, but I found it unreliable (sometimes it would open my Windows home directory, not my WSL home directory). Explicitly changing to home (`~`) in WSL first was reliable. [↩](#a4)
