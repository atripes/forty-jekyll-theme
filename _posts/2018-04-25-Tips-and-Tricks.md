---
layout: post
title: Tips and Trips
description: This... you will love.
permalink: :title
---

## Route traffic over VPN
### Windows
```route print #prints all routes```
```route add <remote_addr> <vpn_gw_address>```
```route delete <remote_addr>```
### OSX/Linux
```ifconfig #to find out which device your tunnel is```
```route add <remote_addr> dev tun0```

## Serve a file in a network
You can use socket on pure Linux machines:
`tar -cvz .folder-to-serve | socket -sqw <port>` on the server and
`socket -q <server-ip>:<port> | tar -xvz` on the client and voila enjoy your file.

You have to use other measures when OSX is in play, because Apple. Use npms `http-server`!
`npm install -g http-server` and then 
`cd <DIR TO SERVE> && http-server` That's it. now you can `wget <server-ip>/file` and feel like a free man.

## Use pipenv instead of pip
(https://docs.pipenv.org/)


## Syncronize a directory via rsync
`rsync --dry-run -P -a -z -e ssh --delete --exclude=node_modules --exclude=.git ./local_dir user@remote.server.com:~/dir`

```
--dry-run #remove when certain of command 
-P #prints progress information
-a #works like -r (recursive) with added benefits
-z #uses compression algo for transfer
-e #login shell ssh (use rsync over ssh connection)
--delete #also sync deletion of files
--exclude #exludes files or dirs
```

There is also a way to watch a dir for changes using another bin called `inotifywait`. More [here](https://github.com/drunomics/syncd/blob/master/watch.sh).

## Unattended linux install using preseed file
Fuck, this will be a standalone post at some point. Nah, wait, it's so easy:
`git clone https://github.com/core-process/linux-unattended-installation.git` and then follow the readme. Only works on Linux hosts.

## Compress mp4 via ffmpeg
This compresses the input by a factor of ~12:
`ffmpeg -i ds_orig.mp4 -c:v libx264 -c:a copy -crf 28 -preset veryfast x264_crf28.mp4`

## Mouse adjustments on OSX
Tested up to Catalina
### Disable mouse acceleration
`defaults read .GlobalPreferences com.apple.mouse.scaling` to read the value.
`defaults write .GlobalPreferences com.apple.mouse.scaling -1` to disable it. Login again for changes being applied.
Does not seem to be necessary, but I'll keep the nice tip:
Create a task with Automator (shellscript) and paste the write portion to it. Add it as a login item in `system-preferences -> user and groups`

### Adjust scroll speed
`defaults write .GlobalPreferences com.apple.scrollwheel.scaling 0.7` to set it to 0.7
`defaults write .GlobalPreferences com.apple.scrollwheel.scaling -1` to disable acceleration. Yes, the setting feels like it does two things.

## Certbot (letsencrypt) for multiple domains
`certbot --nginx -d domain1 -d domain2 -d domain3`.

## Mail via Telnet
    :~ telnet

Start the session.

    :~ SET LOCALECHO

This optional command lets you view the characters as you type them, and it might be required for some SMTP servers.

    :~ set logfile <filename>

This optional command enables logging and specifies the log file for the Telnet session. If you only specify a file name, the log file is located in the current folder. If you specify a path and file name, the path needs to be on the local computer, and you might need to enter the path and file name in the Windows DOS 8.3 format (short name with no spaces). The path needs to exist, but the log file is created automatically.

    :~ OPEN mail1.fabrikam.com 25
    :~ EHLO contoso.com
    :~ MAIL FROM:<chris@contoso.com>
    :~ RCPT TO:<kate@fabrikam.com> NOTIFY=success,failure

The optional NOTIFY command specifies the particular delivery status notification (DSN) messages (also known as bounce messages, nondelivery reports, or NDRs) that the SMTP is required to provide. In this example, you're requesting a DSN message for successful or failed message delivery.

    :~ DATA

Type 'Subject: Test from Contoso'.  
Press Enter.  
A blank line is needed between the Subject: field and the message body.  
  
Type `This is a test message`. 
Type a period `.`.  
To disconnect from the SMTP server, type `QUIT`.  
To close the Telnet session, type `quit`.  


Don't forget about archive.org if the links are broken.
