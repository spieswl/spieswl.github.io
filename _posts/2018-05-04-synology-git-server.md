---
layout:             post
title:              "Running a Git Server on your Synology NAS"
description:        "Discrete NAS systems are useful for data storage, but are increasingly used for hosting local services, such as a git server. I describe how I set up my Synology NAS to host locally-available git repositories."
keywords:           git, networking, synology, git server, NAS
tags:               [git, Synology, NAS Servers]

folders:
  images:           "synology-git-server"                   # This path is post-dependent; don't forget to change it!

published:          true
---

Late last year, I got the idea to start poking around **Synology** forums and support venues to see if someone had figured out how to run a ***Git*** server on Synology NASes. After all, the performance needed to run a shared Git server is easily provided by even the most spartan of Synology devices. I currently own a **13-series NAS running DiskStation Manager (`DSM`) 6.1** which, in addition to holding music, movies, and pictures, also gives the capability to back up content like school work, personal projects, and things not necessarily appropriate to upload to a publicly available system like **[GitHub](https://github.com/)**. It turns out, Synology has a great add-on package for `DSM` and there are decent how-to guides out there, although each guide is typically missing at least one critical step. This guide will recreate the effort I went through to set up my own Git server on a practically-untouched Synology NAS.

Although it is true that private repository facilities exist on service providers like GitHub (which may negate the need for this guide entirely), you may be an individual that wants more explicit control over your repository or you may want to keep specific data from being on a site hosted by another organization. In my case, I use my Git server to keep a plethora of work on two Ubuntu and two Windows 10 operating systems all up-to-date. All it takes is running a mere handful of commands across all my devices - a truly wonderful experience the first time you question whether or not you updated the work on your laptop last night. In the event that you have a Synology device, you want a little more control, and you can handle using the command line and connecting via **SSH**, read on.

Before starting anything, make sure your version of `DSM` is up-to-date and refer to [this list](https://www.synology.com/en-us/dsm/packages/Git "Git Server Add-on Package Specs") to see if you have a supported server model. You will also need administrator access in order to install the Git Server package, configure permissions, and establish or change repositories on the server itself.

#### Installing and Configuring Git Server

Let's get the Git Server package installed. 

1. Log into your Synology as the `admin`, go to the **Package Center**, and install the *Git Server* package.

<img src="{{ site.url }}/{{ site.post_assets }}/{{ page.folders.images }}/01_git_server_install.png" style="width:640px; padding:4px 4px 4px 4px;display: block">

You will need to set up SSH access, so that simple `git` commands or Windows tools (*TortoiseGit* using *PuTTy* to connect, for example) don't require credentials every time you need to interact with your repositories.

#### Setting up SSH Access on your Synology

* 2) Go to the **Control Panel > Advanced Mode > Terminal & SNMP**.
* 3) Click the *Enable SSH Service* option under the **Terminal** tab.

**NOTE:** Synology restricts SSH/Telnet connections to members of the **Administrators** group on any particular Synology device. If you don't already have a `git`-access-specific user with administrator privileges, it is a good time to create one. From here on out, if I need to refer to this specific user, I will do so with `<git-username>`. Just replace `<git-username>` with your user when following these instructions.

* 4) Go to the **Control Panel > User** section and create the `<git-username>` user.
* 5) Add this new user to the *Administrators* group. Depending on your repository location, you may need to add *Read/Write* permissions for the `root` part of the NAS filesystem inside `/volume1`.
* 6) Go to the **Main Menu > Git Server** and click the *Allow Access* check box for `<git-username>`.

<img src="{{ site.url }}/{{ site.post_assets }}/{{ page.folders.images }}/02_added_access.png" style="width:590px; padding:4px 4px 4px 4px;display: block">

**NOTE:** From here on out, I will be using the command line (and connecting via SSH instead of the Synology browser-based front end). In order to follow along, you will need the IP address for your Synology NAS on the network. I will refer to that IP as `<nas-address>` where necessary. Just replace `<nas-address>` with your server's IP address when following these instructions. If you are using a Windows machine, you may need a terminal emulator to continue, such as **[PuTTy](https://www.putty.org/)**.

**NOTE:** Now would be the time to have a public RSA key file ready to facilitate easy access to your NAS server. Good tutorials to follow are [here](https://www.debian.org/devel/passwordlessssh) or [here](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-1604).

* On the NAS server, log in as `admin` and configure SSH to streamline repository access in the future. If you get an error about the "homes" directory not existing (or something similar), look on [this Synology support topic](https://www.synology.com/en-global/knowledgebase/DSM/help/DSM/AdminCenter/file_user_advanced) and modify the general user settings to *Enable User Home Service*:

```bash
$ ssh admin@<nas-address>
$ sudo -i

$ cd /volume1/homes/<git-username>/
$ mkdir .ssh
```

* On the local machine, copy your public RSA key to the newly-made `.ssh` folder on the NAS:

```bash
$ scp ~/.ssh/id_rsa.pub root@<nas-address>:/volume1/homes/<git-username>/.ssh
```

* Back on the NAS server, grant your public key access as an authorized user to the `<git-username>` profile and set permissions accordingly:

```bash
$ ssh admin@<nas-address>
$ sudo -i

$ cd /volume1/homes/<git-username>/.ssh
$ mv id_rsa.pub authorized_keys

$ cd /volume1/homes/
$ chown -R <git-username>:administrators <git-username>/.ssh
$ chmod 750 <git-username>
$ chmod 700 <git-username>/.ssh
$ chmod 600 <git-username>/.ssh/authorized_keys
```

#### Creating a Dummy Repository

Following along with the assumption that the `<git-username>` user will access git repositories either in `/volume1/root/` or just `/volume1/` itself, we need to actually initialize the repository now. I will assume `/volume1/git/<dummy-name.git>` is the target repository; make changes to the target path or repository name as required. *You will have to do this step for every distinct repository you establish on the server.*

```bash
$ # SSH into the NAS server as <git-username>
$ ssh <git-username>@<nas-address>

$ # Navigate to your prospective repository location and initialize a new, shared repository
$ mkdir /volume1/git
$ cd /volume1/git
$ git init --bare --shared <dummy-name.git>

$ # It also may be a good idea to open up the permissions for the git repository, just in case
$ chmod -R 766 <dummy-name.git>

$ # Finally, update the pack information for the git service running on the NAS
$ cd <dummy-name.git>
$ git update-server-info
```

#### Cloning a Repository

Try this command in a sandbox folder on your local machine:

```bash
$ git clone ssh://<git-username>@<nas-address>/volume1/git/<dummy-name.git>
```

**Viola!** You can now push to and pull from the repository on your Synology NAS.

#### References and Helpful Extensions

[How to login to DSM with root permission via SSH/Telnet](https://www.synology.com/en-us/knowledgebase/DSM/tutorial/General/How_to_login_to_DSM_with_root_permission_via_SSH_Telnet)<br>
[Configure Synology NAS as Git Server](https://gist.github.com/walkerjeffd/374750c366605cd5123d)<br>
[Log in to a Synology DiskStation using SSH keys as a user other than root](https://www.chainsawonatireswing.com/2012/01/16/log-in-to-a-synology-diskstation-using-ssh-keys-as-a-user-other-than-root/)<br>
[Git Server on Synology NAS: Installation and Configurations](http://blog.netgloo.com/2015/04/20/git-server-on-synology-ds115j-installation-and-configurations)<br>

[How to Host a Website on a Synology NAS](https://www.synology.com/en-us/knowledgebase/DSM/tutorial/Application/How_to_host_a_website_on_Synology_NAS)<br>