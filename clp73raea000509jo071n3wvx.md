---
title: "LINUX: "zsh" Theme I use"
datePublished: Tue Mar 23 2021 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clp73raea000509jo071n3wvx
slug: linux-zsh-theme-i-use
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1700496493427/e755b6f5-25be-4e60-9b7d-28b558ba7f90.png
tags: linux, zsh, shell, theme, redhat

---

While trying to setup RHEL with WSL 2, I stumbled upon this very helpful blog [https://wsl.dev/mobyrhel8/](https://wsl.dev/mobyrhel8/)

While doing the setup, I found this zsh theme very nice, and I have been using it since then. The above mentioned blog already has the steps, but it has a lot of other info about setting up RHEL in WSL2 as well. So here is the extract of setting up the zsh theme (steps are for RHEL8, might differ for other Linux dists).

### 1\. Setup ZShell

If the zsh is not already installed, then install zsh.

```bash
dnf install -y sudo zsh cracklib-dicts
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700491369801/6926f33d-b7f0-4306-95a7-959133a7c56a.png align="left")

### 2\. Install "Oh My Zsh"

Oh My Zsh is the configuration manager being used for this

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700491537525/f69f4495-a902-4d65-b3b8-570715ec03d0.png align="left")

### 3\. Update user to use zsh as the default shell

Find the location of zsh installation

```bash
cat /etc/shells
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700494830943/d3af91ea-bbe7-47c0-81b0-7734047b177a.png align="left")

Use vi editor to update the default shell of the user to zsh by replacing the path `(example: ec2-user is the user we are working with)`

```bash
sudo vi /etc/passwd
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700494426183/4ba5557c-1f3d-4be8-a8c7-5848db04cc8c.png align="left")

### 4\. Update zsh config file

As written in the installation message, we can now change the zsh config file. For now, we will only change the theme to Fino Theme.

```bash
vi $HOME/.zshrc
```

In the opened vi editor, look for the key ZSH\_THEME and update it as below:

```bash
ZSH_THEME="fino-time"
```

Check if the config file got updated, and then load the new config file.

```bash
# [Optional] Check if the theme value has been correctly saved
grep ZSH_THEME $HOME/.zshrc

# Reload the zsh configuration file
source $HOME/.zshrc
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700495262851/38e34af7-1e9c-42c0-a827-64c7e9a412a0.png align="left")

### 5\. Update theme settings

Open the theme settings in vi editor

```bash
vi $HOME/.oh-my-zsh/themes/fino-time.zsh-theme
```

Look for the PROMPT variable and modify it to add the variables

```bash
PROMPT="╭─%{$FG[040]%}%n%{$reset_color%} %{$FG[239]%}at%{$reset_color%} %{$FG[033]%}$(box_name) (WSL: $WSL_DISTRO_NAME | kernel: $(uname -r))%{$reset_color%} %{$FG[239]%}in%{$reset_color%} %{$terminfo[bold]$FG[226]%}%~%{$reset_color%}\$(git_prompt_info)\$(ruby_prompt_info)
╰─ %* \$(virtualenv_info)\$(prompt_char) "
```

Reload the zsh configuration file, it will also reload the oh my zsh theme

```bash
source $HOME/.zshrc
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700495669171/812404c2-f8c7-4bd4-aeb8-33f3e2db7c0f.png align="center")

That's it, now we have a nice-looking theme that gives a lot more information without being too overwhelming. It has a nice clock in the prompt too.