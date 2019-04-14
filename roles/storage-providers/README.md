<p align="center"><img src="storage_provider.png"></p>

<div align="center">
  <h4>This is a step-by-step guide to setup your <a href="https://github.com/Joystream/storage-node-joystream">storage node</a>, and get started as a Storage Provider on the latest
  <a href="https://testnet.joystream.org/pioneer">Joystream Testnet</a><h4>
</div>



# Table of contents

- [Overview](#overview)
- [Instructions](#instructions)
    - [Mac & Linux](#mac)
        - [Setup node](#setup-node)
        - [Generate your keys](#generate-your-keysm)
        - [Re-start your node as a validator](#re-start-your-node-as-a-validatorm)
        - [Configure your validator keys](#configure-your-validatorm)
- [Troubleshooting](#troubleshooting)


**WIP**

# Overview

This page contains all information on how to setup your storage node and becoming a `Storage Provider` on the Joystream Testnets. It will be updated for improvements, and when something changes for new testnets.

# Instructions
Note that the software will only run on Mac and Linux. If you are not comfortable using the command line, proceed with caution.

## Mac & Linux

If you are using mac, you need [homebrew](https://brew.sh/) installed on your system. The following packages are required:
```
# On mac
$ brew install npm yarn node git
# On Linux
$ sudo apt-get install npm yarn node git
```
Note that `node` v11.xx will lead to some issues on Linux.

#### Run a Joystream Node
To be a `Storage Provider`, you also have to run a [full node](https://github.com/Joystream/substrate-node-joystream). Instructions can be found [here](https://github.com/Joystream/helpdesk/roles/validators). Note that you can stop after the first setup.

#### Download and install
First you need to clone the repo:
```
$ git clone https://github.com/Joystream/storage-node-joystream.git
$ cd storage-node-Joystream
$ yarn run build
$ npm install -g
# If this fails, replace js_storage with ./path/to/storage-node-joystream/bin/cli.js
```
#### Generate keys and memberships

Click [here](https://testnet.joystream.org) to open the `Pioneer app` in your browser. Generate a set of keys, by clicking `Keys` in the sidebar, and `Generate keys`.

...continue.

---

# Troubleshooting
If you had any issues setting it up, you may find your answer here!
