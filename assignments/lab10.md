# Lab 10

## Goals

* Use Puppet to:
    * Create grader account
    * Install updates
    * Install packages
        * OpenSSH, vim, curl, git, fortune, man
    * Add SSH public key to grader account
    * Configure Node Exporter & Prometheus on the node

## Puppet

Use puppet functionality to create a single manifest file to do the required functionality mentioned in the goals [1].

For 10 points Extra Credit: Write it so it prompts the user for their sudo password vs requiring puppet to be run as root.

## Submitting Assignment

Commit to the *assignment10* branch of your CSCI345 organization repo with the following:

* assignment10.pp

Verify that you can see your files on GitHub for the repo under the *assignment10* branch. Also make sure your branch is named correctly or it will not be pulled for grading.


## Resources

1. https://puppet.com/