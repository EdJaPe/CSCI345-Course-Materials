# Lab 4

## Goals

* Setup a cron job
* Setup a service
* Answer questions about cron/systemctl

## Setup CRON

Edit the *crontab* to append the current date to a file in the grader's home directory. 

## Setup a service

Install Node Exporter [1], which will be good practice for getting [Prometheus](https://prometheus.io/) working in a future assignment. 

### Setup Node Exporter Service

Create a systemd service file for node exporter at [*/etc/systemd/system/node_exporter.service*](https://www.google.com/search?q=%2Fetc%2Fsystemd%2Fsystem%2Fnode_exporter.service)

## Answer Questions about Cron/Systemctl

Answer the following questions in A4_README.md file in your submission folder. 

1. What is *cron*?
2. What is *systemctl*?
3. Why might you want to use cron over systemctl?
4. When might you want to not use either? 
5. Why are both important if you are administrating a machine?

## Submitting Assignment

Due to the size of the VM we don't have an easy way for you to submit it so we we will be leveraging Google Drive for submission. If you're on macOS you'll see your VMs likely in a Virtual Machines folder in your home directory, I think on Windows/Linux it's either in your documents or similar location. On macOS you should see the VM as a file potentially with the *.vmwarevm* file extension. In reality the VM is a folder of a bunch of files, which is likely what it looks like on Windows/Linux. You want to put the whole folder into a folder somewhere on your google drive in a folder called **A2_Submission** that you will share with my campus gmail, &#098;&#099;&#100;&#105;&#120;&#111;&#110;&#064;&#109;&#097;&#105;&#108;&#046;&#099;&#115;&#117;&#099;&#104;&#105;&#099;&#111;&#046;&#101;&#100;&#117;. 

So make sure you do the following:

* Create folder named **A4_Submission**
* Share **A4_Submission** with my campus @mail.csuchico.edu email
    * You should additionally put a shared link in a file and commit to your CSCI444 repo
* Copy your *.vmwarevm* folder of files into the submission folder.
    * When I go to the **A4_Submission** folder I should see the vmware image parent folder not all the vm files contained inside. 

This approach to submitting may be a huge headache/problem but I didn't want to build an auto grading tool again to not teach this class again for another 8 years. 

### Alternative Submission Method (Slow Internet)

If you have extremely slow internet and can't upload your submission while on campus for class you can alternatively come show me your submission during office hours; however, make sure you have not opened/touched the VM after the submission deadline or I won't be able to accept your submission.

## References

1. https://prometheus.io/docs/guides/node-exporter/