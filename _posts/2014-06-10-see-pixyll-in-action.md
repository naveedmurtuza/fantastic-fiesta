---
layout:     post
title:      Pixyll in Action
date:       2014-06-10 12:31:19
summary:    See what the different elements looks like. Your markdown has never looked better. I promise.
categories: jekyll pixyll
---
##### Introduction

This blog post guides you to turn your old dusty PC/Laptop to NAS server. I am using using my old (infact my first, bought _wayyybaack_ in 2008!) laptop.
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

* Securing your logon
The first step towards security is changing your default logon password by navigating to `System > Password`. While you are at `System` page enable `SSL` by changing the protocol from `HTTP` to `HTTPS`. Though not required in a home network, Neverthless it is better to enable as it costs nothing :smile: . If you dont want to see that ugly certifate not trusted warning, then install the certificate to trusted chain.
[pics]

* Static IP
    The server should always be assigned a static ip address. Imagine jumping out of your couch to physically go to the Nas box just to check the ip address alloted by the DHCP after a system reboot, to enable you to access the WebGUI again. Tedious! To assign a static IP address, you need to logon to the router's management console. This step differs depending on the router model. Basically, we will be instructing our router to issue the same ip address to our NAS box, on every DHCP request.

    In Belkin you would do this by ...

    Also a nice, but optional trick is to modify your hosts file to add a host name to point to the nas4free ipaddress. Then instead of typing in the ipaddress to access the webGUI, u can type a cool hostname like    `https://homenas`
    To do so, on a Windows machine, open up your explorer `Win + E` and navigate to `<WINDIR>\System32\drivers\etc` and open the `hosts` file in a notepad and append a line
    ```
    192.168.2.100     homenas
    ```
    
    
    ```
    You will need administrator access to save files to that location
    ```
    
* Users & Groups
    
* Import HDDs
* File Structure
* 
There is a significant amount of subtle, yet precisely calibrated, styling to ensure
that your content is emphasized while still looking aesthetically pleasing.

All links are easy to [locate and discern](#), yet don't detract from the [harmony
of a paragraph](#). The _same_ goes for italics and __bold__ elements. Even the the strikeout
works if <del>for some reason you need to update your post</del>. For consistency's sake,
<ins>The same goes for insertions</ins>, of course.

### Code, with syntax highlighting

Here's an example of some ruby code with line anchors.

{% highlight ruby lineanchors %}
# The most awesome of classes
class Awesome < ActiveRecord::Base
  include EvenMoreAwesome

  validates_presence_of :something
  validates :email, email_format: true

  def initialize(email, name = nil)
    self.email = email
    self.name = name
    self.favorite_number = 12
    puts 'created awesomeness'
  end

  def email_format
    email =~ /\S+@\S+\.\S+/
  end
end
{% endhighlight %}

Here's some CSS:

{% highlight css %}
.foobar {
  /* Named colors rule */
  color: tomato;
}
{% endhighlight %}

Here's some JavaScript:

{% highlight js %}
var isPresent = require('is-present')

module.exports = function doStuff(things) {
  if (isPresent(things)) {
    doOtherStuff(things)
  }
}
{% endhighlight %}

Here's some HTML:

{% highlight html %}
<div class="m0 p0 bg-blue white">
  <h3 class="h1">Hello, world!</h3>
</div>
{% endhighlight %}

# Headings!

They're responsive, and well-proportioned (in `padding`, `line-height`, `margin`, and `font-size`).
They also heavily rely on the awesome utility, [BASSCSS](http://www.basscss.com/).

##### They draw the perfect amount of attention

This allows your content to have the proper informational and contextual hierarchy. Yay.

### There are lists, too

  * Apples
  * Oranges
  * Potatoes
  * Milk

  1. Mow the lawn
  2. Feed the dog
  3. Dance

### Images look great, too

![desk](https://cloud.githubusercontent.com/assets/1424573/3378137/abac6d7c-fbe6-11e3-8e09-55745b6a8176.png)

_![desk](https://cloud.githubusercontent.com/assets/1424573/3378137/abac6d7c-fbe6-11e3-8e09-55745b6a8176.png)_


### There are also pretty colors

Also the result of [BASSCSS](http://www.basscss.com/), you can <span class="bg-dark-gray white">highlight</span> certain components
of a <span class="red">post</span> <span class="mid-gray">with</span> <span class="green">CSS</span> <span class="orange">classes</span>.

I don't recommend using blue, though. It looks like a <span class="blue">link</span>.

### Footnotes!

Markdown footnotes are supported, and they look great! Simply put e.g. `[^1]` where you want the footnote to appear,[^1] and then add
the reference at the end of your markdown.

### Stylish blockquotes included

You can use the markdown quote syntax, `>` for simple quotes.

> Lorem ipsum dolor sit amet, consectetur adipiscing elit. Suspendisse quis porta mauris.

However, you need to inject html if you'd like a citation footer. I will be working on a way to
hopefully sidestep this inconvenience.

<blockquote>
  <p>
    Perfection is achieved, not when there is nothing more to add, but when there is nothing left to take away.
  </p>
  <footer><cite title="Antoine de Saint-Exupéry">Antoine de Saint-Exupéry</cite></footer>
</blockquote>

### There's more being added all the time

Checkout the [Github repository](https://github.com/johnotander/pixyll) to request,
or add, features.

Happy writing.

---

[^1]: Important information that may distract from the main text can go in footnotes.
