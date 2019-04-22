<p align="center"><img src="validator.png"></p>

<div align="center">
  <h4>This is a step-by-step guide to setup your <a href="https://github.com/Joystream/substrate-node-joystream">full node</a>, and get started as a Validator on the latest
  <a href="https://testnet.joystream.org/pioneer">Joystream Testnet</a><h4>
</div>



# Table of contents

- [Overview](#overview)
- [Instructions](#instructions)
    - [Windows](#windows)
        - [Setup node](#setup-node)
        - [Generate your keys](#generate-your-keys)
        - [Re-start your node as a validator](#re-start-your-node-as-a-validator)
        - [Configure your validator keys](#configure-your-validator-keys)
    - [Mac](#mac)
        - [Setup node](#setup-node-1)
        - [Generate your keys](#generate-your-keys-1)
        - [Re-start your node as a validator](#re-start-your-node-as-a-validator-1)
        - [Configure your validator keys](#configure-your-validator-keys-1)
    - [Linux](#linux)
        - [Setup node](#setup-node-2)
        - [Generate your keys](#generate-your-keys-2)
        - [Re-start your node as a validator](#re-start-your-node-as-a-validator-2)
        - [Configure your validator keys](#configure-your-validator-keys-2)
- [Nominating](#nominating)
- [Troubleshooting](#troubleshooting)
    - [Session Key](#session-key)
    - [Unstaking](#unstaking)


# Overview

This page contains all information on how to setup your node and becoming a `Validator` on the Joystream Testnets. It will be updated for improvements, and when something changes for new testnets.

If you want to earn more `Joy` tokens, but for some reason can´t or won't become a `Validator`, you can `Nominate` instead.

# Instructions

The instructions below covers Windows, Mac and Linux (64 bit and armv7). As a general note, remember to use your `session` key when setting the `memo` to qualify for the monero rewards.
Some browsers will work better than others, but in general, chrome and chromium based browsers seems to offer the best experience, as it allows the `Pioneer` app to connect to your own node in `Settings`. It seems neither Firefox, Safari or Edge will connect at this time.

If you want to be visible in the polkadot/substrate telemetry, go [here](https://telemetry.polkadot.io/). Note that for windows and armv7 (raspberry pi), you need to add a telemetry flag at startup (see applicable setup node).

**Note**
After introducing `Memberships` to the platform, we found it to be confusing to have a concept of both `Accounts` and `Memberships`. We are in the process of renaming the `Accounts` to the `Keys`, but there are still traces of `Account` showing up.

---

## Windows

* Every time something is written in `<brackets>`, this means you have to replace this with your input, without the `<>`.
* When something is written in `"double_quotes"`, it means the number/data will vary depending on your node or the current state of the blockchain.
* For terminal commands, `>` means you must type what comes after that on windows and mac respectively. `#` Means it's just a comment/explanation, and must not be typed.
```
# This is just a comment, don't type or paste it in your terminal!
> cd C:\joystream-node-1.0.0-windows-x86_64
# Only type/paste the "cd C:\joystream-node-windows-x64", not the preceding > !
```
#### Setup Node

Get the binary [here](https://github.com/Joystream/substrate-node-joystream/releases/download/v1.0.0/joystream-node-1.0.0-windows-x86_64.zip). To make the actual commands the same for all users, I'm going to save it `C:\` and unzip it there. `C:\joystream-node-1.0.0-windows-x86_64`. Feel free to store it somewhere else, just make sure you use the correct path in the instructions that follows.

If you don't have it, download Microsoft Visual Studio C++ runtime distributable 2015 [here](https://www.microsoft.com/en-ie/download/details.aspx?id=48145).  

Get the missing ssl libraries [here](https://indy.fulgan.com/SSL/openssl-1.0.2q-x64_86-win64.zip), extract, and move the files `ssleay32.dll` and `libeay32.dll` to `C:\joystream-node-windows-x64`.

Open `Command Prompt` (type in cmd... after clicking windows button):

```
> cd C:\joystream-node-1.0.0-windows-x86_64
> joystream-node.exe
# If you want your node to have a non-random identifier:
> joystream-node.exe --name <nodename>
# If you want your node to show up in the telemetry: https://telemetry.polkadot.io/
> joystream-node.exe --name <nodename> --telemetry-url ws://telemetry.polkadot.io:1024/
```
Your node should now start syncing the blockchain. The output should look like this:
```
Joystream Node
  version "Version"-"your_OS"
  by Joystream, 2019
Chain specification: Joystream Testnet v2
Node name: "nodename"
Roles: FULL
Generated a new keypair: "some_long_ouput"
Initializing Genesis block/state ("some_long_ouput")
Loaded block-time = 6 seconds from genesis on first-launch startup.
Best block: #0
Local node address is: /ip4/0.0.0.0/tcp/30333/p2p/"your_node_key"
Listening for new connections on 127.0.0.1:9944.
...
...
Syncing, target=#"block_height" ("n" peers), best: #"synced_height" ("hash_of_synced_tip"), finalized #0 ("hash_of_finalized_tip"), ⬇ "download_speed"iB/s ⬆ "upload_speed"kiB/s
```
From the last line, notice `target=#"block_height"` and `best: #"synced_height"`
When the `target=#block_height`is the same as `best: #"synced_height"`, your node is fully synced!

**Keep the terminal window open.**

Now you need to generate your `keys` in the `Pioneer app`. If you want to have the application talk to your own node, choose `Settings` in the sidebar, and change the `remote node/endpoint to connect to` to local node.

#### Generate your keys

While the node is syncing, you can start the process of setting up the rest.

1. Go the [Pioneer App](https://testnet.joystream.org/pioneer), and select `My keys` in the sidebar. Click the `Generate keys` tab.

Names are entirely optional, but the next steps will be easier if you follow the system suggested.

2. Name your first keypair `session`, or at least something that contains the word. Ie `john-doe-session-key`.
3. In the dropdown in the field below, choose `Raw seed`. Note that each time you toggle between `Mnemonic` and `Raw seed`, you will generate a new key pair.
4. Copy the `"0xYourLongSessionRawSeed"`, and save it somewhere safe - like a password manager. You need this later!
5. Choose a password (this key will hold all your tokens!)
6. For the `session` key, you also need to select `Edwards (ed25519)` from the `Advanced creation options`.
7. Click `Save` -> `Create and backup keys`

Depending on your browser, you might have to confirm saving the `"5YourJoySessionAddress.json"`.

Repeat the steps two more times, but with different names, leaving you with three sets of keys as shown below:
* `stash`
* `controller`
* `session`

Note that you only *strictly need* the Raw seed for the `session` keypair, but it's safer to do it for all of them.

#### Re-start your node as a validator

1. Open the terminal that is running your node, and kill the session with `ctrl+c` (twice).
    * On windows, the first`ctrl+c` this will produce a long and confusing output.
2. Restart it again with the following command:
```
> joystream-node.exe --validator --key <0xMyLongRawSeed>
# If you want your node to have a non-random identifier:
> joystream-node.exe --name <nodename> --validator --key <0xYourLongSessionRawSeed>
# If you also want it show up in telemetry:
> joystream-node.exe --name <nodename> --telemetry-url ws://telemetry.polkadot.io:1024/ --validator --key <0xYourLongSessionRawSeed>
```
This time, the output should show a slightly different startup output:
```
Joystream Node
  version "version"-"your_OS"
  by Joystream, 2019
Chain specification: Joystream Staging Testnet
Node name: "nodename"
Roles: AUTHORITY
Best block: #"synced_height"
Local node address is: /ip4/0.0.0.0/tcp/30333/p2p/"your_node_key"
Listening for new connections on 127.0.0.1:9944.
Using authority key  "5YourJoySessionAddress"  # See Note
...
```
**Note**
If your `session` was generated as `Schnorrkel (sr25519)`, it will show a completely different address. If this happens, go back and generate a new [session key](#generate-your-keys) with `Edwards (ed25519)`. If you don't, your node will try to sign blocks with the wrong key. As a consequence, you will get get slashed and kicked out as `Validator`.

#### Configure your validator keys

In order to be a `validator`, you need stake. Note that you may have to refresh your browser if you're not seeing the options right away.

1. Still in the `My keys` sidebar, choose your `stash` key.
2. Click the `Get free tokens` link below your address, [or click here](https://testnet.joystream.org/faucet). Solve the captcha, and you should receive tokens.
3. Send some tokens to your `controller`. It needs to perform at least two transaction, but better to send ~10.
4. Now, click `Validators` in the sidebar, and then the `Validator staking` tab.
5. Locate the address/key named `stash`, and click `Bond Funds`.
6. In the popup window, choose your `controller` as the `controller account`.
7. Enter the amount you want to stake in the `value bonded` field. Best to leave a little, in case you need to make a transaction later.
8. In the `payment destination` dropdown, there are three options.
    * As the validator algorithm will have a preference towards the `Validators` with the most stake, you are most likely to stay competitive if you select the default `Stash account (increase the amount at stake)`. You can always top up later with `Bond additional`.
9. Your `controller` account should now show a `Set Session Key` button. Click it.
10. In the popup, select your `session` as your `session key` in the dropdown. Confirm, sign and submit.
11. Your `controller` account should now show a `Validate` button. Click it.
12. You can leave the `unstake threshold` and `payment preferences` as defaults. Confirm, sign and submit.

Refresh your browser, and select the `Validator Overview` tab. If your account shows under `next up`, wait for the next `era`, and you will be moved to the `validators` list.

---

## Mac

* Every time something is written in `<brackets>`, this means you have to replace this with your input, without the `<>`.
* When something is written in `"double_quotes"`, it means the number/data will vary depending on your node or the current state of the blockchain.
* For terminal commands, `$` means you must type what comes after that on windows and mac respectively. `#` Means it's just a comment/explanation, and must not be typed.
```
# This is just a comment, don't type or paste it in your terminal!
$ cd ~/
# Only type/paste the "cd ~/, not the preceding $ !
```
#### Setup Node

Open the terminal (Applications->Utilities):

```
$ cd ~/
$ wget https://github.com/Joystream/substrate-node-joystream/releases/download/v1.0.0/joystream-node-1.0.0-osx-x86_64.zip
----
# If you don't have wget installed, paste the link in your browser save.
# Assuming it gets saved in your ~/Downloads folder:
$ mv ~/Downloads/joystream-node-1.0.0-osx-x86_64.zip ~/
---
$ tar -vxf joystream-node-1.0.0-osx-x86_64.zip
$ ./joystream-node
# If you want your node to have a non-random identifier:
> ./joystream-node --name <nodename>
```
Your node should now start syncing the blockchain. The output should look like this:
```
Joystream Node
  version "Version"-"your_OS"
  by Joystream, 2019
Chain specification: Joystream Testnet v2
Node name: "nodename"
Roles: FULL
Generated a new keypair: "some_long_ouput"
Initializing Genesis block/state ("some_long_ouput")
Loaded block-time = 6 seconds from genesis on first-launch startup.
Best block: #0
Local node address is: /ip4/0.0.0.0/tcp/30333/p2p/"your_node_key"
Listening for new connections on 127.0.0.1:9944.
...
...
Syncing, target=#"block_height" ("n" peers), best: #"synced_height" ("hash_of_synced_tip"), finalized #0 ("hash_of_finalized_tip"), ⬇ "download_speed"iB/s ⬆ "upload_speed"kiB/s
```
From the last line, notice `target=#"block_height"` and `best: #"synced_height"`
When the `target=#block_height`is the same as `best: #"synced_height"`, your node is fully synced!

**Keep the terminal window open.**

Now you need to generate your `keys` in the `Pioneer app`. If you want to have the application talk to your own node, choose `Settings` in the sidebar, and change the `remote node/endpoint to connect to` to local node.

#### Generate your keys

While the node is syncing, you can start the process of setting up the rest.

1. Go the [Pioneer App](https://testnet.joystream.org/pioneer), and select `My keys` in the sidebar. Click the `Generate keys` tab.

Names are entirely optional, but the next steps will be easier if you follow the system suggested.

2. Name your first keypair `session`, or at least something that contains the word. Ie `john-doe-session-key`.
3. In the dropdown in the field below, choose `Raw seed`. Note that each time you toggle between `Mnemonic` and `Raw seed`, you will generate a new key pair.
4. Copy the `"0xYourLongSessionRawSeed"`, and save it somewhere safe - like a password manager. You need this later!
5. Choose a password (this key will hold all your tokens!)
6. For the `session` key, you also need to select `Edwards (ed25519)` from the `Advanced creation options`.
7. Click `Save` -> `Create and backup keys`

Depending on your browser, you might have to confirm saving the `"5YourJoySessionAddress.json"`.

Repeat the steps two more times, but with different names, leaving you with three sets of keys as shown below:
* `stash`
* `controller`
* `session`

Note that you only *strictly need* the Raw seed for the `session` keypair, but it's safer to do it for all of them.

#### Re-start your node as a validator

1. Open the terminal that is running your node, and kill the session with `ctrl+c`.
2. Restart it again with the following command:
```
$ ./joystream-node --validator --key <0xMyLongRawSeed>
# If you want your node to have a non-random identifier:
$ ./joystream-node --name <nodename> --validator --key <0xYourLongSessionRawSeed>
```
This time, the output should show a slightly different startup output:
```
Joystream Node
  version "version"-"your_OS"
  by Joystream, 2019
Chain specification: Joystream Staging Testnet
Node name: "nodename"
Roles: AUTHORITY
Best block: #"synced_height"
Local node address is: /ip4/0.0.0.0/tcp/30333/p2p/"your_node_key"
Listening for new connections on 127.0.0.1:9944.
Using authority key  "5YourJoySessionAddress"  # See Note
...
```
**Note**
If your `session` was generated as `Schnorrkel (sr25519)`, it will show a completely different address. If this happens, go back and generate a new [session key](#generate-your-keys-1) with `Edwards (ed25519)`. If you don't, your node will try to sign blocks with the wrong key. As a consequence, you will get get slashed and kicked out as `Validator`.

#### Configure your validator keys

In order to be a `validator`, you need stake. Note that you may have to refresh your browser if you're not seeing the options right away.

1. Still in the `My keys` sidebar, choose your `stash` key.
2. Click the `Get free tokens` link below your address, [or click here](https://testnet.joystream.org/faucet). Solve the captcha, and you should receive tokens.
3. Send some tokens to your `controller`. It needs to perform at least two transaction, but better to send ~10.
4. Now, click `Validators` in the sidebar, and then the `Validator staking` tab.
5. Locate the address/key named `stash`, and click `Bond Funds`.
6. In the popup window, choose your `controller` as the `controller account`.
7. Enter the amount you want to stake in the `value bonded` field. Best to leave a little, in case you need to make a transaction later.
8. In the `payment destination` dropdown, there are three options.
    * As the validator algorithm will have a preference towards the `Validators` with the most stake, you are most likely to stay competitive if you select the default `Stash account (increase the amount at stake)`. You can always top up later with `Bond additional`.
9. Your `controller` account should now show a `Set Session Key` button. Click it.
10. In the popup, select your `session` as your `session key` in the dropdown. Confirm, sign and submit.
11. Your `controller` account should now show a `Validate` button. Click it.
12. You can leave the `unstake threshold` and `payment preferences` as defaults. Confirm, sign and submit.

Refresh your browser, and select the `Validator Overview` tab. If your account shows under `next up`, wait for the next `era`, and you will be moved to the `validators` list.

---

## Linux

* Every time something is written in `<brackets>`, this means you have to replace this with your input, without the `<>`.
* When something is written in `"double_quotes"`, it means the number/data will vary depending on your node or the current state of the blockchain.
* For terminal commands, `$` means you must type what comes after that on windows and mac respectively. `#` Means it's just a comment/explanation, and must not be typed.
```
# This is just a comment, don't type or paste it in your terminal!
$ cd ~/
# Only type/paste the "cd ~/, not the preceding $ !
```
#### Setup Node

Open the terminal:

```
$ cd ~/
# 64 bit debian based linux
$ wget https://github.com/Joystream/substrate-node-joystream/releases/download/v1.0.0/joystream-node-1.0.0-linux-x86_64.tar.gz
$ tar -vxf joystream-node-1.0.0-linux-x86_64.tar.gz
# armv7 (raspberry pi)
$ wget https://github.com/Joystream/substrate-node-joystream/releases/download/v1.0.0/joystream-node-1.0.0-armv7.tar.gz
$ tar -vxf joystream-node-1.0.0-armv7.tar.gz
# For both
$ ./joystream-node
# If you want your node to have a non-random identifier:
$ ./joystream-node --name <nodename>
# armv7 (raspberry pi) only:
# If you want your node to show up in the telemetry: https://telemetry.polkadot.io/
$ ./joystream-node --name <nodename> --telemetry-url ws://telemetry.polkadot.io:1024/
```
Your node should now start syncing the blockchain. The output should look like this:
```
Joystream Node
  version "Version"-"your_OS"
  by Joystream, 2019
Chain specification: Joystream Testnet v2
Node name: "nodename"
Roles: FULL
Generated a new keypair: "some_long_ouput"
Initializing Genesis block/state ("some_long_ouput")
Loaded block-time = 6 seconds from genesis on first-launch startup.
Best block: #0
Local node address is: /ip4/0.0.0.0/tcp/30333/p2p/"your_node_key"
Listening for new connections on 127.0.0.1:9944.
...
...
Syncing, target=#"block_height" ("n" peers), best: #"synced_height" ("hash_of_synced_tip"), finalized #0 ("hash_of_finalized_tip"), ⬇ "download_speed"iB/s ⬆ "upload_speed"kiB/s
```
From the last line, notice `target=#"block_height"` and `best: #"synced_height"`
When the `target=#block_height`is the same as `best: #"synced_height"`, your node is fully synced!

**Keep the terminal window open.**

Now you need to generate your `keys` in the `Pioneer app`. If you want to have the application talk to your own node, choose `Settings` in the sidebar, and change the `remote node/endpoint to connect to` to local node.

#### Generate your keys

While the node is syncing, you can start the process of setting up the rest.

1. Go the [Pioneer App](https://testnet.joystream.org/pioneer), and select `My keys` in the sidebar. Click the `Generate keys` tab.

Names are entirely optional, but the next steps will be easier if you follow the system suggested.

2. Name your first keypair `session`, or at least something that contains the word. Ie `john-doe-session-key`.
3. In the dropdown in the field below, choose `Raw seed`. Note that each time you toggle between `Mnemonic` and `Raw seed`, you will generate a new key pair.
4. Copy the `"0xYourLongSessionRawSeed"`, and save it somewhere safe - like a password manager. You need this later!
5. Choose a password (this key will hold all your tokens!)
6. For the `session` key, you also need to select `Edwards (ed25519)` from the `Advanced creation options`.
7. Click `Save` -> `Create and backup keys`

Depending on your browser, you might have to confirm saving the `"5YourJoySessionAddress.json"`.

Repeat the steps two more times, but with different names, leaving you with three sets of keys as shown below:
* `stash`
* `controller`
* `session`

Note that you only *strictly need* the Raw seed for the `session` keypair, but it's safer to do it for all of them.

#### Re-start your node as a validator

1. Open the terminal that is running your node, and kill the session with `ctrl+c`.
2. Restart it again with the following command:
```
$ ./joystream-node --validator --key <0xMyLongRawSeed>
# If you want your node to have a non-random identifier:
$ ./joystream-node --name <nodename> --validator --key <0xYourLongSessionRawSeed>
# armv7 (raspberry pi) only:
# If you want your node to show up in the telemetry: https://telemetry.polkadot.io/
$ ./joystream-node --name <nodename> --validator --key <0xYourLongSessionRawSeed> --telemetry-url ws://telemetry.polkadot.io:1024/
```
This time, the output should show a slightly different startup output:
```
Joystream Node
  version "version"-"your_OS"
  by Joystream, 2019
Chain specification: Joystream Staging Testnet
Node name: "nodename"
Roles: AUTHORITY
Best block: #"synced_height"
Local node address is: /ip4/0.0.0.0/tcp/30333/p2p/"your_node_key"
Listening for new connections on 127.0.0.1:9944.
Using authority key  "5YourJoySessionAddress"  # See Note
...
```
**Note**
If your `session` was generated as `Schnorrkel (sr25519)`, it will show a completely different address. If this happens, go back and generate a new [session key](#generate-your-keys-2) with `Edwards (ed25519)`. If you don't, your node will try to sign blocks with the wrong key. As a consequence, you will get get slashed and kicked out as `Validator`.

#### Configure your validator keys

In order to be a `validator`, you need stake. Note that you may have to refresh your browser if you're not seeing the options right away.

1. Still in the `My keys` sidebar, choose your `stash` key.
2. Click the `Get free tokens` link below your address, [or click here](https://testnet.joystream.org/faucet). Solve the captcha, and you should receive tokens.
3. Send some tokens to your `controller`. It needs to perform at least two transaction, but better to send ~10.
4. Now, click `Validators` in the sidebar, and then the `Validator staking` tab.
5. Locate the address/key named `stash`, and click `Bond Funds`.
6. In the popup window, choose your `controller` as the `controller account`.
7. Enter the amount you want to stake in the `value bonded` field.
8. In the `payment destination` dropdown, there are three options.
    * As the validator algorithm will have a preference towards the `Validators` with the most stake, you are most likely to stay competitive if you select the default `Stash account (increase the amount at stake)`. You can always top up later with `Bond additional`.
9. Your `controller` account should now show a `Set Session Key` button. Click it.
10. In the popup, select your `session` as your `session key` in the dropdown. Confirm, sign and submit.
11. Your `controller` account should now show a `Validate` button. Click it.
12. You can leave the `unstake threshold` and `payment preferences` as defaults. Confirm, sign and submit.

Refresh your browser, and select the `Validator Overview` tab. If your account shows under `next up`, wait for the next `era`, and you will be moved to the `validators` list.

---

# Nominating

If you want to get some return on your tokens without running a node yourself, you can `nominate` another `validator` and get a share of their rewards.

This might also come in handy if there are too many `validators` and you don't have enough tokens get a spot, or if you have to shut down your own node for a while.

## Generate keys
If you haven't already been through the process of setting up your `stash`, `controller` and `session` key, you first need to [generate your keys](#generate-your-keys). Note that you don't need a `session` key to nominate, so you can skip those steps.

## Configure your nominating keys
In order to be a `nominator`, you need stake. Note that you may have to refresh your browser if you're not seeing the options right away. If you have previously been a `Validator`, or tried to do so, skip ahead to step `9.`.

1. In the `My keys` sidebar, choose your `stash` key.
2. Click the `Get free tokens` link below your address, [or click here](https://testnet.joystream.org/faucet). Solve the captcha, and you should receive tokens.
3. Send some tokens to your `controller`. It needs to perform at least two transaction, but better to send ~10.
4. Now, click `Validators` in the sidebar, and then the `Validator staking` tab.
5. Locate the address/key named `stash`, and click `Bond Funds`.
6. In the popup window, choose your `controller` as the `controller account`.
7. Enter the amount you want to stake in the `value bonded` field.
8. In the `payment destination` dropdown, there are three options.
    * As the validator algorithm will have a preference towards the `Validators` with the most stake, you are most likely to stay competitive if you select the default `Stash account (increase the amount at stake)`. You can always top up later with `Bond additional`.
9. Your `controller` account should now show a `Set Session Key` and a `Nominating` button. Click the latter.
10. In the popup, select/paste the `stash` address of the `Validator` you wish to nominate. Confirm, sign and submit.

In the next `era`, you will show as a `nominator` of the `Validator` you nominated.

---

# Troubleshooting
If you had any issues setting it up, you may find your answer here!

#### Session key
Did you accidentally choose `Schnorrkel (sr25519)`, instead of `Edwards (ed25519)` for your `session` key, and didn't notice before you configured your `Validator keys`? This can be resolved.

1. Go to `Validators` -> `Validator staking` and `Unstake`.

2. Generate a new `session` key with `Edwards (ed25519)`, restart your node, and replace the `raw seed` with the new one.

3. Then, choose `Settings` in the sidebar, and switch from `Basic features only` to `Fully featured`.

4. Go to `Extrinsics`, and select your `controller` key from the dropdown at the top. In the second dropdown, select `session`, and in the third, `setKey`. Finally, choose your new `session` key in the fourth and final dropdown, and submit.

5. Once it confirms, go back to `Validators` -> `Validator staking` and `Stake`.

In the `Next up`, your new `session` key should show, and match the `authority key` in your node. (minus the final 3 characters).

#### Unstaking
If you stop validating by killing your node before unstaking, you will get slashed and kicked from the `validator` set. If you know in advance (~1hr) you can do the following steps instead:

First, make sure you have set `Fully Featured` interface in the `Settings` sidebar.
1. In `Validator Staking`, click `Stop Validating` with your controller. This can also be done via `Extrinsic`: With `controller` -> `staking` -> `chill()`.

If you are just pausing the `validator` and intend to start it up later, you can stop here. When you are ready to start again, fire up your node, go to `Validator Staking`, and click `Validate`.

If you want to stop being a `validator` and move your tokens to other/better use, continue.

---

2. Next you must unbond. In `Extrinsics`, using the `controller`, select `staking` -> `unbond(value)` and choose how much you want to unbond, `<unbonding>`. Submit Transaction.

At this point, you can just wait 2hrs to be sure, and proceed to step `6.` Or:

---

3. Go to `Chain State` -> `staking` -> `ledger(AccountId): Option<StakingLedger>` with the `controller`. Output:
```
# If you have successfully initiated unstaking:
{"stash":"5YourStashAddress","total":<tot_bonded>,"active":,<act_bonded>:[{"value":<unbonding>,"era":<E_unbonded>}]}
# If you have not successfully initiated unstaking, or it has already completed:
{"stash":"5YourStashAddress","total":<tot_bonded>,"active":,"<act_bonded>":[]}
```
* `<tot_bonded>` Is the total amount you have staked/bonded
* `<act_bonded>` Is the amount of tokens that is not being unlocked
* `<unbonding>` Is the amount of tokens that is in the process of being freed
  * `<unbonding>` + `<act_bonded>` = `<tot_bonded>`
* `<E_unbonded>` Is the `era` when your tokens will be "free" to transfer/bond/vote

The `era` should only change every 600 blocks, but certain events may trigger a new era. To calculate when your funds are "free"
4. In `Chain State` -> `staking` -> `currentEra()`. Let output be `<E_current>`
5. In `Explored` collect `<blocks_in_era>/600` under era.
```
600(<E_unbonded> - <E_current> - 1) - <blocks_in_era> = <blocks_left>
(<blocks_left> * 6)/60 = <minutes_left>
```
After `<minutes_left>` has passed, ie. `<E_current>` = `<E_unbonded>`, your tokens should be free.
Repeat step `3.` if you want to confirm.
```
# If you have completed unstaking:
{"stash":"5YourStashAddress","total":<tot_bonded>,"active":,"<act_bonded>":[]}
```
---

6. In `Extrinsics`, using the `controller`, select `staking` -> `withdrawUnbonded()`

Your tokens should now be "free".

#### Restart validating after getting booted
If your node shut down before you had stopped validating and/or the grace period for `staking.chill` was completed, all you need to is start `validating` again from `Validator Staking`. Just make sure that your node is back up, and the `authority` key showing at node startup is the same as your `session` key.
**Note**
It doesn't matter if your `stash` has a `balance` < `bonded`.
