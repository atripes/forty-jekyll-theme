---
layout: post
title: Deploying via post-receive hook
description: easy, fast, reliable (why didn't I know this earlier?).
permalink: :title
---
It's amlost as flexible and verbose as gitlabs CI feature. But without all the overhead.
For me, the key was setting up the bare git repo on the production server:
```
cd ~/project_name
git init --bare
```
To this repo we will push later on. This repo also needs the post-receive hook:
```
touch ./hooks/post-receive
chmod a+x ./hooks/post-receive
```
It needs to be executable. Put this code into the hook file:
```
#!/bin/bash

cd /var/www/project_name

while read oldrev newrev ref
do
	echo "Ref $ref successfully received. Deploying the server..."
	git --work-tree=/var/www/project_name --git-dir=/home/user/project_name checkout develop -f
done
```
Since this script gets its params from stdin instead of cli parameters you need to fetch them with `read`. because there could be more than one branch pushed, you need to do this for each one seperate, hence the `while` loop. in fact, this never comes into play at my deployments, because when i push to production, i always ever push master.

What this does is, it checks out the project into `/var/www/project_name` which in this case is the location that I serve my site from. In the case of my jekyll application there are still things to do for it to run (protip at the bottom of this doc how to solve it).

Ah, btw. How to push.
You are on your dev machine, working on the project, pushing happily into whatever repository holds your code. For this deployment routine to work you need to add an additional remote:
```
git remote add production ssh://user@prodserver.com:~/project_name
```
And you push your current master with:
```
git push production master
```
The push will return whatever stdout it received, so you will know wether it worked or not.
Thanks to `http://www.janosgyerik.com/deploying-new-releases-using-git-and-the-post-receive-hook/` to get me started on this topic.

Protip:
You can add whatever deployment routine you want after that checkout. In my case I am building and restarting a jekyll app, and the command looks like this:
```
git --work-tree=/var/www/project_name --git-dir=/home/user/project_name checkout master -f && jekyll build && jekyll serve --detach
```
