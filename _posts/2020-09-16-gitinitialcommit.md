---
layout: post
title:  "How to initial commit history"
date:   2020-09-16 21:00:13 +0800
categories: default
tags: test
---
I want to remove my commit history because i delete password in my file.

so I find how to initial my git history

모든 파일과 커밋 히스토리를 삭제하는 방법

---
rm -rf .git

git init
git add .
git commit -m "initial commit"

git remote add origin [url]

git push -u --force origin master

---

