<p align="center"><img src="validator.png"></p>

<div align="center">
  <h4>This is a step-by-step guide to setup your <a href="https://github.com/Joystream/substrate-node-joystream">full node</a>, and get started as a Validator on the latest
  <a href="https://testnet.joystream.org/pioneer">Joystream Testnet</a><h4>
</div>



# Table of contents

- [Overview](#overview)
- [Instructions](#instructions)
    - [Mac](#mac)
        - [Setup node](#setup-node)
        - [Generate your keys](#generate-your-keys)
        - [Re-start your node as a validator](#re-start-your-node-as-a-validator)
        - [Configure your validator keys](#configure-your-validator)
    - [Linux](#linux)
        - [Setup node](#setup-node-1)
        - [Generate your keys](#generate-your-keys-1)
        - [Re-start your node as a validator](#re-start-your-node-as-a-validator-1)
        - [Configure your validator keys](#configure-your-validator-1)
- [Troubleshooting](#troubleshooting)
    - [Session Key](#session-key)


# Overview

This page contains all information on how to setup your node and becoming a `Validator` on the Joystream Testnets. It will be updated for improvements, and when something changes for new testnets.

# Instructions

The instructions below covers Mac and Linux (64 bit and armv7) for now. Windows binary should come later. As a general note, remember to use your `session` key when setting the `memo` to qualify for the monero rewards. Some browsers will work better than others, but in general, chrome and chromium based browsers seems to offer the best experience, as it allows you to connect to your own node in `Settings`.

**Note**
After introducing `Memberships` to the platform, we found it to be confusing to have a concept of both `Accounts` and `Memberships`. We are in the process of renaming the `Accounts` to the `Keys`, but there are still traces of `Account` showing up.

---
<!---
## Windows

* Every time something is written in `<brackets>`, this means you have to replace this with your input, without the `<>`.
* When something is written in `"double_quotes"`, it means the number/data will vary depending on your node or the current state of the blockchain.
* For terminal commands, `>` means you must type what comes after that on windows and mac respectively. `#` Means it's just a comment/explanation, and must not be typed.
```
# This is just a comment, don't type or paste it in your terminal!
> cd C:\joystream-node-windows-x64
# Only type/paste the "cd C:\joystream-node-windows-x64", not the preceding > !
```
#### Setup Node

**Windows Binary Incoming! If you see this, check back in an hour or two.**

To make the actual commands the same for all users, I'm going to save it `C:\` and unzip it there. `C:\joystream-node-1.0.0-windows-x86_64`. Feel free to store it somewhere else, just make sure you use the correct path in the instructions that follow.

If you don't have it, download Microsoft Visual Studio C++ runtime distributable 2015 [here](https://www.microsoft.com/en-ie/download/details.aspx?id=48145).  

Get the missing ssl libraries [here](https://indy.fulgan.com/SSL/openssl-1.0.2q-x64_86-win64.zip), extract, and move the files `ssleay32.dll` and `libeay32.dll` to `C:\joystream-node-windows-x64`.

Open `Command Prompt` (type in cmd... after clicking windows button):

```
> cd C:\joystream-node-1.0.0-windows-x86_64
> joystream-node.exe
# If you want your node to have a non-random identifier:
> joystream-node.exe --name <nodename>
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
> joystream-node.exe --validator --key <0xMyLongRawSeed>
# If you want your node to have a non-random identifier:
> joystream-node.exe --name <nodename> --validator --key <0xYourLongSessionRawSeed>
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
There are some issues with the `authority key` currently.
* If your `session` was generated as `Edwards (ed25519)`, the last three characters in "5YourJoySessionAddress" the last three in the `Pioneer app`. This is to expected.
* If your `session` was generated as `Schnorrkel (sr25519)`, it will show a completely different address. If this happens, go back and generate a new [session key](#generate-your-keysw).

#### Configure your validator keys

In order to be a `validator`, you need stake. Note that you may have to refresh your browser if you're not seeing the options right away.

1. Still in the `My keys` sidebar, choose your `stash` key.
2. Click the `Get free tokens` link below your address, [or click here](https://testnet.joystream.org/faucet). Solve the captcha, and you should receive tokens.
3. Now, click `Validators` in the sidebar, and then the `Validator staking` tab.
4. Locate the address/key named `stash`, and click `Bond Funds`.
5. In the popup window, choose your `controller` as the `controller account`.
6. Enter the amount you want to stake in the `value bonded` field.
7. In the `payment destination` dropdown, there are three options.
    * As the validator algorithm will have a preference towards the `Validators` with the most stake, you are most likely to stay competitive if you select the default `Stash account (increase the amount at stake)`. You can always top up later with `Bond additional`.
8. Your `controller` account should now show a `Set Session Key` button. Click it.
9. In the popup, select your `session` as your `session key` in the dropdown. Confirm, sign and submit.
10. Your `controller` account should now show a `Validate` button. Click it.
11. You can leave the `unstake threshold` and `payment preferences` as defaults. Confirm, sign and submit.

Refresh your browser, and select the `Validator Overview` tab. If your account shows under `next up`, wait for the next `era`, and you will be moved to the `validators` list.

---
-->
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
There are some issues with the `authority key` currently.
* If your `session` was generated as `Edwards (ed25519)`, the last three characters in "5YourJoySessionAddress" the last three in the `Pioneer app`. This is to expected.
* If your `session` was generated as `Schnorrkel (sr25519)`, it will show a completely different address. If this happens, go back and generate a new [session key](#generate-your-keysm).

#### Configure your validator keys

In order to be a `validator`, you need stake. Note that you may have to refresh your browser if you're not seeing the options right away.

1. Still in the `My keys` sidebar, choose your `stash` key.
2. Click the `Get free tokens` link below your address, [or click here](https://testnet.joystream.org/faucet). Solve the captcha, and you should receive tokens.
3. Now, click `Validators` in the sidebar, and then the `Validator staking` tab.
4. Locate the address/key named `stash`, and click `Bond Funds`.
5. In the popup window, choose your `controller` as the `controller account`.
6. Enter the amount you want to stake in the `value bonded` field.
7. In the `payment destination` dropdown, there are three options.
    * As the validator algorithm will have a preference towards the `Validators` with the most stake, you are most likely to stay competitive if you select the default `Stash account (increase the amount at stake)`. You can always top up later with `Bond additional`.
8. Your `controller` account should now show a `Set Session Key` button. Click it.
9. In the popup, select your `session` as your `session key` in the dropdown. Confirm, sign and submit.
10. Your `controller` account should now show a `Validate` button. Click it.
11. You can leave the `unstake threshold` and `payment preferences` as defaults. Confirm, sign and submit.

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
# armv7 (raspberry pi)
$ wget https://github.com/Joystream/substrate-node-joystream/releases/download/v1.0.0/joystream-node-1.0.0-armv7.tar.gz
$ tar -vxf joystream-node-1.0.0-linux-x86_64.tar.gz
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
There are some issues with the `authority key` currently.
* If your `session` was generated as `Edwards (ed25519)`, the last three characters in "5YourJoySessionAddress" the last three in the `Pioneer app`. This is to expected.
* If your `session` was generated as `Schnorrkel (sr25519)`, it will show a completely different address. If this happens, go back and generate a new [session key](#generate-your-keysl).

#### Configure your validator keys

In order to be a `validator`, you need stake. Note that you may have to refresh your browser if you're not seeing the options right away.

1. Still in the `My keys` sidebar, choose your `stash` key.
2. Click the `Get free tokens` link below your address, [or click here](https://testnet.joystream.org/faucet). Solve the captcha, and you should receive tokens.
3. Now, click `Validators` in the sidebar, and then the `Validator staking` tab.
4. Locate the address/key named `stash`, and click `Bond Funds`.
5. In the popup window, choose your `controller` as the `controller account`.
6. Enter the amount you want to stake in the `value bonded` field.
7. In the `payment destination` dropdown, there are three options.
    * As the validator algorithm will have a preference towards the `Validators` with the most stake, you are most likely to stay competitive if you select the default `Stash account (increase the amount at stake)`. You can always top up later with `Bond additional`.
8. Your `controller` account should now show a `Set Session Key` button. Click it.
9. In the popup, select your `session` as your `session key` in the dropdown. Confirm, sign and submit.
10. Your `controller` account should now show a `Validate` button. Click it.
11. You can leave the `unstake threshold` and `payment preferences` as defaults. Confirm, sign and submit.

Refresh your browser, and select the `Validator Overview` tab. If your account shows under `next up`, wait for the next `era`, and you will be moved to the `validators` list.

---

# Troubleshooting
If you had any issues setting it up, you may find your answer here!

## Session Key
Did you accidentally choose `Schnorrkel (sr25519)`, instead of `Edwards (ed25519)` for your `session` key, and didn't notice before you configured your `Validator keys`? This can be resolved.

1. Go to `Validators` -> `Validator staking` and `Unstake`.

2. Generate a new `session` key with `Edwards (ed25519)`, restart your node, and replace the `raw seed` with the new one.

3. Then, choose `Settings` in the sidebar, and switch from `Basic features only` to `Fully featured`.

4. Go to `Extrinsics`, and select your `controller` key from the dropdown at the top. In the second dropdown, select `session`, and in the third, `setKey`. Finally, choose your new `session` key in the fourth and final dropdown, and submit.

5. Once it confirms, go back to `Validators` -> `Validator staking` and `Stake`.

In the `Next up`, your new `session` key should show, and match the `authority key` in your node. (minus the final 3 characters).
