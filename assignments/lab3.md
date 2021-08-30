# Lab 3

## Goals

* Add new disk to VM
* Use parted to partition disk(s) with ext4
* Add multiple new disks, install ZFS, and setup ZFS volume across disks.
* FUSE ssh mount remote filesystem
* Auto-mount the new SSH, ext4, and ZFS volumes using /etc/fstab

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

### Mount /tmp folder via sshfs

Make a new folder for the mount point */mnt/sshfs* that we'll mount the ecc-linux */tmp* folder to. You can now test that you can mount the */tmp folder to your mount point:

```bash
$ sudo sshfs -o allow_other,default_permissions,IdentityFile=/home/your_user/.ssh/id_rsa username@ecc-linux.csuchico.edu:/tmp /mnt/sshfs
```

This assumes you used an RSA public/private key pair with the default name. You should now see what is in the */tmp* folder on *ecc-linux* at */mnt/sshfs* on your system. This could be more useful to you to mount your *ecc-linux* home folder elsewhere on your system and edit files there directly; however, since I don't want to have you share your home directory with me for this assignment I'm asking you to share the temporary storage folder, that gets deleted everytime *ecc-linux* restarts.

Now unmount the *sshfs* mount.

```bash
$ sudo umount /mnt/sshfs
```

## Automatically mount volumes

So the ZFS pool will already automatically remount at the mount path you gave it before; however, your *EXT4* and *sshfs* mounts will need to be remounted everytime your machine restarts. This isn't ideal so we want to automatically have them mount. To do this we'll need to edit the */etc/fstab* file to add new file systems to the table of mounts. You should be able to add the following lines to the end of the file:

```bash
sshfs#username@ecc-linux.csuchico.edu:/tmp /mnt/sshfs fuse IdentityFile=/home/your_user/.ssh/id_rsa,allow_other,default_permissions 0 0
/dev/sdb1   /mnt/ext4  ext4   defaults    0   0
```

In my case my *EXT4* disk was */dev/sdb1* this may not be the case for you. Also your username/user information for the *sshfs* will need to match accordingly.

###

Restart your VM and verify your 3 filesystems are still mounted. The easiest way to check this is with the mount command:

```bash
$ mount
```

This will list all the mounts on your system, so check that your ZFS, EXT4, and SSHFS mounts are all there.

## Submitting Assignment

Due to the size of the VM we don't have an easy way for you to submit it so we we will be leveraging Google Drive for submission. If you're on macOS you'll see your VMs likely in a Virtual Machines folder in your home directory, I think on Windows/Linux it's either in your documents or similar location. On macOS you should see the VM as a file potentially with the *.vmwarevm* file extension. In reality the VM is a folder of a bunch of files, which is likely what it looks like on Windows/Linux. You want to put the whole folder into a folder somewhere on your google drive in a folder called **A2_Submission** that you will share with my campus gmail, &#098;&#099;&#100;&#105;&#120;&#111;&#110;&#064;&#109;&#097;&#105;&#108;&#046;&#099;&#115;&#117;&#099;&#104;&#105;&#099;&#111;&#046;&#101;&#100;&#117;.

So make sure you do the following:

* Create folder named **A3_Submission**
* Share **A3_Submission** with my campus @mail.csuchico.edu email
    * You should additionally put a shared link in a file and commit to your CSCI444 repo
* Copy your *.vmwarevm* folder of files into the submission folder.
    * When I go to the **A3_Submission** folder I should see the vmware image parent folder not all the vm files contained inside.
* Share the submission folder as a link that you put in a text file and submit to [Tyson's Turnin system](https://turnin.ecst.csuchico.edu/) before the deadline.

This approach to submitting may be a huge headache/problem but I didn't want to build an auto grading tool again to not teach this class again for another 8 years.

### Alternative Submission Method (Slow Internet)

If you have extremely slow internet and can't upload your submission while on campus for class you can alternatively come show me your submission during office hours; however, make sure you have not opened/touched the VM after the submission deadline or I won't be able to accept your submission.



## References

1. https://www.gnu.org/software/parted/manual/parted.html
2. https://ubuntu.com/tutorials/setup-zfs-storage-pool
3. https://www.digitalocean.com/community/tutorials/how-to-use-sshfs-to-mount-remote-file-systems-over-ssh