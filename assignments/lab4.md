# Lab 4

## Goals

* Setup a cron job
* Setup a service
* Answer questions about cron/systemctl

## Setup CRON

Edit the *crontab* to append the current date to a file in the grader's home directory called ***date.txt*** every 5 mins.

## Setup a service

Install Node Exporter [1], which will be good practice for getting [Prometheus](https://prometheus.io/) working in a future assignment.

### Setup Node Exporter Service

Create a systemd service file for node exporter at [*/etc/systemd/system/node_exporter.service*](https://www.google.com/search?q=%2Fetc%2Fsystemd%2Fsystem%2Fnode_exporter.service)

## Answer Questions about Cron/systemctl

Answer the following questions in the A4_README.md file on your GitHub branch.

1. What is *cron*?
2. What is *systemctl*?
3. Why might you want to use cron over systemctl?
4. When might you want not to use either?
5. Why are both important if you are administrating a machine?

## Submitting Assignment
So make sure you do the following:

For testing your VM:

* Submit the IP of your VM instance to [https://inginious.csuchico.edu](https://inginious.csuchico.edu) for the Lab 4 submission.
   * The submission will only count for 50% of the assignment credit, so you won't see higher than a 50 on INGInious, the rest of the score comes from the questions


Commit these to your course GitHub repo on the *assignment4* branch.
* A4_README.md
  
Verify that you can see your files on GitHub for the repo under the *assignment4& branch. Also, make sure your branch is named correctly, or it will not be pulled for grading.

## References

1. https://prometheus.io/docs/guides/node-exporter/
