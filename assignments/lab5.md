# Lab 5

## Goals

* allow SSH and HTTP traffic but nothing else
* test firewall is working
* Answer questions about firewalls/iptables

## Install NGINX

Install NGINX as it'll default as a running service with a hosted web page on port 80 (HTTP traffic) on your machine. You'll use this to test your firewall.

## Configure Firewall

You should now use iptables and/or UFW to enable SSH/HTTP but disallow all other ports/traffic.

### Testing

You should be able to trace the rules with the following:

```bash
$ iptables -L -v -n
```

Verify that traffic other than HTTP/SSH is not allowed.

## Answer Questions about IPTables/UFW

Answer the following questions in A5_README.md file in your submission folder.

1. What is *iptables*?
2. What is *UFW*?
3. Why might you want to use UFW over iptables?
4. Why might you want to disallow all traffic other than specified?
5. Why are both important if you are administrating a machine?

## Submitting Assignment

Due to the size of the VM we don't have an easy way for you to submit it so we we will be leveraging Google Drive for submission. If you're on macOS you'll see your VMs likely in a Virtual Machines folder in your home directory, I think on Windows/Linux it's either in your documents or similar location. On macOS you should see the VM as a file potentially with the *.vmwarevm* file extension. In reality the VM is a folder of a bunch of files, which is likely what it looks like on Windows/Linux. You want to put the whole folder into a folder somewhere on your google drive in a folder called **A2_Submission** that you will share with my campus gmail, &#098;&#099;&#100;&#105;&#120;&#111;&#110;&#064;&#109;&#097;&#105;&#108;&#046;&#099;&#115;&#117;&#099;&#104;&#105;&#099;&#111;&#046;&#101;&#100;&#117;.

So make sure you do the following:

* Create folder named **A5_Submission**
* Share **A5_Submission** with my campus @mail.csuchico.edu email
    * You should additionally put a shared link in a file and commit to your CSCI444 repo
* Copy your *.vmwarevm* folder of files into the submission folder.
    * When I go to the **A5_Submission** folder I should see the vmware image parent folder not all the vm files contained inside.
* Share the submission folder as a link that you put in a text file and submit to [Tyson's Turnin system](https://turnin.ecst.csuchico.edu/) before the deadline.

This approach to submitting may be a huge headache/problem but I didn't want to build an auto grading tool again to not teach this class again for another 8 years.

### Alternative Submission Method (Slow Internet)

If you have extremely slow internet and can't upload your submission while on campus for class you can alternatively come show me your submission during office hours; however, make sure you have not opened/touched the VM after the submission deadline or I won't be able to accept your submission.
