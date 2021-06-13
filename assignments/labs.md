# Labs/Assignments

List of lab ideas for semester

1. Getting Ubuntu VM setup
    * Create their account
    * Create grader account with given credentials
    * Install updates
    * Install packages
        * OpenSSH, vim, curl, git, fortune, man
    
2. SSH/Account Details
    * Add SSH public key to grader account
    * Configure SSH to be public/private key only
    * Change SSH port to non-standard port
    * Insult users who mistype sudo password
    * Adding grader account to sudoer file
    * Allowing grader and root account to only login with SSH keys

3. Filesystems
    * Add new disk(s) to VM
    * Use parted to partition disk(s) with ext4
    * Auto-mount the disk somewhere using /etc/fstab
    * Potentially add multiple new disks, install ZFS, and setup ZFS volume across disks. 

4. IPTables?
    * allow SSH and HTTP traffic but nothing else
    * test IPTables working
    * Answer questions about firewalls/iptables

5. Promethus/Monitoring
    * Setup prometheus on 2 VMs 
    * https://prometheus.io/

6. Logs/Troubleshooting
    * 1-2 broken VMs with service/goal of fixing... possibly broken service on one and need for a recovery disk on a second? 

7. AWS
    * Labs 1/2 on AWS

8. GCP
    * Labs 1/2 on GCP

9. Azure?
    * Labs 1/2 on Azure

10. Puppet
    * Labs 1&2 auto configured with puppet

11. Ansible
    * Labs 1&2 auto configured with Ansible

12. Final Project Cluster
    * Follow manual instructions
    * Turn in ansible playbooks or puppet manifests that automate doing it.
