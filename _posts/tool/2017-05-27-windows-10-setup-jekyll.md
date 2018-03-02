---
layout: post
title: Windows 10 setup jekyll
category: 工具
tags: Tools
keywords: Tool, Windows 10, Jekyll
---

## 1. Install a package manager for Windows called [Chocolatey](https://chocolatey.org/install)
```
@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
```
## 2. Install Ruby via Chocolatey: `choco install ruby -y`
```
choco install ruby -y
```
## 3. Reopen a command prompt and install Jekyll: `gem install jekyll bundler`
```
gem install jekyll
```
