---
title:      "useradd"
date:       2023-09-15T13:20:57-04:00
tags:       ["linux", "lit", "tech"]
identifier: "20230915T132057"
---

to add a user you use `useradd` command

to add a user
`sudo useradd <username>`
- by default, the users home directory will be created in '/home/<username>'

you then need to setup the password
`sudo passwd <username>`

to verify is user was created
`sudo grep <username> /etc/passwd`
- this is the system user database

you can also use the `id` command
`id <username>`

also you can check if the home directory was added


give user sudo permision
========================

you need to add the user to the **sudoers** file

you can install **visudo** to edit the sudoers file
`sudo visudo`
- this will make user you don't add any mistakes

add the following name to the file
`<username> ALL=(ALL:ALL) ALL`

if you don't want to have to provide a password
`<username> ALL=(ALL:ALL) NOPASSWD: ALL`



add user to sudo group
======================

add user to sudo group
`usermod -aG sudo ruahman`

check if groups user was added to 
`groups ruahman`
