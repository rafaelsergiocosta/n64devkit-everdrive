# N64 Homebrew Development Kit Setup

Hi all, hopefully your looking to do homebrew N64 development using the Libdragon library.  I've assembled here setup instructions / scripts for Windows (WIP) and Ubuntu.  I'm also working on a YouTube series on the basics of working with Libdragon (WIP).

# Examples 

Listed below is the source code for Libdragon apps contained in this repo:

| Name / ROM  	| Screenshot | Example Covers | Link   	
|---	|---	     |---   |---
| [hello_world.z64](https://github.com/werkn/n64devkit-everdrive/raw/main/N64DevKit/homebrew/0_hello_world/hello_world.z64)  	| ![hello_world](https://raw.githubusercontent.com/werkn/n64devkit-everdrive/main/N64DevKit/homebrew/0_hello_world/screenshot.png)  	| basic game loop, initialization  	     | [hello_world](https://github.com/werkn/n64devkit-everdrive/tree/main/N64DevKit/homebrew/0_hello_world)
| [n64paint.z64](https://github.com/werkn/n64devkit-everdrive/raw/main/N64DevKit/homebrew/1_n64_paint/end/n64_paint.z64)  	| ![n64_paint](https://raw.githubusercontent.com/werkn/n64devkit-everdrive/main/N64DevKit/homebrew/1_n64_paint/end/screenshot.png)  	| sprite loading, input, animation, timers, rumble, multiple controllers  	     | [n64paint](https://github.com/werkn/n64devkit-everdrive/tree/main/N64DevKit/homebrew/1_n64_paint)
| 2droguelike.z64  	| ![2droguelike](https://raw.githubusercontent.com/werkn/n64devkit-everdrive/main/res/images/wip.png)  	| sprite loading, input, animation, timers, rumble, multiple controllers, procedural generation, audio  	     | 2droguelike
| twinstick.z64  	| ![twinstick](https://raw.githubusercontent.com/werkn/n64devkit-everdrive/main/res/images/wip.png)  	| dual stick using two controllers, audio  	     | twinstick

# Ubuntu 20.04 Setup

[WIP / YouTube Playlist](https://youtube.com/playlist?list=PLtQmMQGpPR6I8mpCemyRqRbxpwHKYsXYt)

Environment setup is there, although currently its all done manually.  Could be scripted and I'm open to pull requests on this repo if someone wants to do that.

# Windows 10 Setup

Note:  I've moved away from using Windows and to Ubuntu 20.04 for development.  As of Dec 25 2020 these instructions still work but may not work in the future.

The following instructions will help you to setup a Docker container based N64 DevKit allowing you to deploy to actual N64 hardware using the open-source Libdragon library.  These instructions are for Windows 10 but could be easily ported to Linux or Mac OS.  There is an open-source .NET Core variant of the Everdrive ROM deployment on the official Krikkz Github repo.

## Requirements

You'll require the following to be installed before continuing:
 - Docker-Desktop (and basic Docker knowledge)
 - VSCode + ms-vscode-remote.remote-containers plugin
 - Git / Github Desktop
 - Flash cart (I'm using the [Everdrive 64 X7](https://krikzz.com/store/home/55-everdrive-64-x7.html))

If you don't have these installed you can use the script below to install these dependencies using Chocolatey.

```powershell
#install chocolatey
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

#install vscode
choco install vscode

#install docker-desktop
choco install docker-desktop

#install github-desktop
choco install github-desktop

#install git-scm
choco install git

Write-Host -ForegroundColor Green "Log out and back into for docker-users group to be created."
Write-Host -ForegroundColor Green "Allow Docker-Desktop to start and configure for WSL2 before moving to next step"
```

1.  From a PowerShell admin terminal run the following: 

```powershell
#add remote container plugin, you'll need to re-open your shell
code --install-extension ms-vscode-remote.remote-containers

git clone https://github.com/werkn/n64devkit-everdrive
cd N64devkit-everdrive\N64DevKit
.\Install-N64DevKit.ps1  
```
2.  After the script completes you can close the shell.

## Development within Container

We can now develop homebrew apps for the N64 using the Docker container we just setup.  The easiest way to do this is to use the VSCode Remote Containers plugin.

To connect to your container (after installing `ms-vscode-remote.remote-containers` plugin to VSCode) perform the following:

1.  From the VSCode sidebar click `Remote Explorer`, expand `Dev Containers`.
2.  Right click `N64DevKit->Attach to Container`.
3.  A new window will launch, after the server has set itself up press `Ctrl+Shift+E` to open the Explorer, click `Open Folder` enter `/libdragon/homebrew` in the dialogue that pops up.
4.  You should know see the folder `0_hello_world` listed, open the directory and edit `hello_world.c`.

You should now be connected into the running container, you can now perform edits on `hello_world.c`.

## Compiling and Deploying a ROM

Begin by connecting your Everdrive 64 X7 to your N64 and be sure to connect the Micro-USB cable to the USB port found on the flash cart.

Next, to make a ROM, from within the VSCode remote container instance, perform the following:

1.  Open a new shell and navigate into `/libdragon/homebrew/0_hello_world`.
2.  Run `make clean & make all`.
3.  After doing this a new ROM will be deployed to `./homebrew/deploy.z64`.
4.  From the host system (not in the container shell) launch Powershell and run `ed64`.  This will automatically deploy the created ROM over USB to the Everdrive and will run on your system.

Congratulations if you made it this far, you can now look into the library we are using for homebrew, [Libdragon](https://github.com/DragonMinded/libdragon), docs can be found at that link or internal to our container at path `/libdragon`.  Check back here periodically here as I may add a few tutorials dealing with sprites, audio, and controller input.




