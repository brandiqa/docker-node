# Example Docker Node.js Project

The full tutorial is available on [sitepoint.com/docker-windows-10-home](https://www.sitepoint.com/docker-windows-10-home). Please **check this README for updated instructions** .

The app is a plain React application bundled with [Parcel](https://parceljs.org/).

## Prerequisites

Use **PowerShell with admin privileges** to perform all installation instructions using `choco`. Use Git Bash to run all Docker and Node.js commands without admin privileges.

- [Chocolatey Package Manager](https://chocolatey.org/)
- [Node.js](https://nodejs.org/en/) LTS version. Install via [nvm for Windows](https://github.com/coreybutler/nvm-windows)
- [VirtualBox 6.0](https://www.virtualbox.org/wiki/Download_Old_Builds_6_0) - newer versions have a bug
- [Git Bash](https://gitforwindows.org/)

Once you've installed choco, you can install Docker tools as follows:

```powershell
choco install docker-machine docker-cli docker-compose -y
```

## UPDATE: May 2020

Please use [VirtualBox 6.0](https://www.virtualbox.org/wiki/Download_Old_Builds_6_0). VirtualBox 6.1.* has a bug that prevents creation of `docker-machine` instances. After installing VirtualBox and Node.js, use Git Bash to create a new `docker-machine` instance:

```bash
# Create instance
docker-machine create --driver virtualbox default
# Stop instance
docker-machine stop default
```

Open VirtualBox and find `default` VM instance. Open the settings and adjust as follows:

- **Display Tab**: Increase memory to 16MB (simply gets rid of the warning)
- **Shared Folders**: Docker will properly mount any project under `C:\Users` directory. To add other directories, simply add a new Shared Folder like this:

![shared-folder-settings](https://snipboard.io/PFdljQ.jpg)

In the above example, I have created the folder `c:\docker`. In order to mount projects located inside this path in Docker, you MUST specify the mount path: `/c/docker`. Otherwise you'll run into 'NOT FOUND' errors when you try to create a container. If you want to put your projects on another partition such as the `D:\` drive, the mount path will be `/d/`.

- **Network Tab**: Specify Port Forwarding rules in order to expose application ports.

![port-forwarding-rules](https://snipboard.io/A15poE.jpg)

Start `default` VM instance and confirm the Shared Folders have been mounted on the correct paths:

```bash
# Start docker machine instance
docker-machine start default

# ssh into the instance
docker-machine ssh default

## Check mount
mount
....
# output should include....
/c/Users on /c/Users type vboxsf (rw,nodev,relatime)                      
/c/docker on /c/docker type vboxsf (rw,nodev,relatime) 
...


cd /c/
```

You should expect the following output:

![docker-confirm-mount](https://snipboard.io/IUBo3H.jpg)

If you can't access your folder from correct mount path, Docker won't be able to access local files during container creation.

## License

MIT License

Copyright (c) 2019 Michael Wanyoike

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
