---
layout: post
title: Ansible in a virtualenv
description: Living the dream of virtualization
permalink: :title
---

## The problem
Ansible installed via standard OS package manager uses standard OS python. Yes, in practice this is exactly as horrible as you might imagine. I learned that the OS python is something you never want to touch, change, symlink, update, or do anything else with it beside acting as if it wasn't there.

Example: We use ansible to provision digital ocean machines with the official DO ansible module. It requires the pip package `dopy`. This is one of many custom dependencies your project might have and if you use system installed ansible then you have to install dopy system wide. That is, if ansible is configured to use the right python binary. That works, if you have the $PYTHONPATH set to something this binary understands, and finds dopy in it.  

Suddenly vim starts acting up. Some of the plugins also use python and cannot cope well with the newly set PYTHONPATH needed for ansible. You see, [python versioning is hell](https://xkcd.com/1987/). Python is powerful and everyone wants to use it. In its own version, depending on version 2.7, 3.5, you name it. So: everyone gets their own python (that must sound so weird to the *elderly*!)

## The solution
`virtualenv` to the rescue. It's like a small python sandbox where all dependecies for the current environment are stored. Thank god ansible is also a pip package, that makes things even easier. Since virtualenv has been around for a bit (~2007) and is a bit clunky to use you can use [virtualenvwrapper](https://virtualenv.pypa.io/en/stable/) and you're good.
Follow the installation procedure for your OS.

So now, if you have a project that needs ansible go inside that projects folder and install ansible inside virtualenv:
```
mkvirtualenv <projectname>
workon <projectname>
pip install ansible
pip install <random ansible dependency>
```
And boom. Try `ansible --version` and muse at the fact that your python binary now comes from within your virtual sandbox. So cosy!

If you, like me, find it a tad tedious to create the sandbox for every project - not all projects have custom deps - then make a folder `ansible` in your `~/bin/` folder and just do 
```
mkvirtualenv --system-site-packages ansible
workon ansible
pip install ansible
```
You can now whenever you need ansible somewhere type `workon ansible` and you have your system wide version ready. I even deinstalled my OS package manager version (brew in my case).

Keep on rocking in the free world, and doo doo deloo doo ...

Don't forget about archive.org if the links are broken.
