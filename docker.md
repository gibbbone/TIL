# Docker
This tips regards mostly Docker on Windows 10 via WSL (yeah, the edgiest of the edge cases).

## Installation
This is a mess. I actually don't know which of the steps is really required because there seem to be quite a bit of confusion on the right procedure. Think of this like ingredients for a magic potion: 

>"I don't really need to put the lizard head separated from the body, right?" 

>"Who cares! As long as the potion works..." 

>\* potion lab explodes \*

The steps that seem to work for me at the moment are the following (these are just to remember them, you may need some adjustments):

1. Check your system:
    - Windows version: Docker is available for Windows 10 Professional but not for Windows home. If you have Home Edition Docker is available through docker-toolbox. 
    - Virtualization: your system must be virtualization compatible and virtualization must be enabled (check in Task    Manager>performance>CPU). Another place where to look maybe System Info.
    - Slightly edge case: your BIOS could have virtualization disabled for security reason. Check that it doesn't, if so enable virtualization. 
1. Remove any previous installation of docker-toolbox and virtual machine: it may be the case, like mine, that you tried many times the same procedure. You must ensure that you do a clean install this time.
    - Uninstall via the Windows interface as usual
    - Remove the two folder `.docker` `.VirtualBox` in `C:\Users\<username>`
1. Install docker-toolbox with administration privileges (right click on the .exe files and chose it from the option)
    - Remember to check the box "install VirtualBox with the NDIS5 driver." which by default is unchecked. Quoting [the official guide](https://docs.docker.com/toolbox/toolbox_install_windows/) by Docker: 
    > A Windows specific problem you might encounter relates to the NDIS6 host network filter driver, which is known to cause issues on some Windows versions. For Windows Vista systems and newer, VirtualBox installs NDIS6 driver by default. Issues can range from system slowdowns to networking problems for the virtual machine (VM). If you notice problems, re-run the Docker Toolbox installer, and select the option to install VirtualBox with the NDIS5 driver.
1. Update the virtual machine to the last version available. (This seemed the fundamental step in my procedure).
    - Open VirtualBox and check updates. If there are download them and install them.        
1. This may be stupid: run the virtual machine *before* the next step. 
1. Launch the Docker Quickstart Terminal. This should jumpstart the first configuration of Docker which should create anything necessary to it to run. It's here that usually scary error messages appear. Brace yourselves.
1. Check your installation by typing in the Docker Quickstart Terminal: 
    ```
    docker run hello-world
    ```
    This should check that your installation is working properly.
 
## Configuration
Configuration is needed because we want Docker to work inside WSL to mimick a "real" Docker experience. This will allow us to avoid using the Docker Terminal.

The nice thing about the WSL is that it allows us to deal with Windows executable without any problem. Try running the following (it should output the default container created with automatic configuration procedure):
```bash
docker-machine.exe ls
```

### Environment variables
Docker comes with an handy command (`docker-machine.exe env`) which outputs a list of command line commands to set up environment variables for future use. Since we are running Docker on Windows the default output format would be for `cmd` terminal. 

We can switch to `bash` via the `--shell` argument:
```bash
docker-machine.exe env --shell sh
```
The output of this command would be a list of instructions to set bash environment variables (hence with `export` instead of `SET` and so on). To actually execute the instructions we need to `eval` them:
```bash
eval $(docker-machine.exe env --shell sh)
```
After this you can check that environment variables are set:
```bash
env | grep DOCKER
```
You will notice though that paths are in the wrong format. To deal you can either do it manually or use a WSL utility called `wslpath` which converts path from Windows to Linux format automatically. However you may not have the right version of WSL (remember? we need to have the edgiest edge case), since this feature has been added afterward. In this case you can use an ad hoc [Python program](https://github.com/lamyj/wsl-path-converter) `wpc`(notice that at the time I'm writing the only install option working is the one via git clone). The final command to launch is:
```bash
export DOCKER_CERT_PATH=$(wpc $DOCKER_CERT_PATH)
```

Since these environment variables will be forgotten once we restart bash let's save them for good:
```bash
echo "eval $(docker-machine.exe env --shell sh)" >> ~/.bashrc && source ~/.bashrc
echo "export DOCKER_CERT_PATH=$(wpc $DOCKER_CERT_PATH)" >> ~/.bashrc && source ~/.bashrc
```

### Install Docker in WSL
To do this I followed almost step-by-step [this guide](https://nickjanetakis.com/blog/setting-up-docker-for-windows-and-wsl-to-work-flawlessly) from  the"Install Docker and Docker Compose within WSL" to the "Ensure Volume Mounts Work" section:

#### Installation
```bash
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
```
Here I got stuck until I did the following (after checking [this](https://github.com/Microsoft/WSL/issues/640) discussion on Github):
```
sudo chmod 777 /var/cache/app-info/xapian/default -R
```
The fix worked for me but be aware that in the discussion someone says that is not a safe operation (it was too late for me). After that I proceeded as in the guide:
```bash
# Update the apt package list (for the new apt repo).
sudo apt-get update -y

# Install the latest version of Docker CE.
sudo apt-get install -y docker-ce

# Allow your user to access the Docker CLI without needing root access.
sudo usermod -aG docker $USER
```
Restart bash after this and install Python and pip if you don't have them already (unlike me). Then:
```bash
# Install Docker Compose into your user's home directory.
pip install --user docker-compose
```
#### Configuration
Here's another step where I didn't followed the guide: all my env variables where settled with `docker-machine.exe env` and hence I didn't want to fiddle with them.

#### Verification
This should check that you can access Docker via bash in the WSL:
```bash
# You should get a bunch of output about your Docker daemon.
# If you get a permission denied error, close + open your terminal and try again.
docker info

# You should get back your Docker Compose version.
docker-compose --version
```
