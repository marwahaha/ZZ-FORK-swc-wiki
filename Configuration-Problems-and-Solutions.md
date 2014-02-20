Despite our best efforts to ensure that bootcamp attendees configure their computers ahead of time, installation and configuration issues pop up frequently during bootcamps. This page collates common configuration problems encountered by instructors and presents their solutions.

**To instructors**: Please feel free to edit this page to add new problems and solutions to the appropriate sections as you encounter them. The only requirement is that you should have only add a problem once you have identified and confirmed a solution that at least worked for the cases that you encountered personally. If you encounter a problem that you don't know how to solve, please raise it in an issue in the `bc` repo so that the community can find a resolution.

### Bash

**Windows (Git Bash)**

* None of the bash commands seem to work, although the student opened "Git Bash" and the command window is open
    * Ensure that the student has run the program `mysys` and not `git-cmd` - the latter will not open the bash emulator environment.

* User has created/saved files and doesn't know where they are
    * Have users run `cd` after opening Git Bash and before doing anything else - this will place users in a home directory with Desktop as a subdirectory. `cd` then `cd Desktop` will place users on their Desktop. ([#56](../issues/56))

* Copy/paste doesn't work in the command window
    * To copy, right click the title bar and go to Edit -> Mark. Then you can use the mouse to select text and anything you select is copied. You can use Edit -> Paste to paste, or the Insert key is a shortcut. ([#140](../issues/140))

### Python

**All**

* Command `ipython notebook` fails with `ipython not found`-type error
    * Very likely that Canopy or Anaconda did not properly append path to Python installation on system path  (on Canopy, this easily occurs if students do not open GUI after installation or if they accidentally select "No" when asked if they want to make Canopy the default Python environment). To fix, export path in .bash_profile. On a Mac using Canopy, command is `PATH=/Users/<username>/Library/Enthought/Canopy_64bit/User/bin:$PATH; export PATH`

* Imports of scientific Python packages fails from a command line interpreter or script (i.e., `python myfile.py`) even though user installed Canopy/Anaconda
    * Ensure that path to Canopy/Anaconda python comes before path to system Python (see above). Check with `which python` and make sure it is Canopy/Anaconda version.

* IPython notebook appears to be running but no output is shown after cells are run
    * Ensure that Ad blocker extensions are not active in browser
    * Turn off Windows Sophos (see https://github.com/ipython/ipython/wiki/Dev:-Windows-Sophos-issues)
    * Try `ipython notebook --ip=localhost`
    * If all else fails and user has Canopy, notebooks can be opened and run directly from Canopy GUI

### git

**Windows (Git Bash)**

* User is prompted to enter email address on first commit
    * Appears to be a bug in 1.8.4, as user is prompted even if global config is set with email address. This should only occur on first commit.

* `git push` to a Github repo fails with error `could not read Username for https:...: No such device or address`
    * Bug in Git Bash 1.8.5 - have users install 1.8.4. ([#234](../issues/234))
    * This appears to be an issue with the default credential store. Changing the credential store will solve the problem, but I don't see the right command in a quick Google search. I'll look again later...
    * If Git Bash 1.8.5 has been installed, using ssh instead of https for the push is a workaround, but takes several minutes to set up. First [generate and set up the SSH key](https://help.github.com/articles/generating-ssh-keys#platform-windows). After generating the ssh key and setting up github to use it, change the origin from the git repository directory (replace username and repo name below):
      * `git remote rm origin`
      * `git remote add origin 'git@github.com:username/repo.git'`

* Configuring the git editor to a Windows program like Notepad++.exe is not intuitive. If the path to the program is not in the PATH, changing the system environment variable may require a restart. Additionally, the editor program may open with the previous session and tabs, causing the commit to abort. The following setup works for Notepad++ (adjust for the correct install path for 32-bit / 64-bit executable):
`git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"`

**Windows (Rstduio)**: 

* git tab does not appear in Rstudio
    * You need to tell Rstudio where to find your git executable. Assuming you are using gitbash,
         * Go to `Tools -> Global options` 
         * Select `Git/SVN`
         * Under `Git executable` select `Browse`
         * Navigate to and select following file: `C:/Progarm Files (x64)/git/bin/git`
   * Restart Rstudio



**Linux**

* Permission denied (public key) on pushing to Github

    * Has several causes and thus several solutions. [Github has a list](https://help.github.com/articles/error-permission-denied-publickey), in a nutshell:
        * Don't use sudo (and check whether the permissions for SSH keys are correct)
        * Connection problems or weird routing problems: ```ssh -vT git@github.com``` should print ```github.com``` and port 22
        * Connect only as user ```git``` 
        * Check whether ```ssh-add -l``` prints a key, if not, create one
        * Check whether ```ssh-keygen -lf ~/.ssh/id_rsa.pub``` prints several keys, and check which one GitHub uses in the account settings
        * You can always troubleshoot more using ```ssh -vT git@github.com```

* error 403 returned on a git push to github

  * This is usually caused by an older version of git. GitHub now [requires git 1.7.10 or later](https://help.github.com/articles/https-cloning-errors).

* a git push results in a dialog requiring a password to unlock the private key: "Enter password to unlock the private key. An application wants access to the private key xxxxx".

  * This can be caused by an ssh agent, such as Gnome Keyring, managing ssh keys when these are already managed with OpenSSH. [Disable SSH keyring support in Gnome](https://wiki.gnome.org/Projects/GnomeKeyring/Ssh). 

