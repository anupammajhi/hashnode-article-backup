---
title: "WINDOWS: Red Hat (RHEL9) with WSL2"
datePublished: Sat Jun 11 2022 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clp9qcxzb001009lahexobwvj
slug: windows-red-hat-rhel9-with-wsl2
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1700655789491/a719157b-6562-4ee5-8471-de63cb459395.png
tags: linux, docker, windows, distro, wsl, redhat, rhel9

---

While trying to get **Red Hat (RHEL9)** installed in my WSL (Windows Subsystem for Linux) within my Windows 11 laptop, I stumbled upon the same hurdle I did for installing the older RHEL8. The list of available distros published by Microsoft still does not have Red Hat, for whatever reason.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700641017683/fa385b6d-6e6f-49df-a321-2c879b7fcc76.png align="center")

However, this extremely helpful [blog post](https://wsl.dev/mobyrhel8/) has helped me earlier, I successfully installed RHEL8 in WSL2 about a year back. In this post, I have captured those steps, but for **RHEL9**, so that I can come back to this again when I need it. Along the way, hopefully, this also helps someone looking to do the same.

### **Prerequisites:**

Before diving into the installation process, make sure you have the following prerequisites:

1. **OS:** Windows 10 Pro or above.
    
2. **Enable WSL2:** Ensure that WSL2 is enabled on your Windows machine. You can follow Microsoft's official documentation for guidance.
    
3. **Docker:** To get the Red Hat image from the Red Hat registry.
    
4. **Terminal** \[Optional\]: Windows Terminal or even Powershell terminal would work.
    
5. **Red Hat account**: Create a free developer account to be able to connect to the Red Hat registry.
    
6. Basic knowledge of creating and editing files in Linux.
    

### Prepare RHEL9 Image

The most straightforward method for establishing a new WSL distro involves importing a container export file. To access Red Hat Enterprise Linux (RHEL) container images, it is necessary to retrieve them from Red Hat's dedicated registry.

> Interestingly, a search for RHEL base images yields only RHEL6 and RHEL7 fully featured images. However, when exploring build images, an RHEL9 image is available, such as the one associated with the Go Toolkit.

Let's proceed to create a container using the Go RHEL9 image. It's essential to note that we won't execute any commands within the container, causing it to be created and exit immediately. This behavior aligns with our intention, as we plan to export the container to a file.

Open your Powershell Terminal and login to the Red Hat registry with the username and password for Red Hat developer account. Then we will get the image from the registry.

```bash
# Login to the Red Hat registry
# Enter the Red Hat account username and password
docker login registry.redhat.io

# Pull the GO image with the RHEL9 base image
docker run --name rhel9 registry.redhat.io/rhel9/go-toolset
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700643852609/3821212f-bdfe-4c89-a84f-37af93a1a25d.png align="center")

Once we create the container, we can now export it as an archive file.

> I am using PowerShell. If you are using another distro within wsl2, then you can use the location as /mnt/c/wslsources instead

```powershell
# Create directory to save the archived image
mkdir C:\wslsources

# Export the container to an archive file
# Command help:
# - base command: docker export
# - option to output to a file: -o C:\wslsources\rhel9.tar.gz
# - name of the container to be exported: rhel9
docker export -o C:\wslsources\rhel9.tar.gz rhel9

# Check if the export has been successful
ls -l C:\wslsources
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700644430044/b06297af-9d62-4a30-8c5e-4ede65555ea9.png align="center")

### Import RHEL9 to WSL

We can now use this archived image to import it to WSL as a new WSL2 distro.

```powershell
# Create a new directory for containing the WSL custom distros, or move into it if already created
mkdir c:\wsldistros
cd c:\wsldistros

# Import the container file as a WSL v2 distro
# Command help:
# - base command: wsl --import
# - name of the distro: rhel9
# - location of the distro files: ./rhel9
# - path of the container file: C:\wslsources\rhel9.tar.gz
wsl --import rhel9 ./rhel9 C:\wslsources\rhel9.tar.gz

# Launch the new distro
wsl -d rhel9
```

> *Note: the command above assumes that WSL is set to have the v2 versions by default. If there’s an uncertainty about which version is actually setup, the following option can be added to the command:* `--version 2`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700645162383/ce6ad234-33d6-4610-9e69-010590b0cd56.png align="center")

Check the OS release version to be sure that we are in the right distro.

```bash
# Check with version is installed
cat /etc/os-release
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700645665547/23b76647-e05d-4c84-b8e1-57c204536577.png align="center")

### Finalizing RHEL9 setup

Once we have RHEL, it’s time to start the configuration. But first, let’s register it.

```bash
# Register the system with our Red Hat username and password
subscription-manager register

# Set a role to the System
# It can be one suggested by Red Hat, or in our case, let's have "some fun", and set it to WSL
subscription-manager role --set="Red Hat Enterprise Linux WSL"

# Set a support level
# In our case, remember we have a free Developer license and the only valid support is "Self-Support"
subscription-manager service-level --set="Self-Support"

# Set a usage (read: purpose) to the System
# We will pick "Development/Test" for this first try
subscription-manager usage --set="Development/Test"

# Once everything is set, we can attach the system to our Red Hat subscription
subscription-manager attach
```

`Note: If Simple Content Access (SCA) is enabled in your Red Hat developer account, then the 'attach' would be ignored, just like in my case, and that is okay.`

`[Optional] We can confirm if the system has been correctly subscribed in the Red Hat Customer Portal by clicking in our Subscription and picking the “Systems” tab`

Now we can go ahead and update the packages and install additional packages to make is a full fledged RHEL9 setup.

```bash
# Update the system
dnf update

# Add sudo, zsh and cracklib-dicts
dnf install -y sudo zsh cracklib-dicts
```

*Note:* while `sudo` and `zsh` are familiar applications among Linux users, `cracklib-dicts` is a package utilized by the `passwd` command for password resets. The container image does not inherently include this package. Including `cracklib-dicts` in the container will prevent the occurrence of a warning message each time a password is changed for any user.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700651603722/b61fea0f-8455-4ba6-b43a-d7649466afc8.png align="center")

With packages installed, we can create our main user.

```bash
# Create a new user with the desired username
# Command help:
# base command: useradd
# create the home directory: -m
# set ZSH as the default shell: -s /bin/zsh
# add the user to the group 'wheel' which is defined in the 'sudoers' file: -G wheel
# username desired: majhi
useradd -m -s /bin/zsh -G wheel majhi

# Set a password for the user created
passwd majhi

# Set a password for the 'root' user
passwd
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700651830902/30fde16a-ab33-494e-af85-c920dc47a8d0.png align="center")

In the final phase of configuration, attention is directed towards WSL itself. We will craft a configuration file specifically for the 'interoperability' of our newly customized distribution.

This configuration file resides 'on the WSL side', implying that its effects are limited to the distro it is associated with"

```bash
# Create the configuration file with a preferred text editor
vi /etc/wsl.conf

## This is the file content
## Replace the username and hostname as desired
## Source for the options: https://docs.microsoft.com/en-us/windows/wsl/wsl-config#configure-per-distro-launch-settings-with-wslconf
[automount]
enabled = true
mountfstab = true
root = /mnt/
options = metadata,uid=1000,gid=1000,umask=0022,fmask=11,case=off

[network]
generatehosts = true
generateresolvconf = true
hostname = rhel9

[interop]
enabled = true
appendwindowspath = true

[user]
default = majhi

# [Optional] Check if the file has been correctly created
cat /etc/wsl.conf
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700652223043/e8fed221-9734-4494-b380-2d77760c4046.png align="center")

Now open a new PowerShell terminal to reload the rhel9 from wsl, for the settings to take effect.

```powershell
# Stop the custom distro only from PowerShell 
wsl --terminate rhel9

# Start the distro
wsl -d rhel9
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700653412777/664dcf34-689a-443e-ab41-db794f8d09eb.png align="center")

Now it is logged in as the new main user that we created.

> After this, we can also go ahead and do some cosmetic changes to our zsh shell to make it look pretty and add some theme. I have covered that in another post, the ["zsh" Theme I use](https://tech.anupamm.com/linux-zsh-theme-i-use)

### SystemD setup

When examining the disparities between WSL distros and fully-fledged Linux distributions, one notable absence is the lack of a service manager. This omission is a deliberate result of the customized init process that facilitates broad interoperability.

A new option in the WSL configuration file now enables the execution of a command at the distro's boot time. This modification introduces a significantly cleaner and more efficient method for initiating SystemD.

```bash
# Edit the wsl configuration file with a preferred text editor
sudo vi /etc/wsl.conf

## Add the new boot section with the command to start SystemD at the end of the file
## Source: https://github.com/diddlesnaps/one-script-wsl2-systemd/tree/build-21286%2B
[boot]
command = "/usr/bin/env -i /usr/bin/unshare --fork --mount-proc --pid -- sh -c 'mount -t binfmt_misc binfmt_misc /proc/sys/fs/binfmt_misc; exec /usr/lib/systemd/systemd --unit=multi-user.target'"

# [Optional] Check if the change has been successfully saved
cat /etc/wsl.conf
```

Then we will need to create a script to be launched with the distro starts:

```bash
# Create the script to be launched when the distro starts
sudo vi /etc/profile.d/00-wsl2-systemd.sh

## The file should have the following content
if [ "$(ps -c -p 1 -o command=)" != "systemd" ]; then
        if [ -z "$SUDO_USER" ]; then
                export > "$HOME/.profile-systemd"
        fi

        if [ "$USER" != "root" ]; then
                exec sudo /bin/sh "/etc/profile.d/00-wsl2-systemd.sh"
        fi

        if ! grep -q WSL_INTEROP /etc/environment; then
                echo "WSL_INTEROP='/run/WSL/$(ls -r /run/WSL | head -n1)'" >> /etc/environment
        fi

        if ! grep -q DISPLAY /etc/environment; then
                echo "DISPLAY='$(awk '/nameserver/ { print $2":0" }' /etc/resolv.conf)'" >> /etc/environment
        fi

        exec /usr/bin/nsenter --mount --pid --target "$(ps -C systemd -o pid= | head -n1)" -- su - "$SUDO_USER"
fi

if [ -f "$HOME/.profile-systemd" ]; then
        source "$HOME/.profile-systemd"
fi

if [ -d "$HOME/.wslprofile.d" ]; then
        for script in "$HOME/.wslprofile.d/"*; do
                source "$script"
        done
        unset script
fi

# [Optional] Check if the file has been correctly saved
cat /etc/profile.d/00-wsl2-systemd.sh
```

Finally, we will need to update the sudoers file so that this can be launched without using a password every time the distro starts.

```bash
# Edit the sudoers file
sudo visudo

## Add a new value to the environment variables that are kept
Defaults env_keep += "WSL_INTEROP"

## Add the command to run the script without password
%wheel ALL=(root) NOPASSWD: /bin/sh /etc/profile.d/00-wsl2-systemd.sh

# [Optional] Check if the sudoers have been correctly updated
sudo grep -i wsl /etc/sudoers
```

Then we just reload the distro for the settings to take effect.

```powershell
# Stop the custom distro only from PowerShell 
wsl --terminate rhel9

# Start the distro
wsl -d rhel9
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700653412777/664dcf34-689a-443e-ab41-db794f8d09eb.png align="center")

To confirm after launching a new session of our distro and check SystemD processes are running, we can check for PID 1:

```bash
ps -ef
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700654587102/e45af9d6-e799-437c-8db2-4975bdb069da.png align="center")

### Setup Docker

This RHEL9 setup still doesn't have any docker client or daemon. However, we can use the following steps as per Docker documentation.

```bash
# Install the yum-utils package
sudo yum install -y yum-utils

# Add the Docker repository
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700654793638/d1e891ff-8ad3-4fc0-90e9-42f2430d32f5.png align="center")

```bash
# Install Docker
sudo yum install docker-ce docker-ce-cli containerd.io

## We will need approve the import of the Docker GPG key
## Just type 'y' and press Enter
```

Before initiating Docker, a necessary modification is required in the Docker SystemD file. The challenge arises from the fact that Red Hat no longer utilizes iptables but has transitioned to its successor, nftables.

Fortunately, we can address this by adjusting the Docker start command to eliminate the reliance on searching for iptables.

```bash
# Update the Docker SystemD service
sudo vi /usr/lib/systemd/system/docker.service

## Locate the ExecStart option and add --iptables=false at the end of the line
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock --iptables=false

# [Optional] Check if the file has been correctly saved
grep ExecStart /usr/lib/systemd/system/docker.service

# Reload the SystemD service configuration files
sudo systemctl daemon-reload

# Start Docker
sudo systemctl start docker

# Check if Docker has been correctly started
sudo systemctl status docker

# Enable Docker to be started when the distro starts
sudo systemctl enable docker
```

In order for us to use Docker without the `sudo` command, we need to add our user to the `docker` group

```bash
# Add our user to the docker group
sudo usermod -aG docker $USER

# Refresh the group docker without launching a new shell
newgrp docker

# Check the Docker version
docker version
```

That's it! We now have a full-blown RHEL 9 distro running in WSL with necessary WSL interoperability settings.

What an enjoyable journey, and if there's one key takeaway from this blog, it's that WSL2 serves as a robust platform for running Linux. Thanks to

some clever scripting, the experience is steadily approaching that of a virtual machine (VM) or bare-metal setup.

It's crucial to note, however, that achieving official support might not always be feasible, introducing potential limitations and roadblocks. Nevertheless, from a technical standpoint, it was entirely achievable.