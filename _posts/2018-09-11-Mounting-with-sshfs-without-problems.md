---
layout: post
title: Mounting remote folders via sshfs.
description: Let's mount with sshfs and be happy about it.
permalink: :title
---

I recently had to setup gitlab backup routines. The way it was done was via the gitlab-rails backup provider `Local`. [Gitlabs own backup documentation is very good](https://docs.gitlab.com/ce/raketasks/backup_restore.html#uploading-to-locally-mounted-shares) on creating backup routines to locally mounted remote folders.

The eery part was the mounting via sshfs. If you mount a folder via, say, ```sshfs -o IdentityFile=/path/to/key/id_rsa user@server:~/source ~/dest/folder``` then the standard procedure of Linux is to make this folder only accessible to the user that made the connection. So far so good, this is secure, but I want user `git` to make backups, while I need user `gitlab-backup` to make the remote connection.  
The reason for this is, that I don't want to be able to connect to the remote server via `ssh` using the `git` user. It doesn't feel right and it isn't.

Lets fuse (Filesystem in user space) help us with this. We need to enable the `allow_others` rule in fuse, which is used when sshfs connections are established, so that other users are able to edit the mounted folders.  
Go forth and uncomment `user_allow_other` in `/etc/fuse.conf`. Now you can issue the sshfs command including this option like so: `sshfs -o IdentityFile=/path/to/key/id_rsa -o allow_other user@server:~/source ~/dest/folder` and you will be able to access and modify the folder with other users than the one that mounted it.
