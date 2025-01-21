# How to install docker
## Windows Server 2019
### Steps to Install Docker
1. Enable Windows Containers Feature
  Windows Server 2019 includes the ability to run Windows Containers natively, but you must first enable the Windows Containers feature.
  * Open Powershell as Administrator
  * Run the following command to enable Windows Containers
  ``` powershell
  Install-WindowsFeature -Name Containers
  ```
2. Enable Hyper-V Feature
  Docker requires the Hyper-V feature to run containers on Windows Server. You need to enable Hyper-V if it isn't already enabled.
  
  * Run the following command to enable Hyper-V:
  ``` powershell
  Install-WindowsFeature -Name Hyper-V -IncludeManagementTools
  ```
3. Install Docker using Powershell
  Windows Server 2019 supports Docker Enterprise Edition (EE), and you can install it directly via PowerShell.

  * First, download and install the DockerMsftProvider module:
    ``` powershell
    Install-Module -Name DockerMsftProvider -Force -Scope CurrentUser
    ```
  * Next, install Docker by running the following command:
    ``` powershell
    Install-Package -Name docker -ProviderName DockerMsftProvider
    ```
 if you have got the following error, you can try with [here](https://learn.microsoft.com/en-us/virtualization/windowscontainers/quick-start/set-up-environment?tabs=dockerce#windows-server-1)
 ```
WARNING: The property '' cannot be found on this object. Verify that the property exists.
WARNING: Cannot bind argument to parameter 'Channels' because it is null.
WARNING: You must specify an object for the Get-Member cmdlet.
WARNING: The property 'versions' cannot be found on this object. Verify that the property exists.
WARNING: The property 'channels' cannot be found on this object. Verify that the property exists.
WARNING: Invalid JSON primitive: .
Install-Package : No match was found for the specified search criteria and package name 'docker'. Try
Get-PackageSource to see all available registered package sources.
At line:1 char:1
+ Install-Package -Name docker -ProviderName DockerMsftProvider
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (Microsoft.Power....InstallPackage:InstallPackage) [Install-Package], Ex
   ception
    + FullyQualifiedErrorId : NoMatchFoundForCriteria,Microsoft.PowerShell.PackageManagement.Cmdlets.InstallPackage
```

4. Start Docker Service
After installing Docker, you need to start the Docker service.

* Run the following command to start Docker:
``` powershell
Start-Service docker
```
5. Set Docker to start Automatically
To ensure Docker starts automatically when the system boots, use the following command:
``` powhershell
Set-Service -Name docker -StartupType Automatic
```
6. Verify Docker Installation
To verify that Docker has been successfully installed and is running, you can check the Docker version by running:
``` powershell
docker --version
```
You can also run a test container to ensure everything is working:
``` powershell
docker run hello-world
```
This will download and run the hello-world Docker image and output a confirmation message if everything is working correctly.
### Steps to Install Docker Compose
1. Download Docker Compose:
Open PowerShell as Administrator and run the following command to download Docker Compose for Windows:
``` powershell
Invoke-WebRequest -Uri https://github.com/docker/compose/releases/download/v2.21.0/docker-compose-Windows-x86_64.exe -OutFile "$env:ProgramFiles\Docker\docker-compose.exe"
```
This will download the latest version of Docker Compose (as of now, v2.21.0) to the appropriate folder.
2. Verify Installation:

After downloading, verify that Docker Compose was installed successfully by checking its version:
``` powershell
docker-compose --version
```
If installed correctly, this command should return the Docker Compose version, for example:
```
docker-compose version 2.21.0, build 8c6e1d7
```
3. (Optional) Add to PATH if necessary:
If you run into issues where the docker-compose command is not recognized, it might be because the installation directory is not in your system’s PATH.
You can add $env:ProgramFiles\Docker to the system PATH environment variable by running:
``` powershell
$env:Path += ";$env:ProgramFiles\Docker"
```
Or you can manually add the path to the environment variables through System Properties.
4. Test Docker Compose:
Run the following to check if Docker Compose works:
``` powershell
docker compose version
```
The command should return the version of Docker Compose, confirming that it is properly installed.

If you have got the following error:
```
Invoke-WebRequest -Uri https://github.com/docker/compose/releases/download/v2.21.0/docker-compose-Windows-x86_64.exe -OutFile "$env:ProgramFiles\Docker\docker-compose.exe"
Invoke-WebRequest : Could not find a part of the path 'C:\Program Files\Docker\docker-compose.exe'.
At line:1 char:1
+ Invoke-WebRequest -Uri https://github.com/docker/compose/releases/dow ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [Invoke-WebRequest], DirectoryNotFoundException
    + FullyQualifiedErrorId : System.IO.DirectoryNotFoundException,Microsoft.PowerShell.Commands.InvokeWebRequestComma
   nd
```
The error you're seeing indicates that PowerShell doesn't have access to the specified directory (C:\Program Files\Docker\) because of permissions issues or because the folder does not exist.
1. Download Docker Compose to a Different Directory:
Try saving it to your C:\Users\Administrator folder or any other directory where you have write permissions:
``` powershell
Invoke-WebRequest -Uri https://github.com/docker/compose/releases/download/v2.21.0/docker-compose-Windows-x86_64.exe -OutFile "C:\Users\Administrator\docker-compose.exe"
```
2. Move Docker Compose to the Docker Directory (Optional):
After downloading it, you can manually move it to the Docker directory (C:\Program Files\Docker) if needed. You can do this via File Explorer or using the following command:
``` powershell
Move-Item "C:\Users\Administrator\docker-compose.exe" -Destination "C:\Program Files\Docker\docker-compose.exe"
```
If C:\Program Files\Docker doesn't exist, you can create it first:
``` powhershell
```
3. Add Docker Compose to the System PATH (if necessary):
After moving the file to the correct directory, make sure the directory is in your system's PATH. You can add it via the following command:
``` powershell
$env:Path += ";C:\Program Files\Docker"
```
