---
layout: post
title: "how to deploy a standalone server app"
date: 2015-12-27 16:34
comments: true
categories:
---

This post shares my experiences deploying a standalone application on a remote Linux server. By standalone application, I mean that the application is packaged as a single file. It could be a jar file or an executable packaged from Java/Scala/C/Haskell/etc. In my case, it was a jar file. My server is Ubuntu 14.04. My automation tool is [Ansible](http://www.ansible.com/) and a dash of shell scripts. The process is very much influenced by [Capistrano](http://capistranorb.com/), a tool that is very popular for deploying Ruby web applications. There are three aims. One, I want to deploy in one step. Two, I want to version my deployment. Three, it should cater for easy rolling back.

<!-- more -->

## Deploying in one step

I want to run one command to deploy my application on a remote server. It would have to be a script that can run a series of commands on a remote server. There are plenty of tools available. I chose Ansible. I like Ansible as it is easy to understand. It is like C, close enough to the metal, but gives a light enough abstraction to make it productive.

What the script does is, it transfers the file over(assuming you built it locally). A symbolic link to the new file is created. And then the application is started.

Here is the Ansible script.

``` yml
---
# connector deployment

- name: deploys the application
  hosts: all
  remote_user: ubuntu
  vars:
    timestamp: "{{ansible_date_time.iso8601}}"

  tasks:
    - name: copies the jar file over
      copy: src=../build/app.jar dest=/home/ubuntu/app-{{timestamp}}.jar

    - name: unlink the current app
      file: path=/home/ubuntu/app.jar state=absent

    - name: link to the newly deployed app
      file: path=/home/ubuntu/app.jar src=/home/ubuntu/app-{{timestamp}}.jar state=link

    - name: stops running app
      sudo: yes
      service: name=app state=stopped

    - name: starts the app
      sudo: yes
      service: name=app state=started

    - name: send notification message via slack
      local_action:
        module: slack
        token: <your slack token>
        msg: "app completed deployment."
```

## Versioning your deployments

I want a way to version my deployments. Versioning gives you the option to roll back. There are 2 mechanism which I used. The symbolic link and the timestamp. I stole the ideas unashamedly from Capistrano.

[Symbolic links](https://en.wikipedia.org/wiki/Symbolic_link) is used to point to the desired version of the app to run. This allows you to run whichever version of the app. Timestamp tells you when this file is deployed. I find it good enough for differentiating.

The relevant commands are all found in the Ansible script above. The `timestamp` variable captures the time of the deployment. We then append the timestamp to the newly deployed file. The symbolic linking are done using Ansible.

As you deploy more often, you need to clear the old files which are unused. For that, I wrote a shell script.

``` bash
#!/usr/bin/env bash

# leaves 5 copies of the standalone app
total_files=$(find -name '*.jar' -type f -print0 | xargs -0 ls -t | wc -l)
file_num_to_remove=`expr $total_files - 5`
find -name '*.jar' -type f -print0 | xargs -0 ls -t | tail -n $file_num_to_remove | xargs rm
```

It first finds all the jar files in the current directory. Next it sorts them by date modified with the most recent first. We want to keep the last five deployments. So it finds how many files it should remove by subtracting the total by five. Once we know how many files to remove, we can find them using `tail` and remove them by passing the file name into `xargs`. 5 is an arbitrary number that you can change.

After deployment, the application is deployed as an [Upstart](http://upstart.ubuntu.com/) service and monitored via [Librato](https://www.librato.com/).

## Room for improvement

I see a few ways it can be improved.

The versioning makes use of timestamp. It can be richer. If there is semantic tagging, we could also use that in addition to timestamps. We will be able to tell which version this file contains.

The transfer time over the network is the bottleneck of the process. If you have a build server like [Jenkins](https://jenkins-ci.org/), it would be faster to deploy from it.

I do hope this is helpful to whoever who reads this. Do share your thoughts on how this can be improved.
