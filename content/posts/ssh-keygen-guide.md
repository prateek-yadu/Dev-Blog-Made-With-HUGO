---
author: "Prateek Yadu"
title: "[Security Matters #1]: Introduction to SSH Keys."
date: "2025-09-27"
description: "This blog is a part of a series called Security Matters. Here I will cover all of the topics which are essential from a security perspective. In this blog I will cover all about SSH keys, what are they? why use them? and what are the advantages of using SSH Keys." 
tags: ["SSH", "Security", "Authentication", "Keys"]
categories: ["linux"]
series: ["Security Matters"]
ShowToc: true
TocOpen: true
---

If you are a developer, system admin or devops engineer you would have used the ```ssh``` command at least once, most people log in by entering their password. But do you know with SSH Keys, you can login passwordlessly, and it is safer than using a password?

Let's understand what SSH is and how to login passwordlessly into the server.

## What is SSH ?   

SSH is a network layer protocol which helps us to connect to the remote server. It is the most commonly used program around the world, every developer has used it at least once. To log in into the system via SSH you just need to type ```ssh user@host-name```.

{{< figure src="/ssh-login.png" caption="Password based login example" alt="Password based login example" align="center">}}


## Why Password Based Authentication is Bad

Password based login is the default and most common way used to log in into the server. But the real problem with password based login is the threat of brute force attacks, this can be dangerous and can compromise the server. To encounter this you would need to set up fail2ban, IP filtering solutions.

Additionally, changing the SSH default port from 22 to some other port will greatly help to reduce unknown SSH login attempts. 

Password based login is generally not recommended as it brings security problems with it.

## What are SSH Keys

SSH Key is a 2 paired key used to authenticate a user into the server. The public key remains in the server while the private key resides in the client's PC. When user goes to the server to login, the server matches the public and private key and lets you login to the server without any use of password.

SSH based login is recommended as it bypasses the problems we face in password based login.

### How to setup SSH Keys

First open your terminal and type  `ssh-keygen -t rsa -b 4096` 

- `-t` :- Specifies the key type as RSA.

- `-b`:- Sets the key length to 4096 bits which is the recommended length. 

After hitting enter a prompt will ask you to specify the name of the key, Just hit enter if you don't want to change the name. Then, it will ask you for a phrase. It is recommended to set the phrase to add an extra layer to security. You can also skip this step by just hitting enter. In our case we will set the phrase to `LEARNING_KEY_BASED_LOGIN` , it will ask us to enter the phrase again after that our key will be generated and stored in `~/.ssh`  directory.

> NOTE :- The phrase which you enter will not be shown in the terminal as it is hidden.

{{< figure src="/ssh-keygen.png" caption="ssh-keys usage example" alt="ssh-keys usage example" align="center">}}

Navigate to `~/.ssh` directory here you will see two files named `id_rsa` and `id_rsa.pub` or the name you have given to your keys,  other than that you might have some other files as well just ignore it for now.

- `id_rsa` :- This is your private key that will stay in your computer.

- `id_rsa.pub` :- This is a public key that we will reside on the server.

Now, to send the public key to your server you can either go to the server physically and paste the contents in the `authorized_keys` file inside `~/.ssh` directory or you can use the `ssh-copy-id` command. We will be looking at `ssh-copy-id` command to add public key into the server.

> NOTE :- for `ssh-copy-id` to work you need to have an active SSH server running on the server.

To add public key into the server type `ssh-copy-id -i ~/.ssh/id_rsa.pub ubuntu@server-01` it will ask for the password of the server, after entering password it will copy the public key to the server in `~/.ssh/authorized_keys` file. 


{{< figure src="/ssh-copy-id.png" caption="ssh-copy-id usage example" alt="ssh-copy-id usage example" align="center">}}

Once the `ssh-copy-id` command completed successfully we can login to the server by typing `ssh [user_name]@[ip_address]` if you have set a phrase it will ask for that phrase, once you enter the phrase you will be logged in to the server.

> NOTE :- Make sure the permissions in `~/.ssh` directory are correctly set. The `~/.ssh` directory should have `read`, `write` and `execute` permissions and contents inside of `~/.ssh` should have `read` and `write` permissions. If you are not sure about which permissions are set, just execute the following command:
>
>``` bash
>sudo chmod 700 ~/.ssh
>sudo chmod 600 ~/.ssh/*
>```

{{< figure src="/passwordless-login.png" caption="passwordless login demonstration" alt="passwordless login demonstration" align="center">}}

SSH keys are the most powerful and safest way to log in to the server. It not only helps to prevent any unauthorized access but also helps in productivity as now you don’t have to type password again and again.

Setting up SSH Keys takes only a few minutes and saves a lot of productive time. I highly recommend using SSH keys as it is fast, easy and industry standard.

This blog is a part of a series called **[Security Matters](/series/security-matters)**. Here, I will cover all the topics around security and best practices for developers and sysadmins. Stay tuned for the next one 🚀.
