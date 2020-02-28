---
title: Change Linux Hostname
layout: post
categories:
  - tutorial
  - linux
comments: true
---

This quick guide outlines the steps to check and update the hostname of a computer or server running Linux from the command line. 
<!--more-->

For this walk through, I'll be sharing examples from a server running Ubuntu 18.04.4 LTS. The commands are compatible across many distros. I use VIM from the command line, but substituting nano for VIM commands will work just as well.

## View current hostname

There are a couple of ways to view the host name. By default, some operating systems like Ubuntu and others use the hostname as part of the shell prompt.

### Bash Prompt

![shell prompt]({{ site.img_dir }}set-hostname-01.png)

In this example, the green text of the bash prompt `gavin@mars` is in the format `[user]@[hostname]`. This isn't always the case since the prompt can be customized through the user profile.

### `hostname` command

Simply enter the command: 

```bash
$ hostname
```

Or read the hostname file:

```bash
$ cat /etc/hostname
```

![hostname prompt]({{ site.img_dir }}set-hostname-02.png)

## Change the hostname

Set the hostname. It can be anything. I'm using `mars` in this example. Replace with your chosen name. The `sudo` command may be required based on user.

```bash
$ echo "mars" > /etc/hostname  
$ hostname -F /etc/hostname
```
Verify the change

```bash
$ hostname
mars
``` 

Reboot

```bash
$ sudo shutdown -r 0
```

Your hostname is now updated. 


## Set the FQDN

For bonus points, you can also set a fully-qualified domain name (FQDN). This is useful for accessing your VPS over the internet without having to remember the IP address.


You will need a registered domain name and access to edit the DNS records.

 Edit the `/etc/hosts` file. Again, `sudo` maybe required.

 ```bash
 $ vi /etc/hosts
 ```

 Add the following line after everything that's already in this file by default. Replace `mars`, `<server.ip>` and `<domain-name>` with your values.

 ```bash
<server.ip>    mars.<domain-name>.com    mars
 ```

Verify the change

```bash
$ hostname -f
```

In order for this to be accessible over the public internet, you'll need to add an A record to your domain's DNS. This A (or Alias) record will route all requests to the chosen FQDN to your server's IP address. This will vary by registrar, but you'll create a new host record for `<domain-name>.com` with the following:

 ```
 Type         Host      Value
 A Record     mars      <server.ip>
 ```

Once this has had a chance to propagate through the DNS servers, you can access your VPS using the FQDN. Like this:

 ```bash
$ ssh mars.<domain-name>.com
 ```
 
### Easy win

Your server now has +10 cool points and as a bonus, you can actually remember the address to access over SSH. Let me know if I missed anything or if you have questions!
