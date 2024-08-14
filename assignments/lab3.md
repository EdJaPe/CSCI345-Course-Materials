# Lab 3

Continue with the VM from Lab 2 to do the following.

## Goals

* Change SSH port to non-standard port
* Change HTTP port to non-standard port
* Insult users who mistype sudo password
* Adding grader account to sudoer file
* Add new disk to VM
* Use parted to partition disk(s) with ext4
* Add multiple new disks, install ZFS, and setup ZFS volume across disks
* Auto-mount the new ext4 and ZFS volumes using /etc/fstab

## SSH Configuration

### Change default port

Why might you want to change the default SSH port? As everyone knows, SSH is on port 22, so if you use a non-standard port, malicious parties may not be able to attack it as easily. You can change your default port by modifying the SSH configuration file [1].

```bash
$ sudo vim /etc/ssh/sshd_config
```

Change the default port from being 22, to being **2222**, this will be tested so make sure you don't set it to some other port other than **2222**. You'll want to setup the GCP firewall to allow this **before** making this change.

### Restrict SSH access

We don’t want any old account to log into our system. So, lets tell the ssh daemon to only allow the users we know about to login, and not, say, any system accounts we haven’t thought to secure. To do this, you’ll need to edit the /etc/ssh/sshd_config file, add a line at the bottom that lists the allowed user accounts (i.e., grader, root, and your user account) like so:

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

## Configure NGINX

Configure NGINX to do the following:

* listen on port 5555 instead of port 80

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

## New EXT4 disk

### Add new VM disk

Add a new 1-5GB disk to your VM. To do this your VM needs to be shut down and then in the settings you can add a new device. The disk doesn't need to be large so I would stick with 1GB, and make sure it doesn't pre-allocate the disk. This is slower performance wise but not pre-allocating will make your VM size smaller for submitting.

### Partition New Disk

A modern disk be it virtual or physical, comes with the low level formatting that break the disk up and organize it by addresses in logical blocks already been done, this is something very old systems required us to do first. You will need to partition the blank disk and format it so that it can use a specific filesystem, in this case *EXT4*.

To do this you will need to use parted [1]. I would recommend reading the parted user manual linked in the references to figure out how to format the entire disk to be a *EXT4* filesystem partition.

### Test that the disk mounts

Make a folder in */mnt* named *ext4*. Mount your new *EXT4* disk to the */mnt/ext4/* folder to test that it works. Finally unmount it since we will be configuring the system to auto mount it later.

## New ZFS volume[2]

### Add new disks

Add three new 1-5GB disks to your VM and don't pre-allocate the disk. Make sure your disks are all the same size. You could do this at the same time as your previous creation, but regardless your VM will need to be powered off to add the disks.

### Install ZFS utilities

To configure and use ZFS under Ubuntu you need to install the utilities package *zfsutils-linux* which will install all the OpenZFS tools we need to create ZFS volumes not as a root volume.

### Create Pool

You created 3 equally sized disks so that we could create a raidz1 ZFS storage pool. This is similar to a RAID5 storage volume in the way it stripes and is capable of losing one disk and not losing the data. In practice raidz1 is not recommended for enterprise use and raidz2 or raidz3 is preferrable. This is due to the fact they can sustain 2 (raidz2) or 3 (raidz3) disk failures in the volume. This makes them more resilient and unlikely to lose data during a rebuild should an additional disk fail.

First find the 3 disks you created for the ZFS pool earlier via the fdisk utility:

```bash
$ sudo fdisk -l
```

Take note of the disk paths for your 3 disks and then use the zpool command to create your new pool:

```bash
$ sudo zpool create <pool_name> raidz1 <drive1> <drive2> <drive3>
```

Make sure to replace all of the above values with **<>** with the values for your machine. If you want more details on the zpool command read the man page.

After creating the pool make sure that it created correctly by checking the status, which in my case looked as follows:

```bash
$ sudo zpool status
  pool: mypool
 state: ONLINE
  scan: none requested
config:

	NAME        STATE     READ WRITE CKSUM
	mypool      ONLINE       0     0     0
	  raidz1-0  ONLINE       0     0     0
	    sdb     ONLINE       0     0     0
	    sdc     ONLINE       0     0     0
	    sdd     ONLINE       0     0     0

errors: No known data errors
```

We can list the zpools to see that it shows up there as well, which in my case shows:

```bash
$ zpool list
NAME     SIZE  ALLOC   FREE  CKPOINT  EXPANDSZ   FRAG    CAP  DEDUP    HEALTH  ALTROOT
mypool  2.75G   205K  2.75G        -         -     0%     0%  1.00x    ONLINE  -
```

We can also see that our ZFS pool is auto mounted at */pool_name* with the *df* command, which in my case shows:

```bash
$ df -h
Filesystem                         Size  Used Avail Use% Mounted on
...
mypool                             1.8G  128K  1.8G   1% /mypool
```

Similar to the *EXT4* mount, I want you to create a folder or mountpoint for your ZFS pool at */mnt/zfs* to do this we will use the zfs command to set the mountpoint of our pool:

```bash
$ sudo zfs set mountpoint=/mnt/zfs <pool_name>
```

Now verify with the *df* command that your pool's mountpoint is at the correct location.

## SSH Filesystem

One of the more interesting applications of Filesystems in User SpaceE (FUSE) is to make a filesystem that mounts a remote SSH filesystem to a local mount point.

### Install sshfs

The first step is installing the *sshfs* package.

### SSH keys

Create an SSH keypair on your VM and add that ssh public key to your ecc-linux account. I would recommend removing this key from your authorized_keys file on ecc-linux as soon as this assignment has been graded.



## Automatically mount volumes

So the ZFS pool will already automatically remount at the mount path you gave it before; however, your *EXT4* mount will need to be remounted everytime your machine restarts. This isn't ideal so we want to automatically have them mount. To do this we'll need to edit the */etc/fstab* file to add new file systems to the table of mounts. You should be able to add the following lines to the end of the file:

```bash
/dev/sdb1   /mnt/ext4  ext4   defaults    0   0
```

In my case my *EXT4* disk was */dev/sdb1* this may not be the case for you. 

###

Restart your VM and verify your 2 filesystems are still mounted. The easiest way to check this is with the mount command:

```bash
$ mount
```

This will list all the mounts on your system, so check that your ZFS and EXT4 mounts are there.

## Submitting Assignment

Submit the IP of your VM instance to [https://inginious.csuchico.edu](https://inginious.csuchico.edu/) for the Lab 3 submission.

### Alternative Submission Method 

If something goes wrong with the inginious submission method, can come see me during office hours or during a lab session to have your assignment graded.

## References

1. https://www.gnu.org/software/parted/manual/parted.html
2. https://ubuntu.com/tutorials/setup-zfs-storage-pool
3. https://www.digitalocean.com/community/tutorials/how-to-use-sshfs-to-mount-remote-file-systems-over-ssh
4. https://linux.die.net/man/5/sshd_config
