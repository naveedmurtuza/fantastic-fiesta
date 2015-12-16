---
layout:     post
title:      Pixyll in Action
date:       2014-06-10 12:31:19
summary:    See what the different elements looks like. Your markdown has never looked better. I promise.
categories: jekyll pixyll
---
##### Introduction

This blog post guides you to turn your old dusty PC/Laptop to NAS server. I am using using my old laptop (infact my first, bought _wayyybaack_ in 2008!).
Dell Inspiron 1501!   Its a AMD turion X2 with 2 GB of RAM and 100 GB HDD.
  
I have selected [Nas4Free](http://www.nas4free.org) as the OS, primarily for its low hardware requirements. [FreeNAS](#), the other excellent alternative has pretty high requirements. If your old laptop turns on and boots, you are good to go. If it doesnt support booting from USB drive then you have to burn a copy of the OS to a CD and install it on the internal HDD. Fortunately my inspiron supports booting from USB drive, so I will be installing NAS4Free on a USB stick.

##### You'll Get:
At the end of the tutorial,  you will have a powerful NAS box with a file shares for every user, Bittorrent support, your own local DropBox using ownClowd, Media Server to stream videos/audio to other supported clients and ability to access all the features from outside your home network.

##### You'll Need:
1. __Nas4Free:__ Grab a copy of nas4free Live USB image from the downloads section [http://www.nas4free.org/index.php?id=6] and unzip it to get your *.img file.
2. __USB memory sticks:__
    You will need 2 USB memory sticks of at least 512MB in size.(One will act as your installation media , the other for installing the OS)
3. __NAS Box:__
    Your old dusty PC that will be turning to a NAS box.
4. __Management PC:__
    This is your regular PC/laptop from where you will be managing/configuring the NAS box.
5. __Additional Softwares__
    * [Win32 Disk Imager](http://sourceforge.net/projects/win32diskimager/files/latest/download)
    * [PuTTy]() - for sshing into your NAS box
    * [ConEmu](https://conemu.github.io/) is completely optional. It is a Windows console window enhancement, which presents multiple consoles and simple GUI applications as one customizable tabbed GUI window with various features. 

##### Installation

###### Creating installation media:
Insert your "source" USB stick and note down the drive letter (Win + E to fire up your explorer). Open the disk imager utility, browse to the downloaded NAS4Free image file, select the appropriate drive letter from the dropbox and click write. Once it finishes you will have your installation media ready.

###### Boot
Restart your computer and select USB from the boot menu. If your BIOS does'nt have boot menu, set the first boot device  to your USB Stick from the BIOS. This step can be done from any PC you are comfortable with. My inspiron had some dispay issues and a few broken keys on the keyboard. So I did all the installation on the other 'management' PC and connected the final USB stick to boot from.

###### Install
Once you get booted into the NAS4Free boot option menu.

[pic]

Wait for  the timer run down automatically (or press 1 if you want to save a second or two ;) ) to start NAS4Free in the default installation mode.The system should boot into NAS4Free Live and you should be presented with the menu like below.

[pic]

Now you should also insert your "destination" USB memory stick and choose option 9 to be presented with  an another menu. Choose 1 to install the embedded version of the OS, which is portable and easier to upgrade later. A simple wizard will guide you to select the source installation disk, destination disk (probably be labelled as da0 or da1), accept the defaults for the swap partition to finally start the installation process.

[pic second options menu] [pics - wizard]

After the installation shutdown the system by choosing option #8. Once your system has shutdown, remove the "source" USB stick. Move the NAS4Free "embedded" stick into its permanent location if necessary and boot into your brand new NAS4Free box. You will be presented with the familiar NAS4Free menu on the console with your IP address.

[pic]

On your management computer, fire up your browser and navigate to http://ipaddress. Youll see the WebGUI of NAS4Free box that we will be using to configure the NAS4Free box. Logon (default credentials are `admin` and `nas4free`)a and make yourself comfortable with GUI.

##### Configuring

Before we start configuring, we need to get a hang of some of the shell commands and file permissions. Though you can find extensive information on the internet on this, but i have found [LinuxCommands](http://www.linuxcommand.org) to be an excellent resource.

`cd` `pwd` `ls` `mkdir` `rm` `chown` `chmod` `chgrp`

Still with me? :) OK lets start configuring.

> After making any change to system using the WebGUI, you have to click "Apply Changes" to commit the change. So remember to hit "Apply Changes" whereever applicable.

* __Securing your logon__

    The first step towards security is changing your default logon password by navigating to `System > Password`. While you are at `System` page enable `SSL` by changing the protocol from `HTTP` to `HTTPS`. Though not required in a home network, Neverthless it is better to enable as it costs nothing :smile: . If you dont want to see that ugly certifate not trusted warning, then install the certificate to trusted chain.
[pics]

* __Static IP__

    The server should always be assigned a static ip address. Imagine jumping out of your couch to physically go to the Nas box just to check the ip address alloted by the DHCP after a system reboot, to enable you to access the WebGUI again. Tedious! To assign a static IP address, you need to logon to the router's management console. This step differs depending on the router model. Basically, we will be instructing our router to issue the same ip address to our NAS box, on every DHCP request.

    In Belkin you would do this by ...

    Also a nice, but optional trick is to modify your hosts file to add a host name to point to the nas4free ipaddress. Then instead of typing in the ipaddress to access the webGUI, u can type a cool hostname like    `https://homenas`
    To do so, on a Windows machine, open up your explorer `Win + E` and navigate to `<WINDIR>\System32\drivers\etc` and open the `hosts` file in a notepad and append a line
    ```
    192.168.2.100     homenas
    ```
    to the end of that file.
    
    
    ```
    You will need administrator access to save files to that location
    ```
    
* __Users & Groups__

    Users and Groups are used for access control — that is, to control access to the services provided by the NAS box. A `user` is _anyone_ who uses a computer (NAS box in our case).So, basically we create a user for every one who uses the network or wants to access the NAS. Multiple users can be "clubbed" together to form `group` to ease the management of multiple users. Instead of assigning permissions to multiple users individually, we can assign those permissions on a group containig those users.
    Lets start by creating a group. Navigate to `Access > Users and Groups` and switch to the `Groups` tab. Add a new group by clicking on the plus symbol.Provide group name and description in the respective fields. I'll name my group `home`.
    
    [pic]
    
    Now switch over to `Users` tab and create as many users as you require. For every user you create, select `home` as their primary group. Preferably you should create one of your user with `root` permissions by adding him to `wheel`, `transmission`, `sshd` groups. Be sure to select shell as `bash` for any user you want to give SSH access Also create a user by the name `torrents` and add him to `home` and `transmission` groups to manage the torrents.
    
    [pic]
    
* __Import HDDs__
    <todo>

* __SSH__
    
    Enable SSH access by going to `Services` dropdown and click `SSH`. After the page loads click on Enable checkbox located at the top right corner, leave the defaults as is, and click `Save & Restart`

    Open up PuTTy, enter your server IP address and click OK. Alternative you can save the session, to save you from typing the IP address everytime you connect. On the putty terminal type in your username an password and voila! you are in your NAS box. This user should be in `sshd` group and its shell selected to `bash`.
    
    If you are ConEmu user, or just downloaded it and you are liking it, then configure ConEmu to use PuTTY[^1]. Move the PuTTY executable to suitable location. Generally, I have a tools directory on C:\ drive where I dump all the tools I regularly use. Infact my ConEmu is there as well. Then add this to your `PATH`.
    
    Crank open ConEmu and open the Settings dialog (Win+Alt+P). On the menu look under `Startup > Tasks` and click the '+' button. You'll see
    
    [pic]
    
    Enter ```cmd putty.exe -new_console -load "yoursavedsession" ```

    
* __File Structure__
    Since the NAS boxh will be used by multiple users accross the network for their file storage needs, we need to have our files properly organized, so as not to clutter the disk space. The structure I am using is as follows. Ofcourse you can change the structure if you are not comfortable with it.

<code>
<pre>
    data
      |---users
            |---user1
            |---user2
            +---usern
      |---public
            |---torrents
            |---downloads
            |---video
            |---audio
            +---photos
      +---transmission
              |---config
              +---incomplete
</pre>
</code>
    The public directory will be accessible by everyone in the `home` group. The directories in the `users`directory can be accessed by their respective user only. Like user1 will be allowed access to user1 directory only.
    If you are ok with directory structure, then issue the below command in ssh console.
    
```bash
mkdir -p /mnt/data/{users/{user1,user2},public/{torrents,downloads,video,audio,photos},transmission/{config,incomplete}}
```
    
    
* __Permissions__

    
    Lets control access to those directories. Login to the server using SSH. First lets give everyone in the `home` group access to the `public` directory.


```console
# su -root
# cd /mnt/data
# chgrp -R home public
```


Now change the ownership of the user directories to their respective users.
    
```console
# cd /mnt/data/users
# chown -R user1:home user1
```
    
Repeat the above command for every user in the system.
    
    
* __CIFS/SMB service__

  

---

[^1]: http://thecrumb.com/2013/03/04/configuring-conemu-and-putty/
