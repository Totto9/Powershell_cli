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

![image]()

    - Upgrade `VLC` to the latest version (it should already be but it's your first use)

`choco upgrade vlc`

![image]()

    - Remove the `VLC` package using `Chocolatey`

`choco uninstall vlc`

![image]()

    - Could you use `Chocolatey` on already installed software? How?
Chocolatey works in a similar manner to how you would do things if you downloaded and installed things yourself. Its design and infrastructure are built that way on purpose. It takes the pain of manually doing it yourself away.
Now, Chocolatey can take over existing installs and be able to handle uninstalls in most cases. Can is very dependent on the packaging and the underlying software installer that is used for the installation (installer packages are the context here).
So when a package takes over the existing install, if the registry snapshot doesn't differ, it won't be able to automatically uninstall it (if you have autoUninstaller turned on). If there is no chocolateyUninstall.ps1 that would uninstall the software, choco won't be able to uninstall it.

Ref.: https://docs.chocolatey.org/en-us/why#can-i-use-chocolatey-with-existing-software

- Manage Windows Features
    - Get installed Windows features with the command `Get-WindowsFeature`

This command is not available on Win10 Enterprise. Only on Server versions.

    - Install a new feature such as hyper-v with `Install-WindowsFeature`

Use this command: `Get-WindowsOptionalFeature -Online`

![image]()

  - Install a new feature such as hyper-v with Install-WindowsFeature

 It actually needs to be enabled, as you can see in the screenshot above.
`Enable-WindowsOptionalFeature -FeatureName Microsoft-Hyper-V-All -Online`

After Enabling this, the computer will need a restart.
       
**WARNING**: This exercise **will only work on Windows** since it's specific to the way Windows manages packages.
