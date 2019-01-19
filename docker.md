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
