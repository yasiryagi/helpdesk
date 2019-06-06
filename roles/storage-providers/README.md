<p align="center"><img src="storage_provider.png"></p>
<!--
<div align="center">
  <h4>This is a step-by-step guide to setup your <a href="https://github.com/Joystream/storage-node-joystream">storage node</a>, and get started as a Storage Provider on the latest
  <a href="https://testnet.joystream.org/pioneer">Joystream Testnet</a><h4>
</div>
-->


# Table of contents
<!-- TOC START min:1 max:3 link:true asterisk:false update:true -->
- [Table of contents](#table-of-contents)
- [Overview](#overview)
- [Instructions](#instructions)
- [Troubleshooting](#troubleshooting)
  - [Install newest version of yarn and node without `sudo` on linux](#install-newest-version-of-yarn-and-node-without-sudo-on-linux)
<!-- TOC END -->


# Overview

This page will contain all information on how to setup your storage node and becoming a `Storage Provider` on the Joystream Testnets. As we have decided to re-write the node from scratch instead of fixing the one currently active, there is currently no reason to run the node.

# Instructions
If you want to try just for fun of it, go [here](old-instructions.md). Note that we will not provide support, and that the paid spots are already filled until [Acropolis](https://github.com/Joystream/joystream/tree/master/testnets/acropolis).

Note that if you are on linux, and don't want to run the software as root, setting up `npm` with `node` and `yarn` often gets users in trouble. Go [here](#install-npm-without-sudo) for instructions.

# Troubleshooting
If you had any issues setting it up, you may find your answer here!

## Install newest version of yarn and node without `sudo` on linux

Go [here](https://nodejs.org/en/download/) and find the newest binary for your distro. This guide will assume 64-bit linux, and `node-v10.16.0`.

If you want to install with `root`, so your user can use `npm` without `sudo` privileges, go [here](#install-as-root).
If you want to install with another user (must have `sudo` privileges), go [here](#install-as-user-with-sudo-privileges).

As an alternative, [nvm](https://github.com/nvm-sh/nvm) is worth considering.

#### Install as Root
This section assumes you are installing as `root`, for user `joystream` (feel free to use a different username). It doesn't matter if `joystream` has `sudo` privileges or not.

```
$ wget https://nodejs.org/dist/v10.16.0/node-v10.16.0-linux-x64.tar.xz
$ mkdir -p /usr/local/lib/nodejs
$ tar -xJvf node-v10.16.0-linux-x64.tar.xz -C /usr/local/lib/nodejs
$ nano ~/.profile
---
Append the following lines:
---
# Nodejs
VERSION=v10.16.0
DISTRO=linux-x64
export PATH=/usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin:$PATH
```
Save and exit, then:
```
$ source ~/.profile
# Verify version:
$ node -v
$ npm -v
$ npx -v
# If everything looks ok:
$ chown -R joystream /usr/local/lib/nodejs
```
Log in to joystream:
```
$ su joystream
$ cd
# Repeat the .profile config:
$ nano ~/.profile
---
Append the following lines:
---
# Nodejs
VERSION=v10.16.0
DISTRO=linux-x64
export PATH=/usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin:$PATH
```
Save and exit, then:
```
$ source ~/.profile
# Verify that it works:
$ node -v
$ npm -v
$ npx -v
# Install yarn
$ npm install yarn -g
# Verify that it works:
$ yarn -v
```

You have now successfully installed the newest versions of `npm`, `node` and `yarn`.

#### Install as user with `sudo` privileges
This section assumes the steps are performed as user `joystream` with `sudo` privileges.

As `joystream`
```
$ wget https://nodejs.org/dist/v10.16.0/node-v10.16.0-linux-x64.tar.xz
$ sudo mkdir -p /usr/local/lib/nodejs
$ sudo tar -xJvf node-v10.16.0-linux-x64.tar.xz -C /usr/local/lib/nodejs
$ nano ~/.profile
---
Append the following lines:
---
# Nodejs
VERSION=v10.16.0
DISTRO=linux-x64
export PATH=/usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin:$PATH
```
Save and exit, then:
```
$ source ~/.profile
$ sudo chown -R joystream /usr/local/lib/nodejs
# Verify that it works:
$ node -v
$ npm -v
$ npx -v
# Install yarn
$ npm install yarn -g
# Verify that it works:
$ yarn -v
```
If you want `root` to be able to use `npm` as well:

```
$ sudo su
$ nano ~/.profile
---
Append the following lines:
---
# Nodejs
VERSION=v10.16.0
DISTRO=linux-x64
export PATH=/usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin:$PATH
```
Save and exit, then:

`$ source ~/.profile`

You have now successfully installed the newest versions of `npm`, `node` and `yarn`.
