# The Git(Hub) Workflow

In this guide I'll explain how to use `git workflow`, applied to our project using the command line on a Debian/Linux system. I know you're using Mac, but at least all the git command should be the same. For the others I assume you know how to adopt them to the Mac equivalents if needed.  
There's a good chance that several of the things which I'll describe in this document make you go *duhh*, but it's not meant to be pedantic but I don't know your proficiency with the command line commands and also to be complete.

The main example I'll use in this guide is about the process of adding donation links to our project.

In this guide I'll show lots of (variations of) commands and I **highly** recommend that not only you actually try/do them yourself, but also experiment with them.

## Fork the project
The first thing you should do when you want to contribute to any project on GitHub, is `fork` that project so it becomes available in your personal repositories list, under https://github.com/Mausy5043?tab=repositories and in our case that would become https://github.com/Mausy5043/raspbian-ua-netinst. Once it's there, you want to get it locally so you can work on it.  

## Clone the project locally
So on your computer, go to the directory where you want to store your project under and for this tutorial I'll use `~/dev/`, which is the `dev` directory in your `home` directory.
Then to get the project locally, do:
```sh
diederik@bagend:~$ cd ~/dev/
diederik@bagend:~/dev$ git clone https://github.com/Mausy5043/raspbian-ua-netinst
```
This will create a `~/dev/raspbian-ua-netinst` directory and downloads the whole repository. To start working on it, `cd` into that directory.
Then I suggest you first start with a command which you will/should use quite often, namely `git status` and it should output something like this:
```sh
diederik@bagend:~/dev$ cd raspbian-ua-netinst
diederik@bagend:~/dev/raspbian-ua-netinst$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
```
This tells you a couple of things:

1. This directory is under control of `git`, otherwise you'd see an error message;
2. You are currently on the `master` branch, which is in most git projects the default;
3. Your local master branch is in sync with the master branch of the `origin` remote.

When you `clone` a git repo, the default name of the *remote* repository is `origin`. More on *remotes* later on.

## Using (feature) branches
You should do **every** new feature, bug fix or any other change in its own branch.  
Branches in git are cheap and you should take advantage of that. In a branch you can freely experiment all you want and if it doesn't work out, you just discard your branch and start over with a new approach. And when it does work out, you push your changes to your **own** fork, with the same branch name, and then you can offer your change to be merged into the main project through a *Pull Request*.

You branch your own branch off of the branch to which you want to apply the change and you do that by first checking out that branch. In our case that should normally be the `v1.1.x` branch, so first checkout that branch:
```sh
diederik@bagend:~/dev/raspbian-ua-netinst$ git checkout v1.1.x
Branch v1.1.x set up to track remote branch v1.1.x from origin.
Switched to a new branch 'v1.1.x'
```
Git recognized that there was already a `v1.1.x` branch in your `origin` remote and checked out that branch and automatically set your local `v1.1.x` branch up to track the remote branch. If you now do a `git status` you'll get a similar response as above with the master branch, but this time it'll say the same things about the `v1.1.x` branch.

Now it's time to create your own branch and check that out. You can do that by `git branch <branch-name>` and `git checkout <branch-name>`, but it's more commonly done through a shortcut which combines those two commands into one: `git checkout -b <branch-name>`:
```sh
diederik@bagend:~/dev/raspbian-ua-netinst$ git checkout -b adding-donation-links
Switched to a new branch 'adding-donation-links'
```
Now that we have our brand new branch, it's time to actually use it.

## Making changes and committing them
The basic procedure of making changes and committing them consist of the following steps:
```sh
vim <file-to-edit>
... make change to <file-to-edit> ...
:wq
git add <file-to-edit>
git commit -m "<commit-message>"
```
If you *really* must you can use another editor then `vim`, git doesn't care, but you'll lose your coolness among your fellow contributors.  
This will commit those changes to your local repo, in your current branch, which is *adding-donation-links*.   
This is pretty straight forward and likely doesn't contain anything you didn't know before.

But now lets make it a bit more interesting and more useful.

To do that I'll add some info about donations to the README.md document. Sounds rather simple right? It usually is, but in this case we'll encounter some issues.  
But we will fix them all and along the way you should pick up some useful git techniques.

Before you start, it's always good to know where you stand:
```sh
diederik@bagend:~/dev/raspbian-ua-netinst$ git status
On branch adding-donation-links
nothing to commit, working directory clean

diederik@bagend:~/dev/raspbian-ua-netinst$ git log -1
commit 2a22a2cf0732c6a7c49e4a161956d95daa5d4bd1
Author: Maurice (mausy5043) Hendrix <Mausy5043@users.noreply.github.com>
Date:   Sat Apr 16 13:15:42 2016 +0200

    wpa_supplicant corrections (#4)
    * Concentrate networking and relocate wpa_supplicant
    * use the wpa_supplicant.conf in `/etc/...`
    * Removed "weird alignment"
    * no quotes needed on empty line
    * move empty line to  before additional entries
    * removed empty comment line
    * moved the `echo` inside the `if..fi` block
    * Add "wpa-conf" entry if not already present
```
You have already seen this output of the `git status` command, with the only difference that it now tells you that you're on the *adding-donation-links* branch.  
The `git log` command is very versatile and the `-n` parameter, where `n` is a number, shows you the last `n` commits.  
You probably recognize commit [2a22a2c](https://github.com/Mausy5043/raspbian-ua-netinst/commit/2a22a2cf0732c6a7c49e4a161956d95daa5d4bd1) with title *wpa_supplicant corrections (#4)* as the last commit you did on your *v1.1.x* branch and that is correct as we branched off that branch and haven't made any changes yet.

There is an issue with your *v1.1.x* branch though and can be seen because of what https://github.com/Mausy5043/raspbian-ua-netinst/tree/v1.1.x says: *This branch is 1 commit ahead, 23 commits behind debian-pi:v1.1.x.*


And here are the changes I've made to *README.md*:

```diff
diederik@bagend:~/dev/raspbian-ua-netinst$ git diff
diff --git a/README.md b/README.md
index 4e646ac..12f4d3a 100644
--- a/README.md
+++ b/README.md
@@ -10,6 +10,7 @@
 - [First boot](#first-boot)
 - [Reinstalling or replacing an existing system](#reinstalling-or-replacing-an-existing-system)
 - [Reporting bugs and improving the installer](#reporting-bugs-and-improving-the-installer)
+- [Donations](#donations)
 - [Disclaimer](#disclaimer)
 
 ## Intro
@@ -202,15 +203,16 @@ The system is almost completely unconfigured on first boot. Here are some tasks
 The default **root** password is **raspbian**.
 
 > Set new root password: `passwd`  (can also be set during installation using **rootpw** in [installer-config.txt](#installer-customization))  
-> Configure your default locale: `dpkg-reconfigure locales`  
-> Configure your timezone: `dpkg-reconfigure tzdata`  
+> Configure your default locale: `dpkg-reconfigure locales`  (can also be set during installation using **rootpw** in [installer-config.txt](#installer-customization))  
+> Configure your timezone: `dpkg-reconfigure tzdata`  (can also be set during installation using **locales** and/or **system_default_locale** in [installer-config.txt](#installer-customization))  
+
 
 The latest kernel and firmware packages are now automatically installed during the unattended installation process.
 When you need a kernel module that isn't loaded by default, you will still have to configure that manually.
 
 > Optional: `apt-get install raspi-copies-and-fills` for improved memory management performance.  
 > Optional: Create a swap file with `dd if=/dev/zero of=/swap bs=1M count=512 && mkswap /swap && chmod 600 /swap` (example is 512MB) and enable it on boot by appending `/swap none swap sw 0 0` to `/etc/fstab`.  
-> Optional: `apt-get install rng-tools` and add `bcm2708-rng` to `/etc/modules` to auto-load and use the kernel module for the hardware random number generator. This improves the performance of various server applications needing random numbers significantly.
+> Optional: `apt-get install rng-tools` to use the Raspberry Pi's hardware random number generator. This improves the performance of various server applications needing random numbers significantly.
 
 ## Reinstalling or replacing an existing system
 If you want to reinstall with the same settings you did your first install you can just move the original _config.txt_ back and reboot. Depending on the hardware you want to reinstall on (Raspberry Pi **1** or **2**), make sure you still have _kernel_rpi1_install.img_ / _kernel_rpi2_install.img_ and _installer-rpi1.cpio.gz_ / _installer-rpi2.cpio.gz_ in your _/boot_ partition. If you are replacing your existing system which was not installed using this method, make sure you copy those files in and the installer _config.txt_ from the original image.
@@ -224,9 +226,15 @@ If you want to reinstall with the same settings you did your first install you c
 When you encounter issues, have wishes or have code or documentation improvements, we'd like to hear from you!
 We've actually written a document on how to best do this and you can find it [here](CONTRIBUTING.md).
 
+## Donations
+If you want to financially support (the members of) this project or just show your appreciation for their time and effort, you can do so in the following ways:
+
+For [Diederik de Haas](https://github.com/diederikdehaas):  
+Bitcoin:  [1GUL3WUWE98eDxmZqPzjsujMMspizzdhbJ](https://blockchain.info/address/1GUL3WUWE98eDxmZqPzjsujMMspizzdhbJ)
+
 ## Disclaimer
 We take no responsibility for ANY data loss. You will be reflashing your SD card anyway so it should be very clear to you what you are doing and will lose all your data on the card. Same goes for reinstallation.
 
-See LICENSE for license information.
+See [LICENSE](LICENSE) for license information.
 
   [1]: http://www.raspbian.org/ "Raspbian"
```
As you can see I did indeed add a section about donations, but I also made several changes which have nothing to do with that. When scrolling down after adding the TOC entry to the actual donation section, I noticed various things which were incorrect and have thus fixed them. But as specified in the [CONTRIBUTING.md document](https://github.com/debian-pi/raspbian-ua-netinst/blob/master/CONTRIBUTING.md) we should make *atomic* commits and not lump together unrelated changes.
For a good example of proper atomic commits, if I may say so myself, can be seen via the following command:
```sh
diederik@bagend:~/dev/raspbian-ua-netinst$ git log v1.1.x --since="2015-10-12" --until="2015-10-14" --author="Diederik de Haas" -3 --pretty=fuller
commit 625000d5290433e62ba408c8585c04ec8a89ec0e
Author:     Diederik de Haas <github@cknow.org>
AuthorDate: Thu Apr 30 06:37:00 2015 +0200
Commit:     Diederik de Haas <github@cknow.org>
CommitDate: Tue Oct 13 10:39:04 2015 +0200

    Added ifupdown components.

commit 452be53a9fa4c224be3ffff2db2c51eba9d5dce3
Author:     Diederik de Haas <github@cknow.org>
AuthorDate: Thu Apr 30 05:37:23 2015 +0200
Commit:     Diederik de Haas <github@cknow.org>
CommitDate: Tue Oct 13 10:39:04 2015 +0200

    Added libatm1 components.

commit df209e4b2a62688842290f6c00810e81295e49db
Author:     Diederik de Haas <github@cknow.org>
AuthorDate: Thu Apr 30 05:35:13 2015 +0200
Commit:     Diederik de Haas <github@cknow.org>
CommitDate: Tue Oct 13 10:39:04 2015 +0200

    Added iproute2 components.

```
Here you see a (far) more extensive example of the `git log` command with some pretty cool parameters.  
Let's break it down and explain them:

* `git log`: this is the git command
* `v1.1.x`: only show log entries from the *v1.1.x* branch even though I may not have checked out that branch (currently)
* `--since="2015-10-12"`: further filter down the list to return to only include commits since October 12, 2015
* `--until="2015-10-14"`: and don't show commits later then October 14, 2015
* `--author="Diederik de Haas"`: and only show commits where the author is *Diederik de Haas*
* `-3`: and only show the 3 most recent commits
* `--pretty=fuller` and show some extra fields which are normally not shown

Git's man(ual) pages are quite extensive and `man git-log` is no exception and will show you all the options for the `git log` command. I do warn you that they are also quite intimidating for non-experts, and that includes me. Yet, I still recommend that you look at it, as well as the other git man pages. Not only will it give you the possible parameters you can give to a (git) command, it pretty much always also includes an *Examples* section which are usually better to understand then the documentation itself and should give you an idea as to how to use it. One of the examples contains `--since="2 weeks ago"` which shows you another (cool) way to use the `--since` parameter.  
I normally don't use the `--pretty=fuller` parameter value, otoh the `oneline` is pretty cool, but otherwise you likely wouldn't have understood the results. 

The following example from my completely up-to-date working directory should help you understand the difference between *Author* and *Committer* a bit better:
```sh
diederik@bagend:~/dev/raspberry-pi/raspbian-ua-netinst$ git log v1.1.x -1 --pretty=fuller
commit 39d7115a7a88492bceabdb25a3a9b3f88d8b18f5
Author:     Diederik de Haas <github@cknow.org>
AuthorDate: Sat Jun 18 15:24:18 2016 +0200
Commit:     Maurice (mausy5043) Hendrix <Mausy5043@users.noreply.github.com>
CommitDate: Sat Jun 18 15:24:18 2016 +0200

    Kernel 4.4 update (#408)
    
    Bump required kernel to 4.4
```
Note that this is a **different** directory then the other examples!  
And now look at how [GitHub renders it](https://github.com/debian-pi/raspbian-ua-netinst/commit/39d7115a7a88492bceabdb25a3a9b3f88d8b18f5).

Back to the *atomic* commits.  
With the previous (extensive) `git log` command we found 3 atomic commits, but to see what happened in a specific commit, you can use the `git show` command:
```sh
diederik@bagend:~/dev/raspbian-ua-netinst$ git show 625000d5
commit 625000d5290433e62ba408c8585c04ec8a89ec0e
Author: Diederik de Haas <github@cknow.org>
Date:   Thu Apr 30 06:37:00 2015 +0200

    Added ifupdown components.

diff --git a/build.sh b/build.sh
index a6e0d7e..da3ad95 100755
--- a/build.sh
+++ b/build.sh
@@ -96,8 +96,10 @@ function create_cpio {
     # create all the directories needed to copy the various components into place
     mkdir -p rootfs/bin/
     mkdir -p rootfs/lib/lsb/init-functions.d/
-    mkdir -p rootfs/etc/{alternatives,cron.daily,default,iproute2,ld.so.conf.d,logrotate.d,network/if-up.d/}
+    mkdir -p rootfs/etc/{alternatives,cron.daily,default,init,init.d,iproute2,ld.so.conf.d,logrotate.d,network/if-up.d/}
     mkdir -p rootfs/etc/dpkg/dpkg.cfg.d/
+    mkdir -p rootfs/etc/network/{if-down.d,if-post-down.d,if-pre-up.d,if-up.d,interfaces.d}
+    mkdir -p rootfs/lib/ifupdown/
     mkdir -p rootfs/lib/lsb/init-functions.d/
     mkdir -p rootfs/lib/modules/${KERNEL_VERSION}
     mkdir -p rootfs/sbin/
@@ -243,6 +245,22 @@ function create_cpio {
     # gpgv components
     cp tmp/usr/bin/gpgv rootfs/usr/bin/
 
+    # ifupdown components
+    cp tmp/etc/default/networking rootfs/etc/default/
+    cp tmp/etc/init/network-interface-container.conf rootfs/etc/init/
+    cp tmp/etc/init/network-interface-security.conf rootfs/etc/init/
+    cp tmp/etc/init/network-interface.conf rootfs/etc/init/
+    cp tmp/etc/init/networking.conf rootfs/etc/init/
+    cp tmp/etc/init.d/networking rootfs/etc/init.d/
+    cp tmp/etc/network/if-down.d/upstart rootfs/etc/network/if-down.d/
+    cp tmp/etc/network/if-up.d/upstart rootfs/etc/network/if-up.d/
+    cp tmp/lib/ifupdown/settle-dad.sh rootfs/lib/ifupdown/
+    cp tmp/sbin/ifup rootfs/sbin/
+    cd rootfs/sbin
+    ln -s ifup ifdown
+    ln -s ifup ifquery
+    cd ../..
+
     # iproute2 components
     cp tmp/bin/ip rootfs/bin/
     cp tmp/bin/ss rootfs/bin/
diff --git a/update.sh b/update.sh
index 597b966..9466d8e 100755
--- a/update.sh
+++ b/update.sh
@@ -31,6 +31,7 @@ packages+=("e2fslibs")
 packages+=("e2fsprogs")
 packages+=("f2fs-tools")
 packages+=("gpgv")
+packages+=("ifupdown")
 packages+=("iproute2")
 packages+=("lsb-base")
 packages+=("netbase")

```
I call this an atomic commit because if you later decide you don't want to use the `ifupdown` components, you can *revert* that commit and the changes in both `build.sh` and `update.sh` will be reverted, but it won't destroy/revert anything else of the project.  
If we would make a single commit of the earlier shown changes to *README.md* and later decide that we don't want to add a donation section after all, a revert of that commit would also undo the improvements to the *First Boot* section. 

An alternative way to show the *diff*erence between 2 commits can be done with `git diff 452be53a..625000d5`. This will show almost the same info as the earlier `git show` command but `git diff` takes a `<oldest>..<newest>` commit range and does not show the commit meta data.  
It's important to note that it won't inlude the changes from the `<oldest>` commit, but the changes after/since that commit!  
The main benefit of `git diff` is that you can get a view the *diff*erence over several commits. But to see the changes of a single commit, `git show <commit-id>` is easier to use.  
While the `<commit-id>` is actually 625000d5290433e62ba408c8585c04ec8a89ec0e (for example), usually the first 7-9 characters are sufficient to indicate the commit and as it saves you a lot of typing and therefor errors, you'd usually use the shortened form of the `<commit-id>` 


So all in all, this is a pretty messed up situation.  


Feel free to do this often, even if it's just to *stash* away your incomplete changes because you need to do something else. In a next section I'll explain how you can rewrite your git history by reordering and squashing (=merging) commits.


## Interacting with the rest of the world
To get more information about remotes, issue the `git remote` command:
```sh
diederik@bagend:~/dev/raspbian-ua-netinst$ git remote -v
origin  https://github.com/Mausy5043/raspbian-ua-netinst (fetch)
origin  https://github.com/Mausy5043/raspbian-ua-netinst (push)
```
I (basically) always add the `-v` switch, which stands for verbose, so you'll get more useful info consisting of the remote name and the `push` and `fetch` urls.
With `fetch` you'll retrieve the lastest data from your remote and with `push` you can share your changes, which by default would be your own fork on GitHub.  
This is a good thing &trade;.
