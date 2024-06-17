# Lab 2

## Goals

* Create your account
* Create a separate grader account
   * Should **not** be done adding a new key to your GCP instance
* Add SSH key to grader account
* Install updates
* Install packages
    * vim, curl, git, fortune, man, Nginx
* Configure Nginx

## Create your virtual machine/accounts

Create an instance on GCP with Ubuntu 24.04.1

### SSH Keys

For GCP cloud SSH access not through the web interface you'll need to generate a public/private key pair.

#### Generate keys

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

#### Generate Additional Keys (optional)

You may want to consider other more secure keys to generate as well such as:

* ed25519 - elliptic curve cryptography based public/private key pair
* id_ecdsa_sk or ed25519-sk - elliptic curve based keys with FIDO enhancement
    * ECDSA type key is acceptable here since requires FIDO as well, but on its own isn't recommended since hinges on your machine's "randomness"
    * Requires a security key like a Yubikey as will require user interaction/presence to secondarily sign the key.

Not sure if any of these are feasible on Windows.

### Create Grader Account

Create a grader account so I can login and test that everything is configured correctly. The account should have the following details:

* username: grader
* password: grader

### Add Grading SSH key to Grader Account

You'll need to manually edit the grader authorized keyfile, so switch to the grader user and edit the following file:

```bash
$ vim ~/.ssh/authorized_keys
```

You won't be able to save unless the hidden .ssh folder exists.

Add the following public key:

```bash
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAAEAQCyy9AM+D+EjIaXoE3XTMyBgFjiwEZBkbRmwHf0oetUXyl6sbOY3qihiN6eQLQCN4/y/AB1cjs+rcIgbhsLsfBlJheqRuv6flaS24ycI9vNA72oKKIw2F/CPgIbr0FkfsjIziSix4e8bC6BAHQxo5PfCNLlGQHQumBFB749QByv25wLRqh7dyY4cuMwszmTzhEzWSSIdqOqDYwOVGuIWWd+f5crpem38fZnP3FR2GGMcnOwjlPnxHj/3JH3Gt9eEY6eLITrlGG6/jxEz3awjulZhJbIs6h9U++b4f772LPRG4hZz3ygBnMo0rqSYJB1hGiFqxLvI6HMrcg9eyOKa8yGxp+s2vToQHRLdN/ORV6lBM+L1C8NkyMsS2zd64kU93CAPV5up45j3S9o4IeHeck1SeU6htTEPzFxlSN1L/R89TivhvTIZRuNclBuJsk9r/FPClqOI7sxjMworXYtSv/UYkHqiGzt646evIBZHJZ4bXgkkO7bmg4kL/+TuKmRgdWfRkO/WsjY4YuHlfC8YMCqhEgb1q4ksv/+PDkiVm4kp6Zu96bB6ZENnMOTFuHx/TYLSXmedQF1Q54mFCX+7+zYiwQuJazdRly/yy5yf9lBS5C7AmRckqcM8zQ7xjWrG7UsU0Pgu2PyrV5T5XDlczjAZgqIrraWtqiSl+2t/8eqSPO395OMK0gakGYPFfvBd/8E+Jqwq/8glU8WHSzAmlpnAy41jHasYj/JjWVrfKaTB1s2TVnIbTHFC91xDHl5L3WNIHzgizVB5nbga8rjJAw+F804ZAIy8hOimYZjIHJcmdHjco+W/NGMHvvnKNdWvyOMCocq6sxW5Tbsje+BfPALDo58pUkyU26xEz9gm9imgTFEtviA2cbTtWSTeSFT6j5QSM3Z7AQvdruvExJusR+jLpwwVT3INxUOFEKhzYiRr1sVliKRB0vHWkksoX42DULYrndvUVDYKLD3FtzzF7K3V/rB2dWXyeTyKJY1OPgeqZXz2Kke9ILR0EOpoNXyXaTeW+U0k/ofjfcRQ8Oio9cREIt9+pmQARcI1uDuointhLDnCeBotSwIC3NifoHmVQWCXBZqhrsWa3V0z+TOgZr/eH6RimsILVVl4gkiaJ1+daw+gfPIlNO5IORsR3C8ECaLSEWuS+GeGYjDXNOK5Jcj5EQoADpWYaIUJcBmyreAajnlhbeImryLiMMd5DrlQxJPJWi02ZSbPYS/Z2k3gZLQndhDXtXX1qsWLPh1RdtvO4x5nveP3X9+gjnHt98aYHR98tFmXLUBPzGLX1MInUL2ARzCQTTS/PXKTeemjYe81rT3mrwVU4MM9o9GyGOe9IuRkL5hL0+kv9MT/MmWhN7D bcdixon@inginious
```

You can your key as well on a second line.

## Packages/Updates

### Updates

Make sure the first thing you do when you get on the machine is install any and all updates to make sure your computer is as current and up to date as possible at the start.

### Packages

Once you've updated the machine, next install the following packages:

* vim
* nano
* emacs
* curl
* wget
* git
* fortune
* man
* nginx

## Configure NGINX

The final goal is to configure NGINX to do the following:

* listen on port 5555 instead of port 80
* display **CINS345** instead of the default NGINX is installed message

## Submitting Assignment

Submit the IP of your VM instance to [https://inginious.csuchico.edu](https://inginious.csuchico.edu/) for the Lab 3 submission.

### Alternative Submission Method 

If something goes wrong with the inginious submission method, can come see me during office hours or during a lab session to have your assignment graded.
