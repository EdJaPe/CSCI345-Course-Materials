# Lab 2

## Goals

* Add SSH public key to yours and grader account
* Configure SSH to be public/private key only
* Change SSH port to non-standard port
* Allowing SSH only login with SSH keys
* Restrict SSH access
* Insult users who mistype sudo password
* Adding grader account to sudoer file

## SSH Keys

### Generate keys

So first you need to generate a public/private key pair on your system that you will want to SSH into your VM from. For macOS/Linux OS this is reasonably simple in the terminal:

```bash
$ ssh-keygen -t rsa -b 4096
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/bcdixon/.ssh/id_rsa):
Created directory '/Users/bcdixon/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /Users/bcdixon/.ssh/id_rsa
Your public key has been saved in /Users/bcdixon/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:r9lNfugmvOpcQ0M5ZjiECk3eaXoehHJcvGJW+W93PWI bcdixon@S59671
The key's randomart image is:
+---[RSA 4096]----+
|   o...o.        |
|  .o.+=o . .     |
|  ..=o=oo *      |
|   o=+. .= .     |
|   o..o S.o    . |
|     o . oo..E...|
|      .  o+.oo. .|
|       . =o=o .  |
|       .*.o=+.   |
+----[SHA256]-----+
```

Where we use the *ssh-keygen* utility to generate a RSA type public/private key pair, with 4096 bits. You could use more bits for higher security, I usually use 8192 bits, but conventional logic is 4096 is still extremely secure for now. I would recommend just going with the defaults (hit enter) for all the prompts. A passphrase may be problematic when we get to scripting/automating using this key potentially in the future. 

If you are on Windows you can generate keys in Putty, use WSL2 to provide linux features, or other methods. Here's a guide from Oracle for doing it in Putty: [https://docs.oracle.com/en/cloud/paas/event-hub-cloud/admin-guide/generate-ssh-key-pair-using-puttygen.html](https://docs.oracle.com/en/cloud/paas/event-hub-cloud/admin-guide/generate-ssh-key-pair-using-puttygen.html)

### Generate Additional Keys (optional)

You may want to consider other more secure keys to generate as well such as:

* ed25519 - elliptic curve cryptography based public/private key pair
* id_ecdsa_sk or ed25519-sk - elliptic curve based keys with FIDO enhancement
    * ECDSA type key is acceptable here since requires FIDO as well, but on its own isn't recommended since hinges on your machine's "randomness"
    * Requires a security key like a Yubikey as will require user interaction/presence to secondarily sign the key. 

Not sure if any of these are feasible on Windows. 

### Keys added to accounts

So now you need to add your public key to your authorized keys file on your VM. There are many ways to do this. The easiest is from your host if you're on macOS/Linux is to use the *ssh-copy-id* utility to copy your key(s) to the remote machine for the given account you tell it to copy to. 

Alternatively, for the grader account and if you're on Windows you'll need to manually add public keys, one on each new line, to the file in the hidden directory in your home folder *.ssh*. So for your account:

```bash
$ vim ~/.ssh/authorized_keys
```

#### Grader Account Keys

Make sure the following public key is added to the grader account:

```bash
sk-ecdsa-sha2-nistp256@openssh.com AAAAInNrLWVjZHNhLXNoYTItbmlzdHAyNTZAb3BlbnNzaC5jb20AAAAIbmlzdHAyNTYAAABBBOmPlrtfbfYXJOJ1G2djM68CaeijY6aQpx3SG3xbGQZhH6M/rNZOcICh/6HDFhrZzsO4pv2z2d1ZIhIARIDnPg4AAAAEc3NoOg== bryandixon@S59671
```

I would recommend you also add your own public key to the grader account as well. 

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

This approach to submitting may be a huge headache/problem but I didn't want to build an auto grading tool again to not teach this class again for another 8 years. 

### Alternative Submission Method (Slow Internet)

If you have extremely slow internet and can't upload your submission while on campus for class you can alternatively come show me your submission during office hours; however, make sure you have not opened/touched the VM after the submission deadline or I won't be able to accept your submission. 

## References

1. https://linux.die.net/man/5/sshd_config
