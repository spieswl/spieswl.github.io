---
layout:             post
title:              "Running a Git Server on a Synology NAS"

description:        "Discrete NAS systems are increasingly used by general consumers as service-providers-in-a-box. In this post, I talk about doing just that exact thing. This post covers how I set up my Synology DS213+ to act as a local Git server."
keywords:           git, synology, git server, NAS, homelab
tags:               [Git, Homelab]

specifics:
    images:         "2018/synology-git-server"

published:          true
---

Sometime last year, I got the idea to start poking around Synology forums and support venues to see if someone had gone through the effort to get a `Git` server up-and-running on any Synology NAS models. I figured that someone HAD to have gone through the effort, since one of the trends in computing is to build up more sophisticated "homelab" systems. After all, the computational horsepower needed to run a version control system is easily provided by even the most barebones of NAS devices. As for the target hardware, I currently operate a Synology **[DS213+](https://global.download.synology.com/download/Document/Hardware/DataSheet/DiskStation/13-year/DS213/enu/Synology_DS213_Data_Sheet_enu.pdf)** NAS that is used mostly for media storage purposes. In addition to being a data store for family media (_pictures, etc._), I also keep various technical and creative projects in there so they can be easily accessed across my Windows and Ubuntu installs. Using my DS213+ as a `Git` handler is the next step in making my DS213+ the one-stop shop for all my side projects. As it turns out, Synology has a great first-party-developed package for setting up and supporting a `Git` server. There are also some decent **_how-to_** guides to enable remote access and perform initial server setup, although most of the guides I found were usually missing various important steps.

In an effort to document this process for myself (_and to fill in some of those gaps_), I am going to outline the process of how I set up my DS213+ to act as a `Git` server.

One note before starting the walkthrough: I could be using private repositories on GitHub or Bitbucket as an alternative to going through this process. There are certainly some benefits to that choice. These solutions would accessible everywhere, I do not have to handle system administration work, and there are more useful features on the larger, professional providers. On the other hand, I place a high value on privacy, so throwing code and data (_some of which may be heavily personalized_) onto a third-party system does not sit right with me. Not to mention that I can use a local server _in addition_ to professional offerings, such as those already mentioned, if I should need to.

If you are going to follow this as a guide, make sure your version of **DiskStation Manager** (or **DSM**) is up-to-date and refer to [the Applied Models list](https://www.synology.com/en-us/dsm/packages/Git) on Synology's website to see if you have a supported server model. You will also need administrator access in order to install the Git Server package, configure permissions, and establish or change repositories on the server itself.

<hr>

##### Installing and Configuring Git Server

1. Log into your Synology as the `admin`, go to the **Package Center**, and install the _Git Server_ package.

<div class="post-image">
    <a href="{{ site.url }}/{{ site.assets.posts }}/{{ page.specifics.images }}/01_git_server_install.png">
        <img src="{{ site.url }}/{{ site.assets.posts }}/{{ page.specifics.images }}/01_git_server_install.png" width="640" style="padding:4px 4px 4px 4px">
    </a>
</div>

I highly recommend setting up SSH access so that push-pull operations do not require username/password credentials every time you interact with the remote server. In case this applies, Windows `Git` tools (such as _TortoiseGit_ using _PuTTy_ to connect) also will not require credentials every time you interact with your repositories, assuming you have your key situation settled. DSM is a slightly tweaked flavor of GNU/Linux, so typical methods for enabling and securing access via SSH will apply here. If you are not familar with SSH and why opening it up could be bad for you, you will have to find an alternate method for getting command-line access to the server.

##### Setting up SSH Access on your Synology

* 2) Go to the **Control Panel > Advanced Mode > Terminal & SNMP**.
* 3) Click the *Enable SSH Service* option under the **Terminal** tab. Change your default SSH port, if desired.

**NOTE:** Synology restricts SSH/Telnet connections to members of the **Administrators** group on any particular Synology device. If you don't already have a `git`-access-specific user with administrator privileges, it is a good time to create one. From here on out, if I need to refer to this specific user, I will do so with `<git-username>`. Just replace `<git-username>` with your user when following these instructions.

* 4) Go to the **Control Panel > User** section and create the `<git-username>` user.
* 5) Add this new user to the _Administrators_ group. Depending on your repository location, you may need to add _Read/Write_ permissions for the `root` part of the NAS filesystem inside `/volume1`.
* 6) Go to the **Main Menu > Git Server** and click the _Allow Access_ check box for `<git-username>`.

**NOTE:** From here on out, I will be using the command line (and connecting via SSH instead of the Synology browser-based front end). In order to follow along, you will need the IP address for your Synology NAS on the network. I will refer to that IP as `<nas-address>` where necessary. Just replace `<nas-address>` with your server's IP address when following these instructions. If you are using a Windows machine, you may need a terminal emulator to continue, such as **[PuTTy](https://www.putty.org/)**.

**NOTE:** Now would be the time to have a public RSA key file ready to facilitate easy access to your NAS server. Good tutorials to follow are [here](https://www.debian.org/devel/passwordlessssh) or [here](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-1604).

* On the NAS server, log in as `admin` and configure SSH to streamline repository access in the future. If you get an error about the "homes" directory not existing (or something similar), look on [this Synology support topic](https://www.synology.com/en-global/knowledgebase/DSM/help/DSM/AdminCenter/file_user_advanced) and modify the general user settings to _Enable User Home Service_:

```bash
$ ssh admin@<nas-address>
$ sudo -i

$ cd /volume1/homes/<git-username>/
$ mkdir .ssh
```

* On the local machine, copy your public key to the newly-made `.ssh` folder on the NAS:

```bash
$ scp ~/.ssh/id_rsa.pub admin@<nas-address>:/volume1/homes/<git-username>/.ssh
```

* Back on the NAS server, use the public key you transferred over to act as the lone authorized user in the `<git-username>` profile and set file permissions accordingly:

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

#### Creating a Repository

Following along with the assumption that the `<git-username>` user will access git repositories either in `/volume1/root/` or just `/volume1/` itself, we need to actually initialize the repository now. I am assuming `/volume1/git/<dummy-name.git>` is the target repository; make changes to the target path or repository name as required. _You will have to do this step for every individual repository that is hosted on the server._

```bash
$ # SSH into the NAS server as <git-username>
$ ssh <git-username>@<nas-address>

$ # Navigate to your prospective repository location and initialize a new, shared repository
$ mkdir /volume1/git
$ cd /volume1/git
$ git init --bare --shared <dummy-name.git>

$ # It also may be a good idea to open up the permissions for the git repository, just in case
$ chmod -R 766 <dummy-name.git>
3
$ # Finally, update the pack information for the git service running on the NAS
$ cd <dummy-name.git>
$ git update-server-info
```

#### Cloning a Repository

Try this command in a folder on your local machine:

```bash
$ git clone ssh://<git-username>@<nas-address>/volume1/git/<dummy-name.git>
```

You can now push to and pull from the remote repository on your local server!

<hr>

#### References and Helpful Extensions

[How to login to DSM with root permission via SSH/Telnet](https://www.synology.com/en-us/knowledgebase/DSM/tutorial/General/How_to_login_to_DSM_with_root_permission_via_SSH_Telnet)<br>
[Configure Synology NAS as Git Server](https://gist.github.com/walkerjeffd/374750c366605cd5123d)<br>
[Log in to a Synology DiskStation using SSH keys as a user other than root](https://www.chainsawonatireswing.com/2012/01/16/log-in-to-a-synology-diskstation-using-ssh-keys-as-a-user-other-than-root/)<br>
[Git Server on Synology NAS: Installation and Configurations](http://blog.netgloo.com/2015/04/20/git-server-on-synology-ds115j-installation-and-configurations)<br>

[How to Host a Website on a Synology NAS](https://www.synology.com/en-us/knowledgebase/DSM/tutorial/Application/How_to_host_a_website_on_Synology_NAS)<br>
