<p align="center"><img src="img/council-members_new.svg"></p>

<div align="center">
  <h4>This is a step-by-step guide through Joystream council election cycle, allowing users to take part in the governance system for the
  <a href="https://testnet.joystream.org/">Joystream Testnet</a>.<h4>
</div>



Table of Contents
---
<!-- TOC START min:1 max:4 link:true asterisk:false update:true -->
- [Overview](#overview)
  - [Why Become a Council Member](#why-become-a-council-member)
    - [Incentives](#incentives)
  - [Get Started](#get-started)
- [Council Elections](#council-elections)
  - [Parameters](#parameters)
  - [Council Election Cycle](#council-election-cycle)
    - [Announcing](#announcing)
      - [How to Announce](#how-to-announce)
      - [End of Announcing](#end-of-announcing)
    - [Voting](#voting)
      - [How to Vote](#how-to-vote)
    - [Revealing](#revealing)
      - [How to Reveal](#how-to-reveal)
      - [End of Revealing](#end-of-revealing)
    - [Term](#term)
- [How to Get Elected](#how-to-get-elected)
  - [Announce on the Forum](#announce-on-the-forum)
- [Elected Council Members](#elected-council-members)
  - [Tasks](#tasks)
- [Governance](#governance)
  - [Proposals](#proposals)
  - [Voting on Proposals](#voting-on-proposals)
  - [Recurring Rewards](#recurring-rewards)
- [Troubleshooting](#troubleshooting)
<!-- TOC END -->

# Overview
This page contains a detailed guide about how the governance system works on the current Joystream testnet, and how you can participate. This page will focus primarily on those wanting to become, or already is, a Council Member ("CM") themselves.

However, if you are interested in the Joystream project and/or want to become a part of the community now or in the future, there is lots of information in here that will help you understand the project and the governance of the platform.

## Why Become a Council Member
As the governance of the platform is the most important part of the platform on mainnet, we are relying on testnets to train and build a strong group of community members that can perform the tasks required of them.

A "good" Council needs CMs that all have a strong understanding of both the platforms token economics (["tokenomics"](/tokenomics)) and each of the individual working groups and the roles each play in making the platform function. Additionally the composition of each Council should ensure that the group has expertise in every domain, and some CMs with low-level technical understanding.

### Incentives
During the Constantinople testnet, Jsgenesis realized we need to put a lot more effort in to attracting, training and retaining these people. CMs will now be a well paid role on the platform, and in the new KPI scheme, also see direct rewards for doing a good job.

Consequently, Jsgenesis will take an active role in the elections. More information on how to apply, and increase your chances of getting elected can be found [here](#council-election-cycle).

The responsibilities of the CMs can be found [here]()

## Get Started
Unlike most of the other current and future roles on the Joystream Platform, most of the information and actions required by participants in the governance system is available in our UI - named [Pioneer](https://testnet.joystream.org). For elected CMs, some familiarity with [GitHub](https://github.com/Joystream/community-repo/) is required, and at any time, a subset of the CMs must be able to use git, and basic coding review skills. As the project grows, new skills and more advanced skills may be required.

If you want to get elected as a CM or vote on the platform, you need to be a Member. Instructions for this can be found [here](https://github.com/JoyStream/helpdesk/#get-started).

**Note**
After introducing `Memberships` to the platform, we found it to be confusing to have a concept of both `Accounts` and `Memberships`. We are in the process of renaming the `Accounts` to the `Keys`, but there are still traces of `Accounts` showing up.

# Council Elections
A new Council of CMs are elected at regular intervals. The election is decided by selecting the applicants that has the highest total "stake" backing them. Staking here means "locking" up your tokens, making them unusable for transfers or staking in other ways, thus forcing participants to put "skin in the game". The total stake is the sum of the applicants own stake required to put themselves up for election, and the stake of any voters voting on them.

The terms of a these election are defined by some parameters. Although these can be changed either by the [Proposal system](/proposals), or by `sudo`, the parameter changes should be both infrequent and small.

The current set of parameters, as well as the status and stage of the [Council/election cycle](#council-election-cycle) can be found [here](https://testnet.joystream.org/#/council).

## Parameters
In addition to the length (and definitions) of the election stages described below, the most important parameters are:
- `Council Size`
  - This is amount of CMs that will be elected. This is an absolute number, meaning neither less nor more CMs can be elected.
- `Candidacy Limit`
  - This is the maximum number of applicants, that have [announced](#announcing) their candidacy, that are eligible to vote for in the [voting](#voting) stage. This number will always be bigger than the `Council Size`
- `Minimum Council Stake`
- `Minimum Voting Stake`
  - The minimum stake that applicants and voters are required to put up, for running themselves and voting (for others or themselves) respectively.

The full details of the election cycle will also expand upon these parameters.

## Council Election Cycle
The election cycle consists four stages. Currently, the length of each are:
1. [Announcing](#announcing) - lasts 28,800 blocks (~48h)
2. [Voting](#voting) - lasts 14,400 blocks (~24h)
3. [Revealing](#reveal) - lasts 14,400 blocks (~24h)
4. [Term](#term) - lasts 144,400 blocks (~10days)

### Announcing
During the entire `Announcing` stage, anyone that is a Member and can stake tokens than the `Minimum Council Stake`, can apply to become a CM.

#### How to Announce
With your membership key:
- select "Council" in the sidebar
- click the "Applicants" tab
- set the amount of tokens you want to stake
- click "Apply to council" and confirm

**Important Notes**
- you can add to your stake later, by following the same approach as above
- this can be particularly useful because of the `Candidacy Limit`, which limits the number of applicants that goes through to the [voting](#voting)
- active CMs can "re-use" their own stake from the last election cycle
- this means that you if you staked 10k JOY on yourself, and have 5k "free" balance, you can stake 15k

#### End of Announcing
At the end of the stage, there are three outcomes depending on:
- the number of `Applicants`
- the `Council Size`
- the `Candidacy Limit`

##### Scenario A:
If: `Council Size <= Applicants <= Candidacy Limit`. Ie. the number of applicants is between the `Council Size` and `Candidacy Limit`.

The `Announcing` stage ends, with all the applicants proceeding to the `Voting` stage.

##### Scenario B:
If: `Candidacy Limit < Applicants`. Ie. the number of applicants is greater than the `Candidacy Limit`.

All of the applicants is sorted and ranked by their stake. The `Announcing` stage ends, and only those with a rank better than, or equal to, the `Candidacy Limit` will proceed to the `Voting` stage.

The applicants that did not make it to the `Voting` stage gets their stake back right away.

##### Scenario B:
If: `Applicants < Council Size`. Ie. the number of applicants is smaller than the `Council Size`.

There are not enough applicants to fill the Council slots, and a new `Announcing` stage begins.

### Voting
As soon as the `Announcing` stage closes, anyone that is a Member and can stake more than the `Minimum Voting Stake`, can vote for any of the applicants for the entire duration of this stage.

#### How to Vote
With your membership key:
- select "Council" in the sidebar
- click the "Applicants" tab
- find your preferred Applicant, and click the "Vote" button
- set the amount of tokens you want to stake
- click "Submit my vote" and confirm

You can vote as many times as you like, for any Applicant (including yourself).

**Important Notes**
When you submit a Vote, a `Random salt` will be generated for you, and only a `Hash` of the vote will be broadcast and stored on chain. This means:
- no one will know who you voted for, or how much you staked
- unless this hash is "revealed" during the [Revealing](#revealing) stage, the vote will not count
- unless you save the `Random salt` somewhere, you will only be able to "reveal" your vote if:
  - you use the same key, computer and browser - without clearing local storage.
  - if you save the `Random salt`, you only need the key
- if you voted for one or more active CMs, you can "re-use" your voting stake from the last election cycle
- this means that you if you staked 10k JOY voting an one or more current CMs, and have 5k "free" balance, you can stake 15k for voting

### Revealing
As soon as the `Voting` stage closes, the `Revealing` stage begins. As stated before, only when a vote is "revealed" will it become public, and count.

#### How to Reveal
**xxx**

#### End of Revealing
At the end of the `Revealing` stage, the applicants are sorted and ranked by their total stake, ie. the sum of the stake(s) they bonded during the `Announcing` stage, and the sum of all **"revealed"** votes.

The applicants ranked within the number equal to the `Council Size`, will become CMs.

The applicants that did not get elected will get their stake back immediately. Same goes for those that voted for them, and those that did not reveal their vote.

### Term
On the block that marks the end of the `Revealing` stage, the elected CMs will automatically be given their new privileges. Namely, the right to vote on [Proposals](#proposals), and be assigned a [recurring reward](#recurring-rewards).

The CMs stakes will be not only be held until the `Term Duration` expires, but until a new Council is elected. The same applies to those that voted for them.

The [recurring rewards](#recurring-rewards) however, will only be paid during the `Term Duration`.

Note that the next `Announcing` stage will start at the exact block the `Term Duration` expires.

# How to Get Elected
Unless you have sufficient tokens to get (re-)elected without any extra voters, you are unlikely to get any votes without making an effort to do so. As Jsgenesis has a large part of the voting power, and community members are unlikely to vote for random people without a proven track record, there are some steps you can take to greatly increase your probability of getting votes:

## Announce on the Forum
Before a new `Announcing` stage begins, a new thread will be made on the on-chain [forum](https://testnet.joystream.org/#/forum). Regardless of whether you are a new to the project, been following it from a distance, an active member or an experienced CM, make a post or two explaining why you deserve getting voted in. Some suggestions of what to include are:
- All
  - A little bit about yourself (no need to dox yourself)
  - Handles on other platforms, such as github, tlgrm, keybase (again, no need to dox yourself)
  - Why you want to be a CM
  - Interest in the project
  - Interest in the space (blockchain, media, both)
  - Any useful skills or assets, such as:
    - technical/coding
    - sysadmin
    - economics
    - excel, numbers
    - math
    - media production, curation
    - free time and grit
    - etc.
- Newcomers
  - A little (more?) about yourself (still, no need to dox yourself)
  - What brought you in to the project
  - Goals
  - Participation in other projects of any kind
  - What you have done to prepare
  - Any questions about the role
  - etc.
- Long time community members
  - General Joystream "record", such as
    - Roles you had
    - Contributions (bounties, KPIs, etc.)
    - Proposals made
    - Participation on discussions on telegram, forum, github, proposals
  - CM "record"
    - Election history
    - Proposal voting history
    - Council KPIs performed

As you are likely to get some follow up questions, you should check in at regular intervals to answer and/or assist.

# Elected Council Members
The CMs have a variety of [tasks](#tasks). Some are pro-active, others are re-active. Some are recurring and predictable, others will require on the spot problem solving.

To some extent, the same applies to their rewards. They receive a fixed (in tJOY) [recurring reward](#recurring-rewards), which will be handled automatically by the chain. The other relies on their ability to solve the tasks and challenges they face through the [Council KPIs](#council-kpis).

A CM that is slacking off, or for other reasons unable or unwilling to perform their tasks will still receive their rewards for the term, but are unlikely to get re-elected in the near future.

## Tasks
The CMs will be evaluated based on how well they perform these tasks. They are free to organize themselves in order to tackle specific tasks, or monitor different parts of the platform. If so, they should disclose it publicly, so that voters can evaluate their performance.

- Pay attention to the forum and telegram, to assist and answer questions when appropriate
- Pay attention to incoming Proposals, discuss and make informed votes
- Check in on the performance on the working group and, if necessary, take action
- Monitor the platforms infrastructure, and report downtime
  - endpoint nodes (does blocks come in on hosted pioneer)
  - pioneer (any sites not responding as expected)
  - storage system (is media served)
  - [status-server](https://status.joystream.org/status)
- Monitor the platforms spending and resource allocation
- Perform and/or delegate the required tasks related to the [Council KPIs](#council-kpis)
- Perform the required tasks related to the [Community KPIs](#community-kpis)

# Governance
Constantinople introduced a number of important changes to the governance structure of the platform. The most important of these was the enhancement of the platform's proposal system. You can read descriptions of each of the proposal types on the helpdesk article [here](../../proposals/README.md).

Most of the proposals are meant to allow the Council to allocate the platforms resources as efficiently as possible. In order to do so, a [tokenomics spreadsheet](https://docs.google.com/spreadsheets/d/13Bf7VQ7-W4CEdTQ5LQQWWC7ef3qDU4qRKbnsYtgibGU/edit?usp=sharing) has been made to assist in the decision making.

## Proposals

As a member (council member or not) you are able to make proposals to be voted on by the council.

The types of proposals available include:
- Text/signal Proposal
- Spending Proposal
- Set Max Validator Count
- Set Content Curator Lead
- Set Content Working Group Mint Capacity
- Set Election Parameters
- Runtime Upgrade
- Add Working Group Leader Opening
- Set Working Group Mint Capacity
- Begin Review Working Group Leader Application
- Fill Working Group Leader Opening
- Slash Working Group Leader Stake
- Decrease Working Group Leader Stake
- Set Working Group Leader Reward
- Terminate Working Group Leader Role

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

## Recurring Rewards
**xxx**

---

# Troubleshooting
If you had any issues setting up this role, you may find your answer here!
