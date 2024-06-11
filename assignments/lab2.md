# Lab 2

## Goals

* Change SSH port to non-standard port
* Change HTTP port to non-standard port
* Insult users who mistype sudo password
* Adding grader account to sudoer file

## SSH Configuration

### Change default port

Why might you want to change the default SSH port? Mostly as everyone know SSH is on port 22, so if you use a non-standard port malicious parties may not know how to attack it as easily. Change your default port by modifying the ssh configuration file [1].

```bash
$ sudo vim /etc/ssh/sshd_config
```

Change the default port from being 22, to being **2222**, this will be tested so make sure you don't set it to some other port other than **2222**.

### Force Public Key

Since you have setup public keys for your account and the grader account. Now you should also modify the ssh configuration to only allow public keys for SSH.

```bash
# To disable tunneled clear text passwords, change to no here!
PasswordAuthentication no
PubkeyAuthentication yes
```

### Restrict SSH access

We don’t want any old account to log into our system. So, lets tell the ssh daemon to only allow the users we know about to login, and not, say, any system accounts we haven’t thought to secure. To do this, you’ll need to edit the /etc/ssh/sshd_config file, add a line at the bottom which lists the allowed user accounts (i.e., grader, root, and your user account) like so:

```bash
AllowUsers grader root <youraccount>
```

### Restart OpenSSH Server

Once you've reconfigured SSH, you should restart the SSH service. Likely using *systemctl*.

```bash
$ sudo systemctl restart sshd.service
```

### Test your setup

Verify that SSH is now working on the non-standard port with only SSH keys. May require a third account that no keys have been configured to test. Small chance we'll need to update the firewall to allow the new port but hopefully will work on Ubuntu as it's not an SE_Linux distro.

## Insult your users who mistype passwords using sudo

One of my favorite features you can enable is the ability for the system to insult users when they mistype their password authenticating for sudo. Edit the sudoers file (*/etc/sudoers*) which is where the sudo configuration is done, we will be using the visudo utility. Options are set on lines that begin with the Defaults keyword.

```bash
$ sudo visudo
...
Defaults        insults,env_reset
```

Once you've added insults to the list of *Defaults* this feature will be enabled.

## Add grader to the sudoers file

So a final task is to add the grader account to the suoders file so it'll have permissions to verify your configurations. To do this we will run *visudo* to edit the /etc/sudoers file. By default on Ubuntu this will likely use *nano* as the editor. If you want to change the default editor you can run the following:

```bash
$ sudo update-alternatives --config editor
```

Now run visudo.

```bash
$ sudo visudo
```

And you want to add the following line after the one for the user root:

```bash
grader    ALL=(ALL:ALL) ALL
```

This will give the user *grader* all sudo access.

## Submitting Assignment

Due to the size of the VM we don't have an easy way for you to submit it so we we will be leveraging Google Drive for submission. If you're on macOS you'll see your VMs likely in a Virtual Machines folder in your home directory, I think on Windows/Linux it's either in your documents or similar location. On macOS you should see the VM as a file potentially with the *.vmwarevm* file extension. In reality the VM is a folder of a bunch of files, which is likely what it looks like on Windows/Linux. You want to put the whole folder into a folder somewhere on your google drive in a folder called **A2_Submission** that you will share with my campus gmail, &#098;&#099;&#100;&#105;&#120;&#111;&#110;&#064;&#109;&#097;&#105;&#108;&#046;&#099;&#115;&#117;&#099;&#104;&#105;&#099;&#111;&#046;&#101;&#100;&#117;.

So make sure you do the following:

* Create folder named **A2_Submission**
* Share **A2_Submission** with my campus @mail.csuchico.edu email
    * You should additionally put a shared link in a file and commit to your CSCI444 repo
* Copy your *.vmwarevm* folder of files into the submission folder.
    * When I go to the **A2_Submission** folder I should see the vmware image parent folder not all the vm files contained inside.
* Share the submission folder as a link that you put in a text file and submit to [Tyson's Turnin system](https://turnin.ecst.csuchico.edu/) before the deadline.

This approach to submitting may be a huge headache/problem but I didn't want to build an auto grading tool again to not teach this class again for another 8 years.

### Alternative Submission Method (Slow Internet)

If you have extremely slow internet and can't upload your submission while on campus for class you can alternatively come show me your submission during office hours; however, make sure you have not opened/touched the VM after the submission deadline or I won't be able to accept your submission.

## References

1. https://linux.die.net/man/5/sshd_config
