# NET Core

### Introduction
.NET Core is an open-source, general-purpose development platform maintained by Microsoft and the .NET community on GitHub. It's cross-platform (supporting Windows, macOS, and Linux) and can be used to build device, cloud, and IoT applications. It was designed so everything could be done from the command line, without the need for an IDE.

### Installing .NET Core On Windows
Download and install the official MSI installer for x64 or x86

> After installation In PowerShell type `dotnet --info`
> The version installed at this time is: **1.0.0-preview2-003133**


| Command | Description |
| ------------- |:-------------:|
| new     | Initialize a basic .NET project |
| restore | Restore dependencies |
| build   | Build a NET project |
| publish | Publish a NET project for deployment |
| run     | Compile and immediately execute |
| test    | Run unit test using the test runner specified in the project |
| pack    | Create a Nuget package |

### Creating a project with the NET Core CLI
- Access to the path where you start the project using Power Shell
	- Or access to the folder in Windows Explorer and Click on file menu and select **Open Power Shell**
- Type the command **dotnet new**, some files are created in the directory.
	- **Project.json**: serves in every project as a configuration file for both Nuget and Visual Studio
	- **Program.cs**: a basic c# file

### Compile and run a project 
- In order to compile and then run our basic console application, we must first restore the dependencies
- Type the command **dotnet restore**, 
	- the file **project.lock.json** is now created.
- Type the command **dotnet build**
- If we want to run the app, type **dotnet run**


### Porting to .NET Core
- *ASP.NET Core app and service* The primary reason to migrate your existing ASP.NET app is to run cross-platform. The best foundation for ASP.NET Core is a website using MVC, WebAPI, or both, while the not-so-fitting candidates are all those web apps built using WebForms.
- *Console Applications* One of the biggest reasons you should look into using .NET Core for console applications is because it allows targeting multiple operating systems (Windows, OS X, and Linux). Another strong reason is that .NET Native for console apps will eventually support producing self-contained, single-file executables with minimal dependencies

### Installing .NET Core On Linux Ubuntu on Azure
- Create a Virtual Machine on Windows Azure with OS Ubuntu 14.04 (this is the last stable version)
- You need to have installed gitbash for windows
- Login to the remote VM from gitbash `ssh roberto@40.71.100.249`
- Installing the .NET CLI 
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    sudo apt-get update    
    sudo apt-get install dotnet-dev-1.0.0-preview2-003118

- Check if the NET Core was installed with `dotnet --info`

#### Remote Desktop access to remote VM
- Login to the remote VM from gitbash `ssh roberto@40.71.100.249`
- Install ubuntu desktop and remote desktop protocol
    sudo apt-get install ubuntu-desktop 
    sudo apt-get install xrdp

#### Configurtation in Azure portal
- Entrar a overview de la maquina virtual creada
- Hacer click en Resource Group
- Dentro del XXX-nsg crear un inbound rule
	- Se debe llamar RDP y debe ser de tipo RDP con el puerto TCP 3389
- Esta configuración permitira el acceso a traves de escritorio remoto