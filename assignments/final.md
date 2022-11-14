# Final Project

## Goals

* Setup 3 node cluster
    * Kubernetes running a single cluster across all 3 nodes.
    * Shared NFS /home folders
    * Grader account w/ SSH key on all nodes
* Automate w/ Ansible or Puppet

## Instructions

For help refer to [K3s Docs to setup a cluster](https://docs.k3s.io/installation)

You only need to setup a basic cluster.

## Answer Questions

Answer the following questions in Final_README.md file in your submission folder.

1. What is Kubernetes?
2. Why might you want a shared home directory?
3. Why would you want to automate the setup?
4. Did you setup your K3s cluster on GCP or locally? Why did you choose this route?

## Submitting Project

Commit to the *finalproject* branch of your CSCI345 organization repo with the following:

* Ansible Playbooks or Puppet Manifests
* Ansible Inventory (if using ansible)
* Any other needed files
* Final_README.md

Verify that you can see your files on GitHub for the repo under the *finalproject* branch. Also make sure your branch is named correctly or it will not be pulled for grading.
