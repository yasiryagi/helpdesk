<p align="center"><img src="img/council_member.png"></p>

<div align="center">
  <h4>This is a step-by-step guide through Joystream council election cycle, allowing users to take part in the governance system for the
  <a href="https://testnet.joystream.org/pioneer">Joystream Testnet</a>.<h4>
</div>



# Table of Contents

- [Overview](#overview)
- [Instructions](#instructions)
    - [Get Started](#get-started)
- [Election Cycle](#election-cycle)
    - [Announcement](#announcement)
    - [Voting](#voting)
    - [Reveal](#reveal)
    - [Term](#term)
- [Governance](#governance)
    - [Proposals](#proposals)
    - [Voting on Proposals](#voting-on-proposals)
- [Troubleshooting](#troubleshooting)
    - [Session Key](#session-key)


# Overview

This page contains a detailed guide about how the governance system works on the current Joystream testnet, and how you can participate.

# Instructions
Unlike most of the other current and future roles on the Joystream Platform, becoming a `Council Member` or voting on proposals requires no extra software. Everything can be done in the browser, by going [here](http://testnet.joystream.org).

**Note**
After introducing `Memberships` to the platform, we found it to be confusing to have a concept of both `Accounts` and `Memberships`. We are in the process of renaming the `Accounts` to the `Keys`, but there are still traces of `Accounts` showing up.

## Get Started
If you want to get elected as a `Council Member` or vote on the platform, you need to be a `Member`. Instructions for this can be found [here](https://github.com/JoyStream/helpdesk/#get-started).

# Election Cycle
The election cycle consists four stages.
1. `Announcement` - lasts 43200 blocks (~72h)
2. `Voting`       - lasts 14400 blocks (~24h)
3. `Reveal`       - lasts 14400 blocks (~24h)
4. `Term`         - lasts 201600 blocks (~14days)

## Announcement
During the `Announcement` stage, anyone that is a `Member`, and holds at least unstaked 1000 Joy (ie. if you use your `validator` `stash` key, you need a `balance` > `bonded` + 1000 Joy) tokens can announce their candidacy to become a `Council Member`.

Select `Council` in the sidebar, and click the `Applicants` tab. Set the amount of tokens you want to, stake, and confirm.
If you want to put more stake behind your candidacy later, you can top up at any point during the stage. After sending the transaction, you should appear under "Applicants". The max number of Applicants is `25`. When the 25th candidate applies, the one with the lowest amount staked will be pushed off the list, and get their stake returned. In total, `12` Council Members must be elected. If there are less than 12 applicants, the `Announcement` stage will be restarted.

## Voting
As soon as the `Announcement` stage closes, you can begin voting for applicants. As with everything else, you need to stake in order to do so. Joystream is currently working under the "One Token - One Vote" principal. Go to the `Votes` tab, set your staking amount, select your candidate and generate a `Random salt`. This will be needed to reveal and actually "broadcast" your vote. You can vote more than once, for your self, and for more than one applicant. All the data will be stored in your browser, so as long as you are using the same machine/browser/cookies, you don't need to save anything.

## Reveal
As soon as the `Voting` stage closes, the Revealing stage begins. This is when you can reveal your vote. Go to the `Reveal a vote` tab, to actually broadcast your vote. Votes that are not revealed in time will not get counted in the election.

## Term
As soon as the `Reveal` stage closes, the 12 candidates with the highest total backing, ie. their own stake + voter stake, will become `Council Members`. Their term will run for 14 days, after which a new `Council` will been elected.

Note that the next `Announcement` stage will start exactly 201600 blocks (14 days) after the previous.

# Governance
TODO

## Proposals
TODO

## Voting on Proposals
TODO


---

# Troubleshooting
If you had any issues setting up this role, you may find your answer here!
