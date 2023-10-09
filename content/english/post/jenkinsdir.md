---
title: "Change Jenkins home dir"
date: 2023-06-02T20:06:06+01:00
Description: ""
Tags: []
Categories: []
DisableComments: false
---

Change Jenkins home directory in Ubuntu

We needed recently to change home directory of Jenkins in Ubuntu. The underlying misconfiguration on this old system is that it wasnt using LVM and so couldnt be expanded on the fly, but it is what it is.

To move we create a new volume - after adding more storage if needed - this time making it under LVM so further expansions can be done on the fly if needed.

as ever ensure we have backups before making changes  
shutdown jenkins safely - wait for current jobs to stop and also dont start new jobs .. https://jenkins-url/safeExit
once stopped copy /var/lib/jenkins  to /newjenk/dir
chown -R jenkins:jenkins /newjenk/dir
edit /etc/default/jenkins - change JENKINS_HOME to point to new dir
start jenkins (systemctl start jenkins)
wait a second .. go to url, login,run a test job that has previously succeeded and ensure ok.

and thats it!

