# An Introduction to Windows Package Management and Software Automation

## What is Chocolatey?
Managing Windows software is time-consuming.  Upgrading across our organization is time and cost exhaustive, and can get increasingly complex. Chocolatey allows us to write something once, deploy to any of our machines anywhere and allow us to track and manage versions and installations. It’s essentially a package manager for Windows.

A package manager or package-management system is a collection of software tools that automates the process of installing, upgrading, configuring, and removing computer programs for a computer in a consistent manner.

Chocolatey has the largest online registry of Windows packages. Windows 11 will have WinGet and it is currently installable on Windows 10.  There are only 300 vendors and packages available for it.  Chocolatey has over 9000+ packages available publicly (and growing).  It also allows us to setup our own internal Chocolatey Server and deploy packages locally for our own software if we ever wanted to.

From a software compliance perspective, if we’re using the community repository – they have a strict policy on malicious and piracy software, and all packages go through a compliance check before being publicly available.

## How do we Install Chocolatey?
Chocolatey has several ways to be deployed using different automation systems.  However, we don’t have any of them setup in our organization (ansible, chef, puppet, etc).  We can install it individually on each machine using a one-liner powershell command.  This command will need to be ran via an Administrative prompt.
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```
If we have Windows Remote Management installed on all devices on the network, we’ll be able to install this across all devices connected to our network.  We can run a script to automatically install this via the domain controller, and keep track of those who weren’t successfully installed.  Installing Windows Remote Management can be done via this link: https://docs.microsoft.com/en-us/windows/win32/winrm/installation-and-configuration-for-windows-remote-management
How do we use Chocolatey?
In order to install something via Chocolatey, we’ll need to have an Admin privileges in a Powershell prompt. This can be done via TeamViewer and starting a powershell prompt in Admin mode via the GUI.  However, if we have WinRM installed, we can remote into the machine by running the following command from your own local powershell terminal:  `Enter-PsSession <COMPUTERNAME>`

This will establish a remote command line session with the computer, and you’ll be able to run your commands from there.
 
## Finding packages
There are a couple of ways to find software packages to install.  One way is to find the package on Chocolatey.org’s website: https://community.chocolatey.org/packages - the command to install the package will be shown:
 
You can also search for packages on the command line by using the command: `choco search <KEYWORD>`
 
Find the package you want to install, and use the package name displayed when you install.
 
## Installing packages
To install a package is simple.  Simply type `choco install <PACKAGENAME>`. Multiple packages can be installed at once by putting them one after another, separated by spaces: `choco install <PACKAGENAME> <PACKAGENAME> <PACKAGENAME>`.

For each package being installed, you will be asked to enter `Y` for yes, `A` for accept all prompts, or `N` to backout of the installation process. 
 
This can be time consuming – so, to install everything and accept all terms ahead of time you can use `choco install <PACKAGENAME> --confirm` and the installation will happen all at once, without prompts.

## Upgrading packages
To upgrade a package already installed with chocolatey: `choco upgrade <PACKAGENAME>`.  You’ll still be asked to confirm you want to run the script and agree to the license terms.  You can use the `--confirm` flag to bypass this prompt:  `choco upgrade <PACKAGENAME> --confirm`.
If you would like to upgrade EVERYTHING on the system at once you can use `choco upgrade all --confirm`.
 

 
## Removing packages
If there is software you wish to uninstall, you can use the command `choco uninstall <PACKAGENAME>`. The `--confirm` flag or `-y` can be used here too to automatically do this.
 

## How do we know what’s Installed?
It can be hard to determine what’s installed on a system with chocolatey, in order for us to upgrade.  You can use the choco list command with the `--local-only` flag added on.
 
 
## Getting Help with Chocolatey syntax
Chocolatey has plenty of documentation online if you want help on anything to do with the system at https://docs.chocolatey.org/ 
If you want immediate help on the command line when dealing with syntax, use `choco /?`
 
## Notes:
Can’t upgrade with software not installed with chocolatey – software must be uninstalled first, and then installed with chocolatey.

