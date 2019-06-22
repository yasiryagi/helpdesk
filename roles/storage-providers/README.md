<p align="center"><img src="storage_provider.png"></p>

<div align="center">
  <h4>This is a step-by-step guide to setup your <a href="https://github.com/Joystream/storage-node-joystream">storage node</a>, and get started as a Storage Provider on the latest
  <a href="https://testnet.joystream.org/pioneer">Joystream Testnet</a><h4>
</div>



# Table of contents
<!-- TOC START min:1 max:3 link:true asterisk:false update:true -->
- [Table of contents](#table-of-contents)
- [Overview](#overview)
- [Instructions](#instructions)
  - [Initial setup](#initial-setup)
  - [Install ipfs](#install-ipfs)
    - [Run ipfs as a service](#run-ipfs-as-a-service)
  - [Setup Hosting](#setup-hosting)
    - [Run caddy as a service](#run-caddy-as-a-service)
  - [Install and Setup the Storage Node](#install-and-setup-the-storage-node)
    - [Update Storage Node](#update-storage-node)
    - [Generate keys and memberships](#generate-keys-and-memberships)
    - [Run storage node as a service](#run-storage-node-as-a-service)
    - [Verify everything is working](#verify-everything-is-working)
- [Troubleshooting](#troubleshooting)
  - [Install yarn and node on linux](#install-yarn-and-node-on-linux)
<!-- TOC END -->


# Overview

This page will contain all information on how to setup your storage node and becoming a `Storage Provider` on the Joystream Testnets. As we have decided to re-write the node from scratch instead of fixing the one currently active, there is currently no reason to run the node before the release of [Acropolis](https://github.com/Joystream/joystream/tree/master/testnets/acropolis#live-milestones).

# Instructions

The instructions below will assume you are running as `root`. This makes the instructions somewhat easier, but less safe and robust.

Note that this has only been tested on fresh images of `Ubuntu 16.04 LTS`, `Ubuntu 18.04 LTS` and `Debian 8`.

The system has shown to be quite resource intensive, so you should choose a VPS with specs equivalent to [Linode 8GB](https://www.linode.com/pricing?msclkid=eaa12e00529310e4665c730d6b01b014&utm_source=bing&utm_medium=cpc&utm_campaign=Linode%20-%20Brand%20-%20Search%20-%20LowGeo&utm_term=linode&utm_content=Linode) or better. (Not an affiliate).

Please note that unless there are any open spots (which you can check in [Pioneer](https://testnet.joystream.org/pioneer) under `Roles` -> `Available Roles`), you will not be able to join. Note that we will be quite vigilant in booting non-performing `Storage Providers`, so if you have everything setup in advance, you could be the quickest to take a slot when it opens!

## Initial setup
First of all, you need a fully synced [Joystream full node](https://github.com/Joystream/substrate-node-joystream/releases). For instructions on how to set this up, go [here](../validators). Note that you can disregard all the parts about keys, and just install the software.
We strongly encourage that you run both the [node](../validators#run-as-a-service) and the other software below as a service.

First, you need to setup `node`, `npm` and `yarn`. This is sometime troublesome to do with the `apt` package manager. Go [here](#install-yarn-and-node-without-on-linux) to do this if you are not confident in your abilities to navigate the rough seas.

Now, get the additional dependencies:
```
$ apt-get update && apt-get upgrade -y
$ apt-get install git build-essential libtool automake autoconf python
```

## Install ipfs
The new storage node uses [ipfs](https://ipfs.io/) as backend.
```
$ wget https://dist.ipfs.io/go-ipfs/v0.4.21/go-ipfs_v0.4.21_linux-amd64.tar.gz
$ tar -vxf go-ipfs_v0.4.21_linux-amd64.tar.gz
$ cd go-ipfs
$ ./ipfs init --profile server
$ ./install.sh
# start ipfs daemon:
$ ipfs daemon
```
If you see `Daemon is ready` at the end, you are good!

### Run ipfs as a service

To ensure high uptime, it's best to set the system up as a `service`.

Example file below:

```
$ nano /etc/systemd/system/ipfs.service
# Paste in everything below the stapled line
---
[Unit]
Description=ipfs
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/root
LimitNOFILE=8192
PIDFile=/var/run/ipfs/ipfs.pid
ExecStart=/usr/local/bin/ipfs daemon
Restart=on-failure
RestartSec=3
StartLimitInterval=600

[Install]
WantedBy=multi-user.target
```
Save and exit. Close the `ipfs daemon` if it's still running, then:
```
$ systemctl start ipfs
# If everything works, you should get an output. Verify with:
$ systemctl status ipfs
# If you see something else than "Daemon is ready" at the end, try again in a couple of seconds.
# To have ipfs start automatically at reboot:
$ systemctl enable ipfs
# If you want to stop ipfs, either to edit the file or some other reason:
$ systemctl stop ipfs
```

## Setup Hosting
In order to allow for users to upload and download, you have to setup hosting, with an actual domain as both chrome and firefox requires `https://`. If you have a "spare" domain or subdomain you don't mind using for this purpose, go to your domain registrar and point your domain to the IP you want. If you don't, you must unfortunately go purchase one.

To configure SSL-certificates the easiest is to use [caddy](https://caddyserver.com/), but feel free to take a different approach. Note that if you are using caddy for commercial use, you need to acquire a license. Please check their terms and make sure you comply with what is considered personal use.

```
$ curl https://getcaddy.com | bash -s personal
# Allow caddy access to required ports:
$ setcap 'cap_net_bind_service=+ep' /usr/local/bin/caddy
$ ulimit -n 8192
```

Configure caddy with `nano ~/Caddyfile` and paste in the following:

```
# Storage Node API
https://<your.cool.url> {
    proxy / localhost:3000 {
        transparent
    }
    header / {
        Access-Control-Allow-Origin  *
        Access-Control-Allow-Methods "GET, PUT, HEAD, OPTIONS"
    }
}
```
Now you can check if you configured correctly, with:
```
$ /usr/local/bin/caddy --validate --conf ~/Caddyfile
# Which should return:
Caddyfile is valid

# You can now run caddy with:
$ (screen) /usr/local/bin/caddy --agree --email <your_mail@some.domain> --conf ~/Caddyfile
```

### Run caddy as a service
To ensure how uptime, it's best to set the system up as a `service`.

Example file below:

```
$ nano /etc/systemd/system/caddy.service
# Paste in everything below the stapled line
---
[Unit]
Description=Reverse proxy for storage node
After=network.target

[Service]
User=root
WorkingDirectory=/root
LimitNOFILE=8192
PIDFile=/var/run/caddy/caddy.pid
ExecStart=/usr/local/bin/caddy -agree -email <your_mail@some.domain> -pidfile /var/run/caddy/caddy.pid -conf /root/Caddyfile
Restart=on-failure
StartLimitInterval=600


[Install]
WantedBy=multi-user.target
```
Save and exit. Close `caddy` if it's still running, then:
```
$ systemctl start caddy
# If everything works, you should get an output. Verify with:
$ systemctl status caddy
# Which should produce something like:
---
● caddy.service - Reverse proxy for storage node
   Loaded: loaded (/etc/systemd/system/caddy.service; disabled)
   Active: active (running) since Tue 2019-06-18 17:15:44 UTC; 6s ago
 Main PID: 5613 (caddy)
   CGroup: /system.slice/caddy.service
           └─5613 /usr/local/bin/caddy -agree email <your_mail@some.domain> -pidfile /var/run/caddy/caddy.pid -conf /root/Caddyfile

Jun 18 17:15:44 localhost systemd[1]: Started Reverse proxy for hosted apps.
Jun 18 17:15:44 localhost caddy[5613]: Activating privacy features... done.
Jun 18 17:15:44 localhost caddy[5613]: Serving HTTPS on port 443
Jun 18 17:15:44 localhost caddy[5613]: https://<your.cool.url>
Jun 18 17:15:44 localhost caddy[5613]: https://<your.cool.url>
Jun 18 17:15:44 localhost caddy[5613]: Serving HTTP on port 80
Jun 18 17:15:44 localhost caddy[5613]: https://<your.cool.url>
---
# To have caddy start automatically at reboot:
$ systemctl enable caddy
# If you want to stop caddy, either to edit the file or some other reason:
$ systemctl stop caddy
```

## Install and Setup the Storage Node

**Note**
If you proceed before [this PR](https://github.com/Joystream/storage-node-joystream/pull/82) is merged, you will need to [update later](#update-storage-node)

First, you need to clone the repo.

```
$ git clone https://github.com/Joystream/storage-node-joystream.git
$ cd storage-node-joystream
$ yarn
# Test that it's working with:
$ yarn run colossus --help
```
You can set the PATH to avoid the `yarn run` prefix by:
`nano ~/.bash_profile`
and append:
```
# Colossus
alias colossus="/root/storage-node-joystream/packages/colossus/bin/cli.js"
```
Then:
`. ~/.bash_profile`
Now, you can test `colossus --help`.

### Update Storage Node

If you need to update your storage node, you will first need to stop the software.

```
# If you are running as service (which you should)
$ systemctl stop storage-node
$ cd /path/to/storage-node-joystream
# Assuming you cloned as shown above
$ git pull origin master
$ yarn
```

### Generate keys and memberships

Click [here](https://testnet.joystream.org) to open the `Pioneer app` in your browser. Then follow instructions [here](https://github.com/Joystream/helpdesk#get-started) to generate a set of `Keys`, get tokens, and sign up for a `Membership`. This will `key` will be referred to as the `member` key from now on. Make sure to save the `5YourJoyMemberAddress.json` file. Note that you need to keep the rest of your tokens as stake to become a `Storage Provider`.

---

Assuming you are running the storage node on a VPS via ssh, on your local machine:

```
# Go the directory where you saved your <5YourJoyMemberAddress.json>:
$ scp <5YourJoyMemberAddress.json> <user>@<your.vps.ip.address>:/path/to/storage-node-joystream/
```
Your `5YourJoyMemberAddress.json` should now be where you want it.


#### Setup and configure the storage node

**Make sure you're [Joystream full node](https://github.com/Joystream/substrate-node-joystream) is fully synced before you move to the next step(s)!**

On the machine/VPS you want to run your storage node:

```
# If you are not already in that directory:
$ cd storage-node-joystream
# If you configured your .bash_profile:
$ colossus signup <5YourJoyMemberAddress.json>
# If you didn't configure your .bash_profile:
$ yarn run colossus signup <5YourJoyMemberAddress.json>
# Note that the rest of the guide will assume you did in fact configure .bash_profile and don't need "yarn run"
# Follow the instructions as prompted. For ease of use, it's best to not set a password.
```

This produces a new key `<5YourStorageAddress.json>`, and prompts you to open the "app" (Pioneer). Make sure that you your current/default is the `5YourJoyMemberAddress` key. After you click `Stake`, you will see a notification in the top right corner. If you get an error, this most likely means all the slots are full. Unfortunately, this means you have to import the `<5YourStorageAddress.json>` to recover your tokens.

If it succeeded, proceed as shown below:

```
# To make sure everything is running smoothly, it would be helpful to run with DEBUG:
$ DEBUG=* colossus server --key-file <5YourStorageAddress.json> --public-url https://<your.cool.url>
```

If you do this, you should see something like:

```
discovery::publish { name: 'QmPwws576n3ByE6CQUvUt3dgmokk2XN2cJgPYHWoM6SSUS',
discovery::publish   value: '/ipfs/QmeDAWGRjbWx6fMCxtt95YTSgTgBhhtbk1qsGkteRXaEST' } +391ms
```
You can just do this instead, but it will make it more difficult to debug...
```
$ colossus server --key-file <5YourStorageAddress.json> --public-url https://<your.cool.url>
```
If everything is working smoothly, you will now start syncing the `content directory`
Note that unless you run this is a [service](#run-storage-node-as-a-service), you now have to open a second terminal for the remaining steps.

#### Check that you are syncing
In your second terminal window:
```
$ ipfs bitswap wantlist
---
# Output should be a long list of keys, eg. Note that it might take a few minutes before the actual content from the Joystream content directory shows up.
---
QmeszeBjBErFQrkiQPh8QhTs3hfCEGJuK2jNopatHnps1k
...
QmfCbUsYhKBmrdop3yFrerqVKwBJvY5tbpV1cf9Cx3L1J8
```
If you did this immediately after FIRST starting your storage node, the `wantlist` might be empty. Give it a minute, and it should contain at least the number of items in the content directory. You can also check what content you have stored by:
```
ipfs refs local
---
# Output should be an even longer list of keys, eg.
---
QmeszeBjBErFQrkiQPh8QhTs3hfCEGJuK2jNopatHnps1k
Qmezum3AWdxkm1AtHe35DZGWdfhTQ4PVmmZatGwDL68RES
...
QmfCCjC5w9wxTFoAaJ947ss2oc1jx6R2mM9xjU7Ccrq55M
QmfCbUsYhKBmrdop3yFrerqVKwBJvY5tbpV1cf9Cx3L1J8
```

In your first terminal (where) the storage node is running, you will soon enough see this:
```
...
joystream:runtime:base TX status: Finalized +7ms
joystream:runtime:base TX Finalized. +1ms
joystream:sync sync run complete +0ms
```

In the second terminal:
```
$ ipfs refs local
```
Should return nothing.

### Run storage node as a service

To ensure how uptime, it's best to set the system up as a `service`. Note that this will not work if you set a password for your `<5YourStorageAddress.json> `.

Example file below:

```
$ nano /etc/systemd/system/storage-node.service
# Paste in everything below the stapled line
---
[Unit]
Description=Joystream Storage Node
After=network.target ipfs.service joystream-node.service

[Service]
User=root
WorkingDirectory=/root/storage-node-joystream
LimitNOFILE=8192
Environment=DEBUG=*
ExecStart=/usr/local/lib/nodejs/node-v10.16.0-linux-x64/bin/node \
        packages/colossus/bin/cli.js \
        --key-file <5YourStorageAddress.json> --public-url https://<your.cool.url>
Restart=on-failure
StartLimitInterval=600

[Install]
WantedBy=multi-user.target
```
Save and exit. Close `colossus` if it's still running, then:
```
$ systemctl start storage-node
# If everything works, you should get an output. Verify with:
$ systemctl status storage-node
# Which should produce something like:
---
● storage-node.service - Joystream Storage Node
   Loaded: loaded (/etc/systemd/system/storage-node.service; disabled)
   Active: active (running) since Tue 2019-06-18 17:25:41 UTC; 4min 19s ago
 Main PID: 5654 (colossus)
   CGroup: /system.slice/storage-node.service
           └─5654 colossus

Jun 18 17:29:31 localhost node[5654]: Tue, 18 Jun 2019 17:29:31 GMT joystream:util:ranges = Got chunk with byte range [ 1555968, 1560063 ]
Jun 18 17:29:31 localhost node[5654]: Tue, 18 Jun 2019 17:29:31 GMT joystream:util:ranges Current requested range is [ 33722848, 44195983 ]
Jun 18 17:29:31 localhost node[5654]: Tue, 18 Jun 2019 17:29:31 GMT joystream:util:ranges Ignoring chunk; it is out of range.
Jun 18 17:29:31 localhost node[5654]: Tue, 18 Jun 2019 17:29:31 GMT joystream:util:ranges = Got chunk with byte range [ 1560064, 1564159 ]
Jun 18 17:29:31 localhost node[5654]: Tue, 18 Jun 2019 17:29:31 GMT joystream:util:ranges Current requested range is [ 33722848, 44195983 ]
Jun 18 17:29:31 localhost node[5654]: Tue, 18 Jun 2019 17:29:31 GMT joystream:util:ranges Ignoring chunk; it is out of range.
Jun 18 17:29:31 localhost node[5654]: Tue, 18 Jun 2019 17:29:31 GMT joystream:util:ranges = Got chunk with byte range [ 1564160, 1568255 ]
Jun 18 17:29:31 localhost node[5654]: Tue, 18 Jun 2019 17:29:31 GMT joystream:util:ranges Current requested range is [ 33722848, 44195983 ]
Jun 18 17:29:31 localhost node[5654]: Tue, 18 Jun 2019 17:29:31 GMT joystream:util:ranges Ignoring chunk; it is out of range.
Jun 18 17:29:31 localhost node[5654]: Tue, 18 Jun 2019 17:29:31 GMT joystream:util:ranges = Got chunk with byte range [ 1568256, 1568874 ]
---
# To have colossus start automatically at reboot:
$ systemctl enable colossus
# If you want to stop the storage node, either to edit the storage-node.service file or some other reason:
$ systemctl stop storage-node
```

### Verify everything is working

In your browser, find click on an uploaded media file. Copy the `<content-id>`, ie. what comes after the last `/` (which looks a `key`).

Then paste the following in your browser:

`https://<your.cool.url>/asset/v0/<content-id>`.

If you get a black screen with a media player, that means you are good!

# Troubleshooting
If you had any issues setting it up, you may find your answer here!

## Install yarn and node on linux

Go [here](https://nodejs.org/en/download/) and find the newest (LTS) binary for your distro. This guide will assume 64-bit linux, and `node-v10.16.0`.

If you want to install as `root`, so your user can use `npm` without `sudo` privileges, go [here](#install-as-root).

If you want to install as another user (must have `sudo` privileges), go [here](#install-as-user-with-sudo-privileges).

Alternatives such as [nvm](https://github.com/nvm-sh/nvm) or [nodesource](https://github.com/nodesource/distributions/blob/master/README.md) are also worth considering.

#### Install as Root
This section assumes you are installing as `root`. It also demonstrates how you can provide another user access without having to use `sudo`. It doesn't matter if user `joystream` has `sudo` privileges or not.

```
$ wget https://nodejs.org/dist/v10.16.0/node-v10.16.0-linux-x64.tar.xz
$ mkdir -p /usr/local/lib/nodejs
$ tar -xJvf node-v10.16.0-linux-x64.tar.xz -C /usr/local/lib/nodejs
$ nano ~/.bash_profile
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
$ source ~/.bash_profile
# Verify version:
$ node -v
$ npm -v
$ npx -v
# Install yarn
$ npm install yarn -g
$ npm i node-gyp@5.0.0
# If everything looks ok, and you want to allow user joystream access:
$ chown -R joystream /usr/local/lib/nodejs
```
Log in to joystream:
```
$ su joystream
$ cd
# Repeat the .bash_profile config:
$ nano ~/.bash_profile
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
$ source ~/.bash_profile
# Verify that it works:
$ node -v
$ npm -v
$ npx -v
$ yarn -v
```

You have now successfully installed the newest (LTS) versions of `npm`, `node` and `yarn`.

#### Install as user with `sudo` privileges
This section assumes the steps are performed as user `joystream` with `sudo` privileges.

As `joystream`
```
$ wget https://nodejs.org/dist/v10.16.0/node-v10.16.0-linux-x64.tar.xz
$ sudo mkdir -p /usr/local/lib/nodejs
$ sudo tar -xJvf node-v10.16.0-linux-x64.tar.xz -C /usr/local/lib/nodejs
$ nano ~/.bash_profile
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
$ source ~/.bash_profile
$ sudo chown -R joystream /usr/local/lib/nodejs
# Verify that it works:
$ node -v
$ npm -v
$ npx -v
# Install yarn
$ npm install yarn -g
# Verify that it works:
$ yarn -v
$ npm i node-gyp@5.0.0
```
If you want `root` to be able to use `npm` as well:

```
$ sudo su
$ nano ~/.bash_profile
---
Append the following lines:
---
# Nodejs
VERSION=v10.16.0
DISTRO=linux-x64
export PATH=/usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin:$PATH
```
Save and exit, then:

`$ source ~/.bash_profile`

You have now successfully installed the newest (LTS) versions of `npm`, `node` and `yarn`.
