This page is still under construction and may be incomplete. Caveat lector.  

**Note** - see [Windows: Step-by-Step Guide](https://www.wireshark.org/docs/wsdg_html_chunked/ChSetupWindows.html) in the [Wireshark Developer’s Guide](https://www.wireshark.org/docs/wsdg_html_chunked/index.html) for full setup of Windows dev environment.  
Pack a lunch - takes a while.

# Introduction

Git for Windows is like a fish out of water, it's not the really the place for it, but it has become a bit of an amphibian. Anyway, git is now required for Wireshark development so this page attempts to fill in some of the blanks for git newbies.

# git clients

There are quite a few [git clients](https://git.wiki.kernel.org/index.php/InterfacesFrontendsAndTools), including the [canonical version](http://git-scm.com/download/win), [GitHub for Windows](http://windows.github.com/), [TortoiseGit](http://code.google.com/p/tortoisegit/), [git extensions](http://sourceforge.net/projects/gitextensions/) and probably many others. However, as most use on other platforms tends to be command-line git, most books and guides tend to concentrate on that so windows users may actually find it easier to start with the command line and then move to graphical clients when they are comfortable with the basics. Unless stated otherwise, all the instructions listed here are intended to be run from a [PowerShell](https://docs.microsoft.com/en-us/powershell/) prompt.

As there are a few moving parts required to make life comfortable, this guide uses the [Chocolatey](http://chocolatey.org/) package manager to acquire and install the items required.

## Installing Chocolatey

*Chocolatey NuGet is a Machine Package Manager, somewhat like apt-get, but built with Windows in mind.*

Chocolatey uses PowerShell to act as a package manager.

Open a **<span class="u">cmd</span>** prompt and paste the following text in and run it (note this should be all on one line):

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%systemdrive%\chocolatey\bin

## Installing git and helpers

Now open a PowerShell prompt, and paste the following line in and run it:

    choco install git

Your PowerShell prompt should now be git enabled, there are some tools however, that make life a little easier.

[posh-git](https://github.com/dahlbyk/posh-git) is *A set of PowerShell scripts which provide Git/PowerShell integration*. ([Video](https://www.youtube.com/watch?v=WBg9mlpzEYU) showing enhanced prompt in action)  
To install, at a PowerShell prompt, paste the following text in and run it.

    choco install poshgit

posh-git should have added itself to your PowerShell profile, check by examining your profile:

    cat $PROFILE

## Configuring git and the helpers

Now all the parts are installed, they need to be connected up.

git requires some initial configuration, adjust as appropriate:

    git config --global user.name "John Doe"
    git config --global user.email johndoe@example.com
    git config --global core.editor Path/with/forward/slashes/to/your/preferred/editor

using ssh with git requires some further configuration. Switch to your "home" directory and create a .ssh subdirectory, copy your ssh private key there (e.g. id\_rsa):

    cd ~
    mkdir .ssh
    cp path\to\key .ssh\

If you have been using PuTTY with svn, you'll have to convert your PuTTY .ppk key file to a standard ssh version by using PuTTYGen to load the .ppk file and then use the Conversions | Export OpenSSH menu item to export the key in the required format as `C:\Users\youraccountname\.ssh\id_rsa`.

Note that if the private key passphrase contains space characters, you'll need to use PuTTYGen to modify the passphrase to one without spaces and export it, and then in Git Bash use `ssh-keygen -p -f path\to\id_rsa` to change the passphrase back to the original one with ssh approved spaces (if you want to keep the spaces).

Exit your PowerShell prompt and start another. This allows posh-git to find the new key and it will fire up ssh-agent and ask you for the key pass-phrase to add the key to the agent.

Assuming you have previously set up your [GitLab](https://gitlab.com/wireshark/wireshark) account with your ssh public key, you can now check your ssh connection to GitLab. Note that you'll need to find the path to the Git version of ssh, usually `C:\Program Files\Git\usr\bin\ssh`:

    path\to\git\ssh git@gitlab.com

This should bring up a quick "Welcome to GitLab" response from the gitlab.com server with your GitLab account name before the connection is closed.

---

Imported from https://wiki.wireshark.org/Development/SubmittingPatches/GitForWindows on 2020-08-11 23:13:10 UTC
