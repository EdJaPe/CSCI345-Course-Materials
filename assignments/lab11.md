# Lab 11

## Goals

* Use Ansible to:
    * Create grader account
    * Install updates
    * Install packages
        * OpenSSH, vim, curl, git, fortune, man
    * Add SSH public key to grader account
    * Configure Node Exporter & Prometheus on the node

## Ansible

Use ansible functionality to create a single playbook file to do the required functionality mentioned in the goals [1].

For 10 points Extra Credit: Write it so it prompts the user for their sudo password vs requiring ansible to be run as root.

## Submitting Assignment

Commit to the *assignment11* branch of your CSCI345 organization repo with the following:

* assignment11.yml

Verify that you can see your files on GitHub for the repo under the *assignment11* branch. Also make sure your branch is named correctly or it will not be pulled for grading.


## Resources

1. https://www.ansible.com/