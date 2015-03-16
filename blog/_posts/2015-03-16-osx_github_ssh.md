---
layout: post
title: osx_github_ssh
author: ilsec
---
##osx下github的ssh设置

1. `ssh-keygen` 创建ssh公私钥对。
2. 登陆github，在设置里选择ssh，然后添加`1`中生成的公钥。
3. `ssh-add -l` 看下刚才添加的ssh是否在ssh列表里。
4. `ssh-add` 添加刚才的公钥，前提是`3`中未列出生成的公钥。
