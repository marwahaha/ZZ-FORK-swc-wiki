Despite our best efforts to ensure that bootcamp attendees configure their computers ahead of time,
installation and configuration issues pop up frequently during bootcamps.
This page collates common configuration problems encountered by instructors and presents their solutions.

Please feel free to edit this page to add new problems and solutions to the appropriate sections as you encounter them.
The only requirement is that you should have only add a problem
once you have identified and confirmed a solution
that at least worked for the cases that you encountered personally.
If you encounter a problem that you don't know how to solve,
please raise it in an issue in the [repo](http://github.com/swcarpentry/bc)
so that the community can find a resolution.

---

#### Bash

**Windows (Git Bash)**

*   None of the bash commands seem to work, although the student opened "Git Bash" and the command window is open
    *   Ensure that the student has run the program `mysys` and not `git-cmd` - the latter will not open the bash emulator environment.

*   User has created/saved files and doesn't know where they are
    *   Have users run `cd` after opening Git Bash and before doing anything else -
        this will place users in a home directory with Desktop as a subdirectory.
        `cd` then `cd Desktop` will place users on their Desktop.

*   Copy/paste doesn't work in the command window
    *   To copy,
        right click the title bar and go to Edit->Mark.
        Then you can use the mouse to select text and anything you select is copied.
        You can use Edit->Paste to paste,
        or the Insert key is a shortcut.

---

#### Python

**All**

*   Command `ipython notebook` fails with an error like `ipython not found`.
    *   It's very likely that Canopy or Anaconda did not properly append the path to its Python installation to the system path.
        (On Canopy,
        this easily occurs if students do not open GUI after installation
        or if they accidentally select "No" when asked if they want to make Canopy the default Python environment.)
        The fix is to add the right directory to the path.
        On a Mac using Canopy,
        for example,
        the command to do this is:

        ~~~
        export PATH=/Users/<username>/Library/Enthought/Canopy_64bit/User/bin:$PATH
        ~~~

*   Imports of scientific Python packages fails from a command line interpreter or script
    (i.e., `python myfile.py`)
    even though user installed Canopy or Anaconda.
    *   Use `which python` to make sure the Python being run is actually Anaconda or Canopy's;
        if not,
        ensure that path to Canopy or Anaconda's Python
        is before the system's Python in the path.
    * Ask if the user wants their path restored after the workshop and, if so, ensure this is done.
    
*   The IPython notebook appears to be running but no output is shown after cells are run.
    *   Ensure that ad blocker extensions are not active in the browser.
    *   Turn off Windows Sophos
        (see [https://github.com/ipython/ipython/wiki/Dev:-Windows-Sophos-issues](https://github.com/ipython/ipython/wiki/Dev:-Windows-Sophos-issues)).
    *   Try `ipython notebook --ip=localhost`
    *   Whitelist `127.0.0.1:8888` in the firewall settings
    *   If all else fails and the user has Canopy,
        notebooks can be opened and run directly from the Canopy GUI.

**Windows (Anaconda)**

*   New users can't easily open an IPython notebook in an arbitrary directory.
    *   The start menu shortcut created by Anaconda starts a notebook server in `%USERPROFILE%\Documents\IPython Notebooks` by default.
    *   New users, who may be unfamiliar with the command prompt, might have trouble opening a notebook in any other directory.
    *   A simple batch file with the single-line command `ipython notebook` will start a notebook server in any directory from which it is run.
    *   To make a batch file script that opens an IPython notebook in whatever directory it's run:
        1.   Create a simple text file on the desktop using Right Click->New->Text Document.
        2.   Re-name it "Start IPython Notebook Here.bat". (Don't forget to change the extension!)
        3.   Open it with Notepad using Right Click->Edit.
        4.   Add the text "ipython notebook" to the file: it should look just like you would type it in a command prompt.
        5.   Save and close.
    *   Now when a user double clicks on the `.bat` file, a notebook server will spawn in that directory. 
    *   The user can move or copy the `.bat` file wherever they want their IPython notebook's working directory to be. 
    *   Instructors can distribute a copy of the file in the same folder as any example notebooks,
        so the users can just double-click-and-go in the correct directory
        without ever seeing a command-line prompt.

* matplotlib import error when non-ASCII characters are present in the user's current working directory or user name
    * See matplotlib issue [3516](https://github.com/matplotlib/matplotlib/issues/3516/)
    * If attendee has permission on their computer, then re-install Anaconda in another location without non-ASCII characters in the directory path e.g. C:\Local\

**Mac(Anaconda)**

*  If a ValueError about locale and UTF-8 occurs when launching IPython notebook, this document
   discusses the solution: <http://conda.pydata.org/docs/troubleshooting.html#issue-valueerror-unknown-locale-utf-8-on-mac-os-x>

---

#### Git

**All**

*   When pushing to GitHub, the username or password is rejected although student knows that they are correct.
    *   Some students have needed to use their full email address, rather than their username, in the username field.
    *   (Although this makes little sense)
        Try hitting backspace many times in the password field to "clear out old password info".
        This might be a Git Bash issue, but is unconfirmed.

**Windows (Git Bash)**

*   The user is prompted to enter email address on first commit.
    *   Appears to be a bug in Git 1.8.4,
        as the user is prompted even if the email address is set in her global configuration.
        This should only occur on first commit.

*   `git push` to a GitHub repo fails with the error `could not read Username for https:...: No such device or address`.
    *   This is a bug in Git Bash 1.8.5 - have users install 1.8.4.
    *   If Git Bash 1.8.5 has been installed,
        using SSH instead of HTTPS for the push is a workaround,
        but takes several minutes to set up.
        First [generate and set up the SSH key](https://help.github.com/articles/generating-ssh-keys#platform-windows).
        After generating the SSH key and setting up GitHub to use it,
        change the origin from the Git repository directory using commands like these
        (with the appropriate user name and repository name):
        *   `git remote rm origin`
        *   `git remote add origin git@github.com:username/repo.git`

*   Configuring the git editor to a Windows program like Notepad++.exe is not intuitive.
    If the path to the program is not in the PATH,
    changing the system environment variable may require a restart.
    Additionally,
    the editor program may open with the previous session and tabs,
    causing the commit to abort.
    The following setup works for Notepad++
    (adjust for the correct install path for 32-bit / 64-bit executable):

    ~~~
    git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
    ~~~

**Windows (Rstudio)**: 

*   The Git tab does not appear in Rstudio.
    *   You need to tell Rstudio where to find your `git` executable.
        Assuming you are using Git Bash:
        *   Go to `Tools->Global options`.
        *   Select `Git/SVN`.
        *   Under `Git executable` select `Browse`.
        *   Navigate to and select following file: `C:/Progarm Files (x64)/git/bin/git`.
    *   Restart Rstudio.

**Mac**

*   Running the `git` command fails with an error like `Illegal operation`.
    *   It appears that git 1.9.x is incompatible with Mac OS X 10.6 and 10.7.  [Version 1.8.4.2](https://code.google.com/p/git-osx-installer/downloads/detail?name=git-1.8.4.2-intel-universal-snow-leopard.dmg) works for Mac OS X 10.6.8 (newest release of Snow Leopard).  Other older installers are available from [https://code.google.com/p/git-osx-installer/downloads/list](https://code.google.com/p/git-osx-installer/downloads/list). 

*   Running `git` on Mac OS X 10.8.5 produced an error message reporting "lazy symbol binding failed".
    *   Most online advice involved installing XCode with the command line tools,
        which is a 1.6 GByte download...

*   Trying to clone a repository from github fails with an error like `error: SSL certificate problem, verify that the CA cert is OK`
    *   A temporary fix that should allow the learner to progress with the lesson is to run

        ~~~
        export GIT_SSL_NO_VERIFY=true`
        ~~~

    *   This setting change will not persist after rebooting the machine. The error is probably caused by expired SSL certificates and should be fixed by updating the OS.
    *   If an OS update is not possible, you can also try installing the tigerbrew port of git, like so:

        ~~~
        ruby -e "$(curl -fsSkL raw.github.com/mistydemeo/tigerbrew/go/install)"
        export PATH=/usr/local/sbin:/usr/local/bin:$PATH
        brew install git
        hash -r
        git clone ...
        ~~~

**Linux**

*   Permission denied (public key) on pushing to Github.
    *   This has several causes and thus several solutions: [GitHub has a list](https://help.github.com/articles/error-permission-denied-publickey).
        In a nutshell:
        *   Don't use sudo (and check whether the permissions for SSH keys are correct).
        *   Connection problems or weird routing problems: `ssh -vT git@github.com` should print `github.com` and port 22.
        *   Connect only as user `git` .
        *   Check whether `ssh-add -l` prints a key, if not, create one.
        *   Check whether `ssh-keygen -lf ~/.ssh/id_rsa.pub` prints several keys, and check which one GitHub uses in the account settings.
        *   You can always troubleshoot more using `ssh -vT git@github.com`.

*   Error 403 returned on a `git push` to GitHub.
    *   This is usually caused by an older version of Git.
        GitHub now [requires git 1.7.10 or later](https://help.github.com/articles/https-cloning-errors).

*   A `git push` results in a dialog requiring a password to unlock the private key:
    "Enter password to unlock the private key. An application wants access to the private key xxxxx".
    *   This can be caused by an SSH agent,
        such as Gnome Keyring,
        managing SSH keys when these are already managed with OpenSSH.
        [Disable SSH keyring support in Gnome](https://wiki.gnome.org/Projects/GnomeKeyring/Ssh). 