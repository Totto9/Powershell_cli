# Powershell_cli
## The mission

You already had the opportunity to learn and use the Bash shell, but there are many other shells one could use. They all have their specificities, strengths and weaknesses; although they all work in similar manner they all answer to specific needs. One could says that knowing a new shell is like having a new tool in your toolset, this challenge will have you add [Powershell](https://en.wikipedia.org/wiki/PowerShell) to it.

Powershell is one of the **most common shells** in modern **Windows systems**, it has a different approach to Bash in that it manipulates [data types](https://docs.microsoft.com/en-us/powershell/scripting/lang-spec/chapter-04?view=powershell-7.1) instead of exclusively text I/O (_input/output_). Furthermore, since it's developped by Microsoft it's also deeply integrated into Windows enebling extensive Windows system configuration.

> **NOTE**: Powershell is actually not a Windows exclusive shell, it can easily be used in Linux as well. However some of its functionalities are Windows specific and some of the exercises below will only be doable on a Windows system.

1. RTFM
2. Navigation
3. File operations
4. Permissions
5. Package Management
6. Environment Variables
7. Trial of Might
8. Bonus

To validate this challenge you will need to make a pull request on your training's repository with a file (`<your_name>.txt`, _snake cased_) containing a list of the passwords required to advance in the "Trial of Might" exercice. One password per line (_if possible_).

Powershell Answer and exercice.

# RTFM

Everybody needs a little help at one time or another. In this exercise we will check how you can get help on powershell and how you can find interesting commands following your needs.

- Open a PowerShell session in your terminal
- type `Get-Help`
- Find out what a command such as `Get-Process` does without looking on Google
  `Get-Process -?` => The "Get-Process" cmdlet gets the processes on a local or remote computer. 
- Now try with the `-online` parameter
  `get-help Ger-Process -online` will open the browser to display the command's online library.
- What does `Get-command` do?
  `Get-Command -?` => The Get-Command displays all the cmdlets, functions, and aliases installed on the computer.
  
Answer : The Get-Command displays all the cmdlets, functions, and aliases installed on the computer.

# Navigation

Now we will learn how to move around in the filesystem with `Set-Location`, `Get-Location`, `Get-ChildItem` ...

- Print your current location on the screen
Get-Location
- Print the content of your current directory
Get-ChildItem
- Print the content of your root (`C:` _if you're running windows_, `/` _for linux_)
 Answer .  dir C:\
- Go into your home folder (_C:\Users\Username or /home/Username_)
  Set-Location C:\Users\Username
- Print the content of your home
Get-ChildItem

- Those commands are pretty long to type, do you know any shorter way to do it?
For Get-Location we can use gl
For Get-ChildItem we can use dir

# File Operations

Now that we know how to move in the file system, let's check how to manipulate files and folders. In this challenge, you will discover and use the commands `Get-Content`, `echo`, `New-Item`, `Move-Item`, `Copy-Item`, and `Remove-Item`.

- Create a file named story1.txt

`New-Item story1.txt`
- Type `echo "Hello World" > story1.txt`
- Print the content of the file

`Get-Content story1?.txt` or `Get-Content ./story1.txt`
- Create a folder named `story`

You need to specify the type. `New-Item story -Type Directory`
- Move `story1.txt` inside `story`

`Move-Item story1.txt story`
- Copy `story1.txt` as `story2.txt`

`Copy-Item .\story\story1.txt .\story\story2.txt`
- Print both files

`Get-Content story1.txt,story2.txt`
- Rename `story2.txt` as `me.txt`

`Rename-Item story2.txt me.txt`
- Append `me.txt` and add "I am a junior at Becode"

`echo "I am a junior at BeCode" >> me.txt`
- Remove the folder story with its content

`Remove-Item story` And answer "Y" or "A".
Or `Remove-Item –path c:\users\username\story –recurse `

# Powershell Permissions

Now that we can navigate and create files, we should be able to change permissions on these. We will use the commands `Get-Acl`, `Set-Acl`, `RunAs`

- Create a file

`Net-Item Alice.txt`
- Check the owner and the groups

`Get-Acl Alice.txt`
- Change the file owner to the built-in administrator (administrator account is disabled by default, check how to enable it. Don't forget to set a strong password!)

The usual way:
`$acl = Get-Acl Alice.txt
$acl.setOwner([System.Security.Principal.NTAccount]::new("Administrator"))
Set-Acl Alice.txt $acl
Get-Acl Alice.txt`
- Check the file's permission

![image](https://github.com/Totto9/Powershell_cli/blob/main/Screenshot%20from%202023-06-26%2010-42-42.png) 
- Try to print the content of the file as your normal user

`Nothing happens.`
- Print the content of the file using the "administrator" account

To run as the Administrator user: `runas /user:Administrator" "powershell"`

This will prompt the user to type in the Administrator's password. Once typed in, a new PowerShell window will open and you're logged in as Administrator. 

`Set-Location c:\Users\alice`

`Get-ChildItem`

`Get-Acl Alice.txt`

# Powershell Package Management

It's time to start installing software and keep them updated. We will see how to use Chocolatey and how to use Windows Updates.

* Instructions

- Get Windows updates
    - Install the `PSWindowsUpdate` module

`Install-Module -Name PSWindowsUpdate`
=> In order to run the command Get-WindowsUpdate (below), we need to enable PowerShell scripts to run. For that, we need to run the command `Set-ExecutionPolicy RemoteSigned` and Type "A" to accept all. 

Type `Get-WindowsUpdate` to check for updates
Type `Install-WindowsUpdate` to install updates

- Manage Packages

Chocolatey or Choco as it is sometimes referred to, is a free, open-source package manager for Windows that is very similar to Apt or DNF in the Linux realm. In other words, this is a program used for installing software via the Windows command line. It downloads a program, installs it, then will check for updates, and installs those updates automatically if needed. Those who use Linux are quite familiar with the package management systems like this.

    - Install `Chocolatey`

From https://chocolatey.org/install, you need to run the following script: `Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))`

![image](https://github.com/Totto9/Powershell_cli/blob/main/Screenshot%20from%202023-06-26%2011-56-56.png)

    - Install `VLC` from `Chocolatey`
`choco install vlc`

![image](https://github.com/Totto9/Powershell_cli/blob/main/Screenshot%20from%202023-06-26%2012-18-09.png)

    - Upgrade `VLC` to the latest version (it should already be but it's your first use)

`choco upgrade vlc`

![image](https://github.com/Totto9/Powershell_cli/blob/main/Screenshot%20from%202023-06-26%2012-25-28.png)

    - Remove the `VLC` package using `Chocolatey`

`choco uninstall vlc`

![image](https://github.com/Totto9/Powershell_cli/blob/main/Screenshot%20from%202023-06-26%2012-20-46.png)

    - Could you use `Chocolatey` on already installed software? How?

Chocolatey is a tool that simplifies software installation on Windows by automating the process. It can take over existing installations and handle uninstallation in most cases, depending on the packaging and underlying software installer used. However, since software installers on Windows vary greatly in their approach, each package needs to account for these differences. If the registry snapshot is the same or there is no appropriate uninstall script, Chocolatey may not be able to automatically uninstall the software. Nevertheless, Chocolatey continues to improve and may include future checks to handle such scenarios. Overall, using Chocolatey can provide benefits such as streamlined upgrades, increased consistency, and a unified interface for managing Windows installations.

Ref.: https://docs.chocolatey.org/en-us/why

- Manage Windows Features
    - Get installed Windows features with the command `Get-WindowsFeature`

This command is not available on Win10 Home. Only on Server versions.

    - Install a new feature such as hyper-v with `Install-WindowsFeature`

Use this command: `Get-WindowsOptionalFeature -Online`

- Install a new feature such as hyper-v with Install-WindowsFeature

 It actually needs to be enabled.
`Enable-WindowsOptionalFeature -FeatureName Microsoft-Hyper-V-All -Online`

![image](https://github.com/Totto9/Powershell_cli/blob/main/Screenshot%20from%202023-06-26%2014-07-45.png)

After Enabling this, we need to restart the computer.
       
**WARNING**: This exercise **will only work on Windows** since it's specific to the way Windows manages packages.

# Powershell Environment Variables

In any programming language, you can store data within variables. However they only exist as long as the program is executed, but what if you wanted to share information between two applications not running at the same time?

Then you could use environment variables, which are variables stored at the OS level. You can access them from anywhere.

- Open a Powershell Terminal
- Type `echo $env:computername`. It should show your computer's name
- Create a variable by typing `$env:test = "hello powershell"`
- Check the variable you just created the same way you did with the computer name
- Now we will add something to this variable by typing `$env:test += " Becode"`
- Check the variable
- There is one really important environment variable: `$env:path`. This variable stores the location where Windows looks for files that are not in your current folder. For example, if you call VSCode from the PowerShell terminal, it opens it even if the executable isn't present in the current folder. That's because the path to the vscode's executable is stored in `$env:path`. Now download an executable software ([rufus](https://github.com/pbatard/rufus/releases/download/v3.13/rufus-3.13p.exe) for example) and copy it on your desktop. If you try from a command line to launch it, it will fail with a command not found (if you are not in the same folder).
- Try to append the $env:path by adding the path to rufus' executable (it should be something like this if you copied it on your desktop: `C:\Users\Username\Desktop`)
