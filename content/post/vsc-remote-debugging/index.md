---
title: Remote Development and Debugging with Visual Studio Code
date: 2020-03-28
#math: true
#diagram: true
image:
  placement: 3
  caption: 'Image credit: [stock.xchng](https://www.freeimages.com/photo/help-1192586)'
---

Due to COVID-19, many of us are now working from home and are away from our dev machines. In my case, I know that I cannot work efficiently through a laggy VNC conneciton. While I could do most things through SSH and command line, debugging can be problematic.

A while ago I came across the [Visual Studio Code Remote Development](https://code.visualstudio.com/docs/remote/remote-overview) extension pack. This extension is new, so the documentation for it is not perfect, especially when your target machine is a Mac. I had a hard time finding solutions to the multiple problems I faced when setting this up, so I decided to write it up in a blog post. 

Below are the steps needed for making VSC debugging work for a **C++ project** when on a **Windows host** connecting to a **remote Mac**. This even works when connecting to a **remote VM**! I imagine only slight modifications will be needed for other scenarios. The instructions assume you already have a C++ project setup for VSC on your remote machine.

## Setting up Remote Development

* Locally, on your host, install VSC and the [remote development extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack).
* Open VSC, go to File → Preferences → Settings
  * Search for `remote.SSH.showLoginTerminal`. Enable this setting.
  * Search for `remote.SSH.useLocalServer`. Disable this setting.
* Now, press F1, search for and select the following: "Remote-SSH: Connect to Host..."
  * Enter <your_username>@<your_remote_machine_ip>
  * The VSC terminal will prompt for your VM's password
  
After you enter your password you are connected to your VM and can browse/modify the files there as you would locally! The experience is much smoother than VNC or RDP.
  
## Setting up Remote Debugging
To debug remotely, we will need a few more steps. 
* While connected to your VM, we will need to install the following VSC extensions (this will install them on your VM and not your host):
  * [C++ extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
  * [CodeLLDB extension](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb)
* Turn on developer mode on your remote machine: `$ sudo /usr/sbin/DevToolsSecurity --enable`
* Add the following configuration to the `.vscode/launch.json` of your project:
```json
{
  "name": "Launch (Remote)",
  "type": "lldb",
  "request": "launch",
  "program": "/path/to/your/program" // e.g. ${workspaceFolder}/a.out
}
```
* Go to the debugging tab of VSC, select the "Launch (Remote)" configuration, and F5 to debug!
* If you need to attach to a process, use the following configuration to `.vscode/launch.json`:
```json
{
  "name": "Attach (Remote)",
  "type": "lldb",
  "request": "attach",
  "program": "/path/to/your/program", // e.g. ${workspaceFolder}/a.out
  "pid": "${command:pickProcess}"
}
```
* Using the "Attach (Remote)", after you press F5 a prompt will open where you can search for the process to attach to.

Hope you found this guide helpful. Happy debugging and stay safe out there!
