<p align="center"><img src="img/council-members_new.svg"></p>

<div align="center">
  <h4>This is a step-by-step guide through Joystream council election cycle, allowing users to take part in the governance system for the
  <a href="https://testnet.joystream.org/">Joystream Testnet</a>.<h4>
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


# Overview

This page contains a detailed guide about how the governance system works on the current Joystream testnet, and how you can participate.

# Instructions
Unlike most of the other current and future roles on the Joystream Platform, becoming a `Council Member` or voting on proposals requires no extra software. Everything can be done in the browser, by going [here](https://testnet.joystream.org).

**Note**
After introducing `Memberships` to the platform, we found it to be confusing to have a concept of both `Accounts` and `Memberships`. We are in the process of renaming the `Accounts` to the `Keys`, but there are still traces of `Accounts` showing up.

## Get Started
If you want to get elected as a `Council Member` or vote on the platform, you need to be a `Member`. Instructions for this can be found [here](https://github.com/JoyStream/helpdesk/#get-started).

# Election Cycle
The election cycle consists four stages. Currently, the length of each are:
1. `Announcement` - lasts 14,400 blocks (~24h)
2. `Voting`       - lasts 14,400 blocks (~24h)
3. `Reveal`       - lasts 14,400 blocks (~24h)
4. `Term`         - lasts 57,600 blocks (~4days)

Note that these parameters can be changed through the proposal system.

## Announcement
During the `Announcement` stage, anyone that is a `Member` and holds the required amount of tokens to stake, can announce their candidacy to become a `Council Member`.

Select `Council` in the sidebar, and click the `Applicants` tab. Set the amount of tokens you want to, stake, and confirm.
If you want to put more stake behind your candidacy later, you can top up at any point during the stage. After sending the transaction, you should appear under "Applicants". The max number of Applicants is `25`. When the 25th candidate applies, the one with the lowest amount staked will be pushed off the list, and get their stake returned. In total, `12` Council Members must be elected. If there are less than 12 applicants, the `Announcement` stage will be restarted.

## Voting
As soon as the `Announcement` stage closes, you can begin voting for applicants. As with everything else, you need to stake in order to do so. Joystream is currently working under the "One Token - One Vote" principal. Go to the `Votes` tab, set your staking amount, select your candidate and generate a `Random salt`. This will be needed to reveal and actually "broadcast" your vote. You can vote more than once, for your self, and for more than one applicant. All the data will be stored in your browser, so as long as you are using the same machine/browser/cookies, you don't need to save anything.

## Reveal
As soon as the `Voting` stage closes, the Revealing stage begins. This is when you can reveal your vote. Go to the `Reveal a vote` tab, to actually broadcast your vote. Votes that are not revealed in time will not get counted in the election.

## Term
As soon as the `Reveal` stage closes, the 12 candidates with the highest total backing, ie. their own stake + voter stake, will become `Council Members`. Their term will run for 14 days, after which a new `Council` will been elected.

Note that the next `Announcement` stage will start exactly 57,600 blocks (~4 days) after the previous.

# Governance
Constantinople introduced a number of important changes to the governance structure of the platform. The most important of these was the enhancement of the platform's proposal system. You can read descriptions of each of the proposal types on the helpdesk article [here](../../proposals/README.md).

Most of the proposals are meant to allow the Council to allocate the platforms resources as efficiently as possible. In order to do so, a [tokenomics spreadsheet](https://docs.google.com/spreadsheets/d/13Bf7VQ7-W4CEdTQ5LQQWWC7ef3qDU4qRKbnsYtgibGU/edit?usp=sharing) has been made to assist in the decision making.

## Proposals

As a member (council member or not) you are able to make proposals to be voted on by the council.

The types of proposals available include:
- Text/signal Proposal
- Funding Requests
- Evict Storage Provider
- Set Storage Role Parameters
- Set Max Validator Count
- Set Content Curator Lead
- Set Content Working Group Mint Capacity
- Set Election Parameters
- Runtime Upgrade

To make a proposal:<br><br>
 (1) Click the Proposals tab in the Pioneer sidebar (`/proposals`). <br>
 (2) This will provide a view of all of the currently active (as well as past) proposals.<br>
 (3) If there are fewer than five `active` proposals, you can click the `New Proposal` button at the top of the page.<br>
 (4) You will be given a list of proposal types; select the one which is required for your proposal.<br>
 (5) Make a note of the staking requirements, ensuring your account balance is sufficient to create the proposal.<br>
 (6) Also note the other variables on the page, paying particular attention to the quorum and threshold for votes.<br>
 (7) Click `Create Proposal` and fill in the required fields, `Title`, `Rationale` and `Description`.<br>
 (8) When you are ready, click `Submit Proposal` and sign the transaction.<br>
 (9) If everything has worked correctly, your proposal should now be `active` on the proposals page.<br>

## Voting on Proposals
While any member can make a proposal, only council members can vote!

The voting process is rather simple. The first step is to navigate to the proposals (`/proposals`) page and view the currently active proposals. Select the proposal you would like to vote on and simply click the button corresponding to your decision (based on discussion with other council members) on the merits of the proposal.

You can choose:

- `Approve` - approving the proposed action
- `Reject` - reject the proposed action
- `Slash` - reject the proposed action, and slash the stake of the proposer
- `Abstain` - abstain from voting

More information on how council votes are processed can be read [here](../../proposals/README.md).

---

# Troubleshooting
If you had any issues setting up this role, you may find your answer here!
