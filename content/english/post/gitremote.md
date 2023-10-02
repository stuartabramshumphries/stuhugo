---
title: "How to change remote in git"
date: 2023-10-02T19:57:15+01:00
Description: ""
Tags: []
Categories: []
DisableComments: false
---
generally remote is set by default to the place you originally cloned your repo from, however you may want to change this.

a simple way is on first push use:

git push -u origin remotename


the above can be configured as the default in your gitconfig file:

[push]
default = current

git remote set-url ..is the command we need.

normally remote has the name origin, which is an alias for a remote repository, set as a key locally in place of the remote repos full url.

to do the change its pretty simple:

$ git remote set-url origin https://urlname.com/USERNAME/REPONAME.git


an interesting feature is we can have multiple remotes (or aliases pointing at them) in your repo... can push or pull to multiple remotes then..but be careful..can get confusing!
e.g.


git remote add alt different-machine:/path/to/repo
