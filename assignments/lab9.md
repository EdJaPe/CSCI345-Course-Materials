# Lab 9

## Goals

* Setup *node-exporter* on 2 GCP micro instances 
* Setup *prometheus* on one of the 2 GCP micro instances
* Setup *grafana* on the same micro instance you setup *prometheus*
* On both GCP instances
    * Create grader account
    * Install updates
    * Install packages
        * vim, curl, git, fortune, man
    * Add my SSH public key to grader account
    * Insult users who mistype sudo password
    * Adding grader account to sudoer file

## Details

You should create two micro-instances on the Google Cloud Platform (GCP). Each of these should have *node-exporter* configured as a service. You should also do many of the lab 1 & 2 configurations as mentioned in the goals to both instances. 

Choose one instance to be your *Prometheus*/*Grafana* instance and setup both as services on this instance, to pull data from both GCP instances. You may need to configure the GCP firewall to allow internal access so that this node can poll the *node-exporter* on the other instance. 

## Answer Questions about GCP

Answer the following questions in A9_README.md file in your submission folder. 

1. What is *GCP*?
2. What makes it "Cloud" computing?
3. When might you use Cloud vs on-premises computing?
4. Are there any risks/concerns about Cloud? 
5. What is the IP address to access your prometheus instance? 
    * Along with any other info I need to access it from the web
    * Reminder I should be able to access it via SSH as well with the grader account. 

## Submitting Assignment

Commit to the *assignment9* branch of your CSCI444 organization repo with the following:

* A9_README.md

Verify that you can see your files on GitHub for the repo under the *assignment9* branch. Also make sure your branch is named correctly or it will not be pulled for grading.  


## Resources

1. https://prometheus.io/
2. https://linuxhint.com/install_prometheus_ubuntu/
3. https://prometheus.io/docs/visualization/grafana/
