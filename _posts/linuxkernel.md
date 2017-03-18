---
layout: blog
title: Linux
permalink: /linux/
---

### 1.1 Introduction

This tutorial is the layman's guide to hacking the Linux Kernel. The Linux foundation in general and the Kernel documentation in particular have intimidating documentation which put most of the newbies off. Well this blog in some level is an attempt to level the playing ground and give a setup guide and a follow-up instruction set to get your first patch merged into the main tree and hence making you a part of the largest open source project in the world, so cheers! We start off by setting up the system with some pre-requisites, making and compiling the Linux Kernel. Once that is done we can straight away dive into the C code which makes up the kernel and fix some annoying little bugs! **_ NOTE: The entire process is carried out in a 64-bit native Ubuntu 16.04 LTS version. To set up a virtual machine setup refer [here](https://kernelnewbies.org/OutreachyfirstpatchAlt?action=show&redirect=OPWfirstpatchAlt)_**

### 1.2 Pre-requisites

1.  Setting up our editor (vim in my case Open up ~/.vimrc and add in the following lines,

    <pre>filetype plugin indent on</pre>

    <pre>syn on se title</pre>

    <pre>set tabstop=8
    set softtabstop=8
    set shiftwidth=8
    set noexpandtab</pre>

Once this is done, we proceed to change this editor as our default editor by

> <pre>sudo update-alternatives --config editor</pre>

Here you will be prompted with options listing out your editors.  Choose appropriate editor(vim in this case). Next we proceed towards installing a mail transfer client. To send patches to Linux kernel, mutt and git email is recommended.(There are stringent mailing rules, as you will find out soon, so brace yourself).

> <pre>sudo apt-get install mutt</pre>

Once the client is installed we have to appropriately configure it to use SMTPS. For this enable IMAP in your Gmail account. Once that is done we create a ~/.muttrc file and add the following configurations on it ![muttrc.png](https://rahulkrishnanlive.files.wordpress.com/2016/11/muttrc3.png) NOTE : Ignore those settings in comments section( followed by #) .Fill in your user name and password in the given sections . To check if your mutt is working fine, type in **mutt** into the terminal, after passing couple of certificate tests it will open your inbox and you are now ready to send mails via a teminal MTA. 2\. Now If you already haven't setup git in your system, install it and setup user name and password as follows

> <pre>git config --global user.name `"Rahul Krishnan"`</pre>
>
> <pre>git config --global user.email "mrahul.krishnan@gmail.com"</pre>

NOTE : It is **absolutely important** that this git user name is your full name and your email is the same as given in the previous configuration. 3\. Git cloning the Greg-Kroah-Hartman's staging tree repository  (This might take a while)

> <pre>mkdir -p git/kernels
> cd git/kernels</pre>

> <pre>git clone -b staging-testing git://git.kernel.org/pub/scm/linux/kernel/git/gregkh/staging.git</pre>

When that is done, change into the cloned directory.

> <pre>cd staging</pre>

_**From here on you shall create another local branch and switch from master branch to this new branch. From here you can make changes and send in patches.(As shown down below)** _Now we can either choose to work in the running kernel or compile a new one to be safe. I am continuing with the present one. Though if you are still interested in setting up a new kernel and compiling it follow [this](https://kernelnewbies.org/OutreachyfirstpatchSetup?action=show&redirect=OPWfirstpatchSetup)

### 2.1 Working in staging

From now we will be working in the following directory.**(~/git/kernels/staging)** Explore the staging/drivers/staging directory to find an array of unlimited drivers, yes and that means unlimited bugs too. To find out the error and and to further patch it will be the main focal points of the article. By a simple git status from the working directory will show that you are in staging-testing. We will create another branch and start working in our patch right away, for that type in the following command in the terminal

> <pre>git checkout -b first-patch</pre>

This creates another branch called first-patch. "When you create your first application clean-up patches, you want to create them on top of the latest commit from the staging-testing tree". So type in the following

> <pre>**git fetch origin**
> git log origin/staging-testing (This is just the log of commits)
> **git rebase origin/staging-testing**</pre>

### 3.1 Fixing your first Linux Kernel bug

If you pick a driver in staging, you can run the script that checks whether a file conforms to kernel coding style, known as the **checkpath.pl**

> <pre>perl scripts/checkpatch.pl -f drivers/staging/greybus/sdio.c | less</pre>

In this instance I chose to run the script over sdio.c file and in return it returns the number of _errors, warnings_ and _checks_. For the beginner level patch submit, we start working with the warnings and work our way through some errors in the near future. (It is best to stay away from the errors in the beginning though). Pick a warning and successfully get it fixed. Once you are sure of the change, run checkpath.pl again and double check. For this minor fix it would be safe to omit the recompiling of drivers, but once we start working on bigger projects it's safe to deploy it once locally, by recompiling the kernel and making it again. Now use the following git commands to first, see the changes that have been made and then finally to commit those.

> <pre>git diff</pre>
>
> <pre>git add  // this is to add the change</pre>
>
> <pre>git commit -s -v   // with commit message</pre>

Finally to add the commit message and send the mail, refer this document ([Patch Philosophy](https://kernelnewbies.org/PatchPhilosophy)). It is really well documented and should suffice, If not please dont hesitate to ping me [Contact Cap'n Tech](https://rahulkrishnanlive.wordpress.com/contact/)

### 3.2 Creating a patch

Now that the change has been committed and added we create the patch which can be sent via mutt. For that type in the following **_ Provided this is the (single) last commit in your working directory_**

> <pre>git format-patch -o /tmp/ HEAD</pre>

Now that we have generated the patch we can send it via mutt.

> <pre>mutt -H /tmp/0001-</pre>

A sample of a completed patch submitted to corresponding mailing lists and maintainers is given down below for your quick reference. ![patcc.png](https://rahulkrishnanlive.files.wordpress.com/2016/11/patcc.png) Finally before I end this tutorial, it's important to know who to send the patch to, or else it would just be ignored, if it were to make it to the wrong mailing lists.

> **perl scripts/get_maintainer.pl -f  (path-to-your-file)**

This will return a list of maintainers and mailing lists to send to.

### 4.1 Conclusion

Hope you are reading this after having your patch accepted. Trust me it adds more than just a smile to my face if thats the case. Welcome to Linux Kernel family. Hope you had a pleasant time (irony of it all) , if you have any queries, doubts still unanswered, feel free to ping me. Hope to see you again for more awesome content, till then _CIAO_.
