# Lab 1

## Goals

* Create your account
* Create grader account
* Install updates
* Install packages
    * OpenSSH, vim, curl, git, fortune, man, Nginx
* Configure Nginx

## Create your virtual machine/accounts

Get a copy of VMware Fusion (macs) or VMware Workstation (windows/linux) from the campus [OnTheHub](https://csuchico.onthehub.com/) page. This will be a free 1 year license for this software.

### Download Ubuntu ISO

Initially we will be using Ubuntu 20.04, so download a copy of the server version of Ubuntu 20.04 from [Ubuntu's website](https://ubuntu.com/download/server). Make sure you click on *"Option 2: Manual server installation"* to download the server ISO disk image to use to setup a new VM from.

### Setup VM

Use the VMware *"create new VM dialog"* to create a new VM that uses the ISO file you downloaded from Ubuntu's website. This should be reasonably intuitive/automatic with VMware; however, you may wish to customize some things about the system. I would make sure of the following:

* the VM has 35GB of storage
    * The storage is configured to split into multiple files and **not pre-allocate**, we don't want it to pre-allocate as will keep the size of the files you need to upload/send to me smaller.
* Your user account username/password are configured
* One of the packages I'll have you install later is OpenSSH which if you are paying attention during installation can be installed at this point.

### Create Grader Account

Create a grader account so I can login and test that everything is configured correctly. The account should have the following details:

* username: grader
* password: grader

## Packages/Updates

### Updates

Make sure the first thing you do when you get on the machine is install any and all updates to make sure your computer is as current and up to date as possible at the start.

### Packages

Once you've updated the machine, next install the following packages:

* openssh-server
* vim
* nano
* emacs
* curl
* wget
* git
* fortune
* man
* nginx

## Configure NGINX

The final goal is to configure NGINX to do the following:

* listen on port 5555 instead of port 80
* display your name instead of the default NGINX is installed message

## Submitting Assignment

Due to the size of the VM we don't have an easy way for you to submit it so we we will be leveraging Google Drive for submission. If you're on macOS you'll see your VMs likely in a Virtual Machines folder in your home directory, I think on Windows/Linux it's either in your documents or similar location. On macOS you should see the VM as a file potentially with the *.vmwarevm* file extension. In reality the VM is a folder of a bunch of files, which is likely what it looks like on Windows/Linux. You want to put the whole folder into a folder somewhere on your google drive in a folder called **A1_Submission** that you will share with my campus gmail, &#098;&#099;&#100;&#105;&#120;&#111;&#110;&#064;&#109;&#097;&#105;&#108;&#046;&#099;&#115;&#117;&#099;&#104;&#105;&#099;&#111;&#046;&#101;&#100;&#117;.

So make sure you do the following:

* Create folder named **A1_Submission**
* Share **A1_Submission** with my campus @mail.csuchico.edu email
    * You should additionally put a shared link in a file and commit to your CSCI444 repo
* Copy your *.vmwarevm* folder of files into the submission folder.
    * When I go to the **A1_Submission** folder I should see the vmware image parent folder not all the vm files contained inside.
* Share the submission folder as a link that you put in a text file and submit to [Tyson's Turnin system](https://turnin.ecst.csuchico.edu/) before the deadline.

This approach to submitting may be a huge headache/problem but I didn't want to build an auto grading tool again to not teach this class again for another 8 years.

### Alternative Submission Method (Slow Internet)

If you have extremely slow internet and can't upload your submission while on campus for class you can alternatively come show me your submission during office hours; however, make sure you have not opened/touched the VM after the submission deadline or I won't be able to accept your submission.