<p align="center"><img src="img/storage-provider_new.svg"></p>

<div align="center">
  <h4>This is a guide to setting up your <a href="https://github.com/Joystream/joystream/tree/master/storage-node">storage node</a>, and getting started as a Storage Provider on the latest
    <a href="https://testnet.joystream.org/">testnet</a>.</h4><br>
</div>



Table of Contents
==
<!-- TOC START min:1 max:3 link:true asterisk:false update:true -->
- [Overview](#overview)
- [Instructions](#instructions)
  - [Initial setup](#initial-setup)
  - [Setup Hosting](#setup-hosting)
    - [Caddy](#caddy)
    - [Run caddy as a service](#run-caddy-as-a-service)
  - [Setup Query Node](#setup-query-node)
  - [Install and Setup the Storage Node](#install-and-setup-the-storage-node)
    - [Applying for a Storage Provider opening](#applying-for-a-storage-provider-opening)
    - [Setup and configure the storage node](#setup-and-configure-the-storage-node)
    - [Accept Invitation](#accept-invitation)
    - [Set Metadata](#set-metadata)
  - [Deploy the Storage Node](#deploy-the-storage-node)
    - [Verify everything is working](#verify-everything-is-working)
- [Troubleshooting](#troubleshooting)
<!-- TOC END -->



# Overview

This page contains all information required to set up your storage node and become a `Storage Provider` on the current Joystream testnet.

The guide for the `Storage Provider Lead` can be found [here](/roles/storage-lead).

# Instructions

The instructions below will assume you are running as `root`. This makes the instructions somewhat easier, but less safe and robust.

Note that this has been tested on a fresh images of `Ubuntu 20.04 LTS`.

The system has shown to be less resource intensive, so you may get away with a VPS with specs equivalent to [Linode 4GB](https://www.linode.com/pricing/) or better (not an affiliate link).

Please note that unless there are any openings for new storage providers (which you can check in [Pioneer](https://testnet.joystream.org/) under `Working Groups` -> `Opportunities`), you will not be able to join. Applying to the opening is easiest in Pioneer, but once hired, you no longer need it. Actions you may want to perform after getting hired are easiest to carry out with the [CLI](/tools/cli/README.md#working-groups). With this, you can configure things like:
- changing your reward destination address
- changing your role key
- increasing your stake
- leaving the role

## Initial setup
First of all, you need to connect to a fully synced [Joystream full node](https://github.com/Joystream/joystream/releases). By default, the program assumes you are running a node on the same device. For instructions on how to set this up, go [here](/roles/validators). Note that you can disregard all the parts about keys before applying, and just install the software so it is ready to go.

We strongly encourage that you run both the [node](/roles/validators#run-as-a-service) and the other software below as a service.

Now, get the additional dependencies:
```
$ apt-get update && apt-get upgrade -y
```

## Setup Hosting
In order to allow for users to upload and download, you have to setup hosting, with an actual domain as both Chrome and Firefox requires `https://`. If you have a "spare" domain or subdomain you don't mind using for this purpose, go to your domain registrar and point your domain to the IP you want. If you don't, you will need to purchase one.

### Caddy
To configure SSL-certificates the easiest is to use [caddy](https://caddyserver.com/), but feel free to take a different approach. Note that if you are using caddy for commercial use, you need to acquire a license. Please check their terms and make sure you comply with what is considered personal use.

For the best setup, you should use the "official" [documentation](https://caddyserver.com/docs/).

The instructions below are for Caddy v2.4.6:
```
$ wget https://github.com/caddyserver/caddy/releases/download/v2.4.6/caddy_2.4.6_linux_amd64.tar.gz
$ tar -vxf caddy_2.4.6_linux_amd64.tar.gz
$ mv caddy /usr/bin/
# Test that it's working:
$ caddy version
```

Configure the `Caddyfile`:
```
$ nano ~/Caddyfile
# Modify, and paste in everything below the stapled line
---
# Storage Node
https://<your.cool.url>/storage/* {
        log {
                output stdout
        }
        route /storage/* {
                uri strip_prefix /storage
                reverse_proxy localhost:3333
        }
        header /storage/api/v1/ {
                Access-Control-Allow-Methods "GET, PUT, HEAD, OPTIONS"
                Access-Control-Allow-Headers "GET, PUT, HEAD, OPTIONS"
        }
        request_body {
                max_size 10GB
        }
}
```

Now you can check if you configured correctly, with:
```
$ caddy validate ~/Caddyfile
# Which should return:
--
...
Valid configuration
--
# You can now run caddy with:
$ caddy run --config /root/Caddyfile
# Which should return something like:
--
...
... [INFO] [<your.cool.url>] The server validated our request
... [INFO] [<your.cool.url>] acme: Validations succeeded; requesting certificates
... [INFO] [<your.cool.url>] Server responded with a certificate.
... [INFO][<your.cool.url>] Certificate obtained successfully
... [INFO][<your.cool.url>] Obtain: Releasing lock
```

### Run caddy as a service
To ensure high uptime, it's best to set the system up as a `service`.

Example file below:

```
$ nano /etc/systemd/system/caddy.service

# Modify, and paste in everything below the stapled line
---
[Unit]
Description=Caddy
Documentation=https://caddyserver.com/docs/
After=network.target

[Service]
User=root
ExecStart=/usr/bin/caddy run --config /root/Caddyfile
ExecReload=/usr/bin/caddy reload --config /root/Caddyfile
TimeoutStopSec=5s
LimitNOFILE=1048576
LimitNPROC=512
PrivateTmp=true
ProtectSystem=full
AmbientCapabilities=CAP_NET_BIND_SERVICE

[Install]
WantedBy=multi-user.target
```
Save and exit. Close `caddy` if it's still running, then:
```
$ systemctl start caddy
# If everything works, you should get an output. Verify with:
$ systemctl status caddy
# Which should produce something similar to the previous output.
# To have caddy start automatically at reboot:
$ systemctl enable caddy
# If you want to stop caddy:
$ systemctl stop caddy
# If you want to edit your Caddfile, edit it, then run:
$ caddy reload
```

## Setup Query Node
[Go here for the installation guide](#tools/query-node/README.md).

## Install and Setup the Storage Node

First, you need to clone the Joystream monorepo, which contains the storage software.
Note that if you already have a storage-node installed (or running), go [here](#update-your-storage-node).

```
$ git clone https://github.com/Joystream/joystream.git
$ cd joystream
$ ./setup.sh
# this requires you to start a new session. if you are using a vps:
$ exit
$ ssh user@ipOrURL
# on your local machine, just close the terminal and open a new one
$ ./build-packages
$ yarn storage-node --help
```

### Applying for a Storage Provider opening

Click [here](https://testnet.joystream.org) to open the `Pioneer app` in your browser. Then follow instructions [here](https://github.com/Joystream/helpdesk#get-started) to generate a set of `Keys`, get tokens, and sign up for a `Membership`. This `key` will be referred to as the `member` key.

Make sure to save the `5YourJoyMemberAddress.json` file. This key will require tokens to be used as stake for the `Storage Provider` application (`application stake`) and further stake may be required if you are selected for the role (`role stake`).

To check for current openings, visit [this page](https://testnet.joystream.org/#/working-groups/opportunities) on Pioneer and look for any `Storage Provider` applications which are open for applications. If there is an opening available, fill in the details requested in the form required and stake the tokens needed to apply (when prompted you can sign a transaction for this purpose).

During this process you will be provided with a role key, which will be made available to download in the format `5YourStorageRoleKey.json`. If you set a password for this key, remember it :)

The next steps (below) will only apply if you are a successful applicant.


### Setup and configure the storage node

On the machine/VPS you want to run your storage node:

```
$ mkdir ~/keys/
$ cd ~/joystream/
$ yarn joystream-cli account:create

# give it the name:
  storage-operator-key

# this guide assumes you don't set a password

cat /root/.local/share/joystream-cli/accounts/storage-operator-key.json
```
This will give show you the address:
`..."address":"5StorageOperatorKey"...`


Assuming you are running the storage node on a VPS via ssh, on your local machine:
```
# Go the directory where you saved your <5YourStorageRoleKey.json>, then rename it to

storage-role-key.json

$ scp storage-role-key.json <user>@<your.vps.ip.address>:/root/keys/
```

**Make sure your [Joystream full node](https://github.com/Joystream/joystream/releases) and [Query Node](#setup-query-node) is fully synced before you move to the next step(s)!**

### Accept Invitation
Once hired, the Storage Lead will invite you a to "bucket". Before this is done, you will not be able to participate. Assuming:
- your Worker ID is `<workerId>`
- the Lead has invited to bucket `<bucketId>`

```
$ cd ~/joystream
$ yarn run storage-node operator:accept-invitation -i <bucketId> -k /root/keys/storage-role-key.json -w <workerId> -t <5StorageOperatorKey>
```

### Set Metadata
When you have accepted the invitation, you have to set metadata for your node. If your VPS is in Frankfurt, Germany:

```
$ nano metadata.json
# Modify, and paste in everything below the stapled line
---
{
  "endpoint": "https://<your.cool.url>/storage/",
  "location": {
    "countryCode": "DE",
    "city": "Frankfurt",
    "coordinates": {
      "latitude": 52,
      "longitude": 15
    }
  },
  "extra": "Hello world!"
}
```
Where:
- The location should really be correct (you can google your way to latitude/longitude)
- extra is not that critical. It could perhaps be nice to add some info on your max capacity?

## Deploy the Storage Node
First, create a `systemd` file. Example file below:

```
$ nano /etc/systemd/system/storage-node.service

# Modify, and paste in everything below the stapled line
---
[Unit]
Description=Joystream Storage Node
After=network.target joystream-node.service

[Service]
User=root
WorkingDirectory=/root/joystream/
LimitNOFILE=10000
ExecStart=/root/.volta/bin/yarn storage-node server \
        -u ws://localhost:9944 \
        -w <workerId> \
        -o 3333 \
        -l /<root/joystream-storage>/log/ \
        -d /<root/joystream-storage> \
        -q http://localhost:8081/graphql \
        -k /root/keys/storage-role-key.json \
        -s
Restart=on-failure
StartLimitInterval=600

[Install]
WantedBy=multi-user.target
```

If you (like most) have needed to buy extra storage volume, remember to set `-d /path/to/volume`
Save and exit.

```
$ systemctl start storage-node
# If everything works, you should get an output. Verify with:
$ journalctl -f -n 200 -u storage-node

# If it looks ok, it probably is :)
---

# To have colossus start automatically at reboot:
$ systemctl enable storage-node
# If you want to stop the storage node, either to edit the storage-node.service file or some other reason:
$ systemctl stop storage-node
```

### Verify everything is working

In your browser, try:
`https://<your.cool.url>/storage/api/v1/state/data`.

# Troubleshooting
If you had any issues setting it up, you may find your answer here!
