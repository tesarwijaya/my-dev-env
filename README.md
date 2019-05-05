# HOW TO SETUP MY WINDOWS DEVELOPMENT ENVIRONMENT

## List of Contents
- [Windows Subsystem Linux (WSL) Install Guide](https://github.com/tesarwijaya/my-dev-env#1-windows-subsystem-linux-wsl-install-guide)
- [Setup Docker for windows with WSL](https://github.com/tesarwijaya/my-dev-env#2-setup-docker-for-windows-with-wsl)
- [Useful Daily App](https://github.com/tesarwijaya/my-dev-env#useful-app-daily-app)
- [Cheat Sheet](https://github.com/tesarwijaya/my-dev-env#cheat-sheet)


## Windows Subsystem Linux (WSL) Install Guide
### Install the Windows Subsystem for Linux
Before installing any Linux distros for WSL, you must ensure that the "Windows Subsystem for Linux" optional feature is enabled:

- Open PowerShell as Administrator and run:
```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```
- Restart your computer when prompted.
- Install your Linux Distribution of Choice from the Windows Store

Now that your Linux distro is installed, you must initialize your new distro instance once, before it can be used.

[source](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

## Setup Docker for windows with WSL
First, download docker for windows from [here](https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe) and install it.

After successfully installing docker, go to docker configuration In the general settings, we want to expose the daemon without TLS. don't forget to set shared drive settings too!

### Install Docker and Docker Compose within WSL
follow this instruction
```
# Update the apt package list.
sudo apt-get update -y

# Install Docker's package dependencies.
sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

# Download and add Docker's official public PGP key.
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Verify the fingerprint.
sudo apt-key fingerprint 0EBFCD88

# Add the `stable` channel's Docker upstream repository.
#
# If you want to live on the edge, you can change "stable" below to "test" or
# "nightly". I highly recommend sticking with stable!
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

# Update the apt package list (for the new apt repo).
sudo apt-get update -y

# Install the latest version of Docker CE.
sudo apt-get install -y docker-ce

# Allow your user to access the Docker CLI without needing root access.
sudo usermod -aG docker $USER
```

### Install Docker Compose
We’re going to install Docker Compose using PIP instead of the pre-compiled binary on GitHub because it runs a little bit faster (both are still Python apps).

```
# Install Python and PIP.
sudo apt-get install -y python python-pip

# Install Docker Compose into your user's home directory.
pip install --user docker-compose
```

### The next step is to make sure `$HOME/.local/bin` is set on your WSL `$PATH`.

You can check if it’s already set by running `echo $PATH`. Depending on what WSL distro you use, you may or may not see `/home/<<USERNAME>>/.local/bin`

If it’s there, you’re good to go and can skip to the next sectio

If it’s not there, you’ll want to add it to your `$PATH`. You can do that by opening up your profile file with `vim ~/.profile`. Then anywhere in the file, on a new line, add `export PATH="$PATH:$HOME/.local/bin"` and save the file.

Finally, run `source ~/.profile` to active your new `$PATH` and confirm it works by running `echo $PATH`. You should see it there now. Done!

### Connect to a remote Docker daemon

```
echo "export DOCKER_HOST=tcp://localhost:2375" >> ~/.bashrc && source ~/.bashrc
```
That just adds the export line to your .bashrc file so it’s available every time you open your terminal. The source commands reloads your bash configuration so you don’t have to open a new terminal right now for it to take effect.

### Ensure Volume Mounts Work

run `sudo nano /etc/wsl.conf` to create and modify the new WSL configuration file:
```
# mount drives to /c and /d instead of /mnt/c and /mnt/d
[automount]
root = /
options = "metadata"
```

We need to set root = / because this will make your drives mounted at /c or /e instead of /mnt/c or /mnt/e.

### Verify if everything works
```
# run docker
docker --version

# run docker-compose
docker-compose --version

```

[source](https://nickjanetakis.com/blog/setting-up-docker-for-windows-and-wsl-to-work-flawlessly)

## Useful Daily App
- [Git](https://git-scm.com/)
- [Git Flow](https://github.com/nvie/gitflow/wiki/Installation)


## Cheat Sheet
All common command that i might forgot in the future

### Docker
```
# delete all dangling images
docker rmi $(docker images -q -f "dangling=true")
```

### Tmux
```
# new session
tmux

# Split pane vertically
ctrl + b %

# Split pane horizontally
ctrl + b "

# Close current pane
ctrl + b x
```