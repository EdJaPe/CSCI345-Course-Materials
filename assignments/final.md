# Final Project

## Goals

* Setup 3 node cluster
    * Kubernetes running a single cluster across all 3 nodes.
    * Shared NFS /home folders
    * Grader account w/ SSH key on all nodes
* Automate w/ Ansible or Puppet

## Instructions

For help refer to [Kubernetes Docs to setup a cluster with *kubeadm*](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/)

## Answer Questions

Answer the following questions in Final_README.md file in your submission folder.

1. What is Kubernetes?
2. Why might you want a shared home directory?
3. Why would you want to automate the setup?
4. What are the public IPs for GCP instances for K8s cluster?

## Submitting Project

Commit to the *finalproject* branch of your CSCI444 organization repo with the following:

* Ansible Playbooks or Puppet Manifests
* Ansible Inventory (if using ansible)
* Any other needed files
* Final_README.md

Verify that you can see your files on GitHub for the repo under the *finalproject* branch. Also make sure your branch is named correctly or it will not be pulled for grading.