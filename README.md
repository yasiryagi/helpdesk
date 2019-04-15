<p align="center"><img src="helpdesk-repo.svg"></p>

<div align="center">
  <h4>This repo contains detailed guides and help on how to use and interact with our current testnet.<h4>
</div>
<div align="center">
  <h5>Forward looking functionality and long term plans our <a href="https://github.com/Joystream/whitepaper/blob/master/paper.pdf">whitepaper</a> </h5>
</div>
<div align="center">
  These are our <a href="https://github.com/Joystream/manifesto">ends and means</a>
</div>

<br />

<div align="center">
  <h3>
    <a href="/roles/validators">
      Validators
    </a>
    <span> | </span>
    <a href="/roles/council-members">
      Council Members
    </a>
    <span> | </span>
    <a href="/roles/storage-providers">
      Storage Providers
    </a>
    <span> | </span>
    <a href="/roles/builder">
      Bug Reporters
    </a>
  </h3>
</div>

# Table of contents

- [Overview](#overview)
- [Contribute](#contribute)
- [Get Paid to Participate](#get-paid-to-participate)
    - [How it Works](#how-it-works)
- [Get Started](#get-started)
    - [Generate Keys](#generate-keys)
    - [Get a Membership](#get-a-membership)
- [Active Roles](#active-roles)
    - [Validator](#validator)
    - [Council Members](#council-members)
    - [Storage Providers](#storage-providers)
    - [Builders and Bug Reporters](#builders-and-bug-reporters)
- [Glossary](#glossary)
- [Troubleshooting](#troubleshooting)

# Overview
This repo contains detailed guides and help for user to interact with our current [testnet](http://testnet.joystream.org/).

# Contribute
If you find something that is wrong or missing, please make an [Issue](/issues), or better yet, fork the repo and make a [PR](https://github.com/Joystream/helpdesk/pulls) to help us improve! It might even qualify for a [reward](#builders-and-bug-reporters).

# Get Paid to Participate
Some of the roles will be compensated in [Monero](https://www.getmonero.org/). Our philosophy behind the choice of paying for participation is outlined in [this](https://blog.joystream.org/pay-for-play/) blog post. Information about the current incentive structure can be found in the list of [Active Roles](#active-roles) below.

## How it Works
**Note**
After introducing `Memberships` to the platform, we found it to be confusing to have a concept of both `Accounts` and `Memberships`. We are in the process of renaming the `Accounts` to the `Keys`, but there are still traces of `Account` showing up.

In order for us know what address to pay, you must tie your Joystream address to your monero address. The easiest way to do this is in the `My memo` tab under the `My Keys` sidebar.

```
# Only the part in the line below goes in the memo:
"4or8YourXmrAddressInDoubleQuotesAndNothingElse"
```

For our convenience, we ask you to use a regular address or sub-address (95 char) instead of the (106 char) payment-ID style address. The latter forces us to make multiple monero transactions instead of just one.

# Get Started
To get started and participate on the Joystream testnets, you must first generate `Key(s)`, and sign up for a `Membership`. This requires no software or downloads, and can be done in your browser [here](http://testnet.joystream.org).

## Generate Keys
Click `My keys` in the sidebar, and then select the `Generate Keys` tab. The choices you make from here, depends a little on how you want to participate. If you just want to play around, you can just follow the defaults. If you have a specific role in mind, you might want to follow the links to the instructions in the header, or access them via [Active Roles](#active-roles).

In any event, the `Keys` will be stored in your browser for your convenience, but it's safest to save your `Raw seed` (you need it for certain roles) and save the .json file. The `Mnemonic` can also be used to restore your `Keys`, but will not do you any good if you want to become a `Validator`.

## Get a Membership
To become a `Member` of the platform, you need some tokens. Either click the `Get free tokens` link, or click [here](https://testnet.joystream.org/faucet). After you solved the captcha, your tokens should be on their way.

Now, click `Members` in the sidebar, and select the `Register` tab. Choose a `Handle/nickname`. Optionally, provide a link to an image file for your avatar, and fill in the markdown enabled `About` field.

# Active Roles

The list below shows the currently active roles available at our current [testnet](https://testnet.joystream.org/pioneer).

## Validators

<p align="center"><img src="validator_earn.png"></p>

In proof of stake systems, block producers, or `Validators`, are typically paid a fixed amount for each block produced. While Sparta has been running, we have learned that the interest for being a `Validator` was higher than we anticipated, so we are increasing the [`validator_count`](https://github.com/Joystream/substrate-node-joystream/blob/03f87d875098511caea98d42b233bf12e3d66999/src/chain_spec.rs/#L164) from 10 to 20. To avoid reducing individual rewards too much, we are increasing the pool from $20 to $30 per week.

```
blocktime = 6
weekly_reward = 3000
seconds_in_week = 60*60*24*7

blockreward = (weekly_reward * blocktime)/seconds_in_week
print(blockreward)

----

0.03
```

The number - 0.03 cents per block - seems a bit underwhelming, but validation requires little effort for the user after setup, and with armv7 binaries, it should be cheap to run! Payouts will be made every Monday at ~11:00GMT.

## Council Members

<p align="center"><img src="council_earn.png"></p>

`Council Members` are elected by the stakeholders in the system to act in the interest of their constituency. Somewhat simplified, the council will allocate the platforms resources, and hire executive personnel to run the day to day.

We are looking at how to best incentivize them to act in the platforms long term interest. As this position generated less interest than we anticipated, we are tweaking the incentives by increasing the payout to get elected from $5 to $8.

If during you term a proposal to upgrade the runtime is submitted by the `sudo` key, `5CJzTaCp5fuqG7NdJQ6oUCwdmFHKichew8w4RZ3zFHM8qSe6` anyone that votes yes will receive a bonus worth $5.

During the `Announcement` and `Voting` stage, you should include some information about yourself, and why you should get elected in your `memo` field. If you do get elected, make sure to change the `memo` field to your monero address in order to get your reward. Payouts will occur at ~11:00GMT the day after the election/vote.

## Storage Providers

<p align="center"><img src="storage_earn.png"></p>

You can't have a video platform without videos, so someone has to take the role storing the data. In the future, this will be highly specialized role, focusing on what is implied by the name of the role. For Athens, it will in practice also entail the future `Bandwidth Provider` role.

Unlike `Validators` that can come and go without too much friction (at least for now), a new `Storage Provider` will currently need to replicate the entire content directory. As a consequence, the platform needs some stability for this role to avoid providing a poor user experience, or worse, loss of data.

Up to 10 `Storage Providers` that keeps a full copy and provides continues service for at least 24h will compete for $75 per week. In addition, you will earn a $0.025/GB/week calculated on an average basis. We will try our best to catch any cheaters, so at the very least you must avoid getting caught! Payouts will be made every Monday at ~11:00GMT.

## Builders and Bug Reporters

<p align="center"><img src="bug_earn.png"></p>

Unlike the Validators and Council Members, the bug bounty payments will be somewhat subjective. Long term, such decisions will be resolved by the platform, so in future testnets these payouts will at least partially be made by the Council.

We saw little interest in this role for Sparta, and only one community member reported `Issues` in the hopes of getting rewarded. Jsgenesis contracted this person to perform a more in depth investigation, but we still hope to generate more interest.

To report an `Issue` or make a `Pull request` go to the [node repo](https://github.com/Joystream/substrate-node-joystream), the [UI repo](https://github.com/Joystream/apps/tree/joystream), the [storage node repo](https://github.com/Joystream/storage-node-joystream) or the [runtime repo](https://github.com/Joystream/substrate-runtime-joystream). Based on the *importance and quality* of the issue/PR, the Jsgenesis team will decide on the rewards.

-   For issues, the reward will range up to $20
-   For a PR, the reward can range up to $100 (if you get in touch with a longer)

The quality of an issue can be measured from the level of details in general, like how to reproduce, pasted log outputs, etc. In terms of PRs, simply copying new features implemented on substrate will not be rewarded unless the PR includes changes that was required for compatibility on Joystream.

The contributor must include either their Joystream or monero address when submitting the issue/PR. If you choose the former, you must then make sure the add your monero address to the `memo` field of your `keys` as explained at the beginning of this post.

# Glossary
TODO

# Troubleshooting
If you had any issues setting it up, you may find your answer here!
