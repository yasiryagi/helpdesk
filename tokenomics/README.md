Overview
===
This page will explain the token economy ("tokenomics") of the Joystream testnets, and how this applies to the individual actors, and platform as a whole.

Table of Contents
---
<!-- TOC START min:1 max:3 link:true asterisk:false update:true -->
- [Tokenomics](#tokenomics)
  - [Token Issuance](#token-issuance)
    - [Inflationary Forces](#inflationary-forces)
    - [Deflationary Forces](#deflationary-forces)
  - [Fiat Pool](#fiat-pool)
    - [Weekly Replenishment](#weekly-replenishment)
- [KPIs and Bounties](#kpis-and-bounties)
  - [Council KPIs](#council-kpis)
  - [Community Bounties](#community-bounties)
    - [Types of Tasks](#types-of-tasks)
    - [Structure Example](#structure-example)
  - [Tokenomics Examples](#tokenomics-examples)
    - [Example A](#example-a)
    - [Example B](#example-b)
  - [KPIs](#kpis)
    - [Structure](#structure)
    - [Example KPIs](#example-kpis)
<!-- TOC END -->

# Tokenomics
At launch of a new testnet, the token (tJOY) issuance will be set/calculated, and an initial USD denominated fiat pool will be created to back it. The key concept is outlined below:

- All roles will be rewarded directly in tJOY
- tJOY will be backed by a USD fiat pool, and redeemable via monero at the convenience of participants
- This means that tJOY - only needed for staking and fees on the previous testnet - will now function more like the mainnet token
- The exchange rate will simply be amount of tokens issued divided by the size of the fiat pool
- When a user exchanges their tJOY for USD, the tokens will be burned immediately, thus **not affecting the exchange rate**
- New tokens rewards are minted to pay for the various roles on the network
- The fiat pool will be topped up weekly, without minting new tokens, effectively increasing the exchange rate (all else being equal)
- For every new Council term, Jsgenesis will create [Council KPIs](#council-kpis), each assigned a USD value. If the goals are achieved, Jsgenesis will reward the Council without affecting the exchange rate
- Jsgenesis will also create [Community Bounties](#community-bounties), similar to bounties, but managed by the Council. These are also assigned a USD value, and if achieved, Jsgenesis will (indirectly) reward the individual or group that achieved the goals.

Examples of the system through the eyes of a user can be found [here](#tokenomics-examples). In order for community members to get a grasp of the tokenomics, a [spreadsheet](https://docs.google.com/spreadsheets/d/13Bf7VQ7-W4CEdTQ5LQQWWC7ef3qDU4qRKbnsYtgibGU/edit?usp=sharing) has been made.


## Token Issuance

The issuance of new tJOY will in practice be a cost incurred for all holders of tJOY, both passive and active (with the exception of tokens minted for [KPIs](#kpis) achieved). Because of this, it is in the interest of all holders to ensure the platforms resources are well managed, and keep tabs on the [Council Members'](../roles/council-members) spending.

Jsgenesis will try to mimic the market forces of supply and demand, by rewarding good behavior through [KPIs](#kpis), to ensure participants have incentives to keep the platform and associated infrastructure running.

### Inflationary Forces
There are many inflationary forces in the network, but not all of them will impact the exchange rate. The main ones are recurring rewards, and other costs associated with achieving KPIs.

- Recurring rewards, currently paid to:
  - [Council Members](/roles/council-members) for running and managing the platform's day to day operations
  - [Storage Providers](/roles/storage-providers) for receiving, storing and serving content files
  - [Content Curators](/roles/content-curators) for monitoring and curating content and channels
- Automatic rewards, to [Validators](/roles/validators) and [Nominators](/roles/validators/README.md#nominating) are minted as rewards for finding blocks, and keeping the network up and running
- [Spending Proposals](/proposals/README.md#spending) that gets "approved" and executed

Tokens will also be minted for other purposes, such as faucets, competitions, [KPIs](#kpis), etc. Unlike the costs listed above however, these will under normal circumstances be offset an equivalent increase of [Fiat Pool](#fiat-pool).

If the system is changed, some of these can impact the exchange rate in the future, but users will be warned in due time.

### Deflationary Forces
Tokens are slashed and/or burned for a variety of reasons, thus reducing supply and increasing the exchange rate.

- Transactions (aka. extrinsics) fees, are burned
  - All extrinsics cost at least 1tJOY
  - Users may need/choose to set a higher fee for priority
  - Users, or Jsgenesis, may set high fees to intentionally burn tokens
- Creating [Proposals](/proposals) requires the creator to put up [Stake](/proposals/README.md#stake) (the size depends on the perceived "severity" of the Proposal type). Unless the Proposal gets [Approved](/proposals/README.md#approved), the full amount will not be returned to the "creator":
  - A [Slashed Proposal](/proposals/README.md#slashed) means slashing and burning the entire Stake of the "creator"
  - A [Cancelled Proposal](/proposals/README.md#slashed) means a fixed amount of the Stake is burned
  - A [Rejected Proposal](/proposals/README.md#rejected) means a smaller, fixed amount of the Stake is slashed and burned
  - An [Expired Proposal](/proposals/README.md#expired) counts as "rejected"
- Slashing Staked [workers](/roles). The mechanism of these slashes depends on the role, but in all cases, the tokens are burned.
  - [Validators](/roles/validators) (and their [Nominators](/roles/validators/README.md#nominating)) gets slashed automatically by the chain if they go offline, or responds too slowly, without first stopping.
  - [Storage Providers](/roles/storage-providers) can get their stakes slashed by their Lead.
  - The [Storage Lead](/roles/storage-lead) can get their stakes slashed by the Council through a [Proposal](/proposals/README.md#slash-working-group-leader-stake).
  - [Content Curators](/roles/storage-providers) can get their stakes slashed by their Lead.


When an exchange takes place, the tokens are also burned, but this will have no impact on the exchange rate as the fiat pool will decrease proportionally.

## Fiat Pool
The fiat pool, denominated in USD, will start at a set value, but will change continuously.

### Weekly Replenishment
Every week, an amount of USD will be added to the pool, without changing the tJOY issuance. The size of this will be determined the week before.

# KPIs and Bounties
Jsgenesis will regularly release new Key Performance Indicators ("KPIs") and Bounties as a way to incentivize the community and its participants to perform certain actions, maintain network functionality, produce reports, assets, code or other deliverables, etc.

The KPI scheme has evolved over time, and further changes in the future should be expected.

Currently, we separate these as two different types, [Council KPIs](#council-kpis) and [Community Bounties](#community-bounties), each with a distinct structure, frequency, scope, and reward mechanism.

## Council KPIs
For each new Council elected, a fresh set of Council KPIs are published by Jsgenesis. These KPIs will mainly serve to incentivize the Council Members to manage the platform, pay attention to the Tokenomics, monitor the network and respond to proposals and other requests.

Each individual KPI in the set will:
- contain some defined tasks or conditions for the Council to address or deliver
- have a specific maximum reward assigned to it (in USD)
- (usually) last during the entire Term of the Council
- be graded (individually) a few days after the end of said Term

The sum of the rewards earned will be given directly to the individual Council Members and those that voted for them, without affecting the overall exchange rate, by topping up the [Fiat Pool](#fiat-pool) and minting new tokens.

As the Council KPIs only apply to prospective Council Members, the full details can be found under their role section [here](/roles/council-members/README.md#council-kpis).

## Community Bounties
The Community Bounties are meant to replace the "old" Bounty system previously used by Jsgenesis. In discussions with the community, these have been referred to as "Community KPIs", but we've chosen to use the term Community Bounties to properly distinguish them.

Jsgenesis will publish these in a format similar to that of a [Council KPI](#council-kpis), but with some key differences:
- They will not be published at the same regular and predictable intervals
- They will not necessarily have deadlines
- Jsgenesis will rarely get involved in managing or assigning them

The last part is key, as the Council will act as Project Managers, and serve as a bridge between Jsgenesis and the individual or group working on them.

### Types of Tasks
The tasks associated with these Community Bounties will ideally try to solve some problem either for the community or Jsgenesis, but in some cases, their main purpose will be to create some fun and/or attract new members to the community.

Over time, the tasks should allow people with different skillsets and interests to participate. Most challenges will be easier if you are somewhat technical or creative, but in other situations it will simply require putting in some time and effort:
- Coding
  - Telegram bots
  - Scripts
  - Enhance the UI
  - Improve infrastructure
- Writing
  - Documentation
  - Marketing material
  - Explainers
  - Translations
- Creative
  - Videos
  - Artwork
  - Gifs
  - Memes
- Research, testing and sourcing
  - Source/upload freely licensed media
  - Testing and benchmarking tools
  - Research interesting projects
  - Tokenomics research
- Reviewing
  - All of the above

### Structure Example
An example of structure for a Community Bounty, as published by Jsgenesis.

- `id:` - A unique identifier (eg. `5`)
- `Title:` - The title of the Community Bounty
- `Reward:` - The maximum reward paid, assuming all `Success Events` are delivered and graded complete
- `Description:` - A description of the problem solved if all `Success Events` are complete
- `Success Events:`
  - `1.` - A precise definition of subtask `1.`
  - `2.` - A precise definition of subtask `2.`
  - ...
  - `n.` - A precise definition of subtask `n.`
- `Annihilation:` - A precise definition of something that, if it occurs, would result in the entire `Reward` getting lost, even in the event all the `Success Events` are fully completed
- `Conditions` - Any conditions that must be met, in addition to the `Success Events` themselves
- `Review period:` - A period of time for which Jsgenesis review and grade the deliverable, after it being "officially" submitted to them.

In addition to these, there are some other information that may or may not be included:
- `Rules:` - Although it will usually be up to the Council the define the [Rules](#rules), Jsgenesis may in some circumstances choose to set them themselves. Examples of this could be:
  - Specifying the [Format](#format)
  - Specifying the [Worfklow](#workflow)
  - Assigning, or "reserving" the rights for one or more community members to work on the KPI for some period of time
- `Deadline:` - In some cases, a Community Bounty could have a deadline for the when the deliverable must be made. Although Jsgenesis reserves the right not to grade or reward any submissions made after the deadline, it doesn't necessarily mean any work will be in vain:
  - Late submissions may still be accepted, with full or partial rewards
    - especially if the delay can be attributed to the Council
  - It may be slightly modified, and re-published under a different `ID`
  - It may renewed with a new deadline under a different `ID`

## Tokenomics Examples

A rational user will continuously consider their options, and try to maximize their profits based on a variety of inputs such as:
- tJOY holdings
- the tJOY costs and returns associated with the available roles
- the USD costs of hardware, electricity and their time
- opportunity costs
- exchange rate changes from
  - tJOY issuance change
    - recurring role rewards
    - fees
    - slashes and tJOY burns
    - `Council` spending
  - fiat pool inflows and outflows
    - replenishment of fiat pool
    - KPIs achieved leading to an increased fiat pool

### Example A

Suppose there is `10,000 tJOY` in circulation, and `USD 1,000` represents the initial fiat pool. This means that the exchange rate is `0.1 tJOY/USD`.

A user has `1,500 tJOY`, and after weighing their options and calculating the rate, they decide to cash in `500 tJOY`. They go to `<page>`, verify the exchange rate, and follow the instructions on how to cash in. After sumbitting and signing the transaction, a new pending exchange is displayed on the `<page>`:

The Joystream transaction locks in the USD value of the exchange. At regular intervals, all pending transactions will be paid out in Monero, with the prevailing XMR/USD rate at the time of payout.

All else being equal, this will reduce the tJOY issuance from `10,000 to 9,500`, and the USD fiat pool from `1,000 to 950`. The exchange rate will therefore not be changed, and there is no incentive to start a run on the bank.

Suppose further the user, after considering their skillset and rate of return, decided that the best option for them was to stake their remaining `1,000 tJOY` for the role of `Storage Provider`, and assume further that the payout for this role was `2 tJOY per 3600 blocks` (6 hours).

Assuming again all else being equal, ie. the only token inflation on the network was the payout for *this* user, the payout stays the same, and our user performs their job satisfactory, they can expect to cash in `560 tJOY` every week. After the first week, the total issuance is now `10,060 tJOY`, and the fiat pool is now `$950 USD`. The user cash out their `560 tJOY`, and with a rate of `~0.094 tJOY/USD`, will receive `$52.9 USD` for their job.

If the user had instead chosen to exchange all their `1,500tJOY` in the first place, the would have locked in a value of `$150 USD`. Their actual choice however, resulted in having cashed in a total of `$102.9 USD`, with another `1,000` tJOY worth `$94.4 USD` at the prevailing rates, for a total of `$197.3 USD`.

In practice, the system will be far more complex than what is outlined above. For more precise calculations, please use [the spreadsheet](https://docs.google.com/spreadsheets/d/13Bf7VQ7-W4CEdTQ5LQQWWC7ef3qDU4qRKbnsYtgibGU/edit?usp=sharing).

### Example B

After two weeks, suppose there is now `100,000 tJOY` in circulation, and `USD $3,000` in the fiat pool. This means that the exchange rate is `0.03 tJOY/USD`. However, the next replenishment of the fiat pool is set to `$200 USD`. Additionally, two `KPIs` are live, with a potential payout of `$100 USD` for each. These are both scheduled in exactly 7 days. The users have `4000 tJOY`.

Currently, the status of the roles and rewards are:

**Storage Providers**

-   5 slots filled, 1 slot open
-   Stake is `3500 tJOY`, cost of entering is `200 tJOY`
-   Rewards are `4 tJOY/600` blocks per slot
-   The users finds that running a Storage Provider costs them `$4 USD per week`

**Validators**

-   All 10 slots filled
-   Lowest current stake in the pool is `2500 tJOY`
-   Rewards are `575 tJOY/100800 blocks` in total
-   The users finds that running a Storage Provider costs them `$1 USD per week`

**Council Members:**

-   New election is running, with 6 slots
-   It is assumed `4000 tJOY` will be enough to earn a slot
-   Rewards are `300 tJOY/100800` blocks per CM

**Curator:**

-   The lead and 2 slots are filled, 1 slot is open for application
-   Stake required is `500 tJOY`
-   Rewards are `5 tJOY/3600` blocks per slot for curators
-   The lead earns `12tJOY/3600` blocks

The user assume that on average, the KPIs will return 150 USD this week, and decides to calculate their EV for each option, assuming:

-   all slots fill up
-   no tx fees included
-   no slashing

At first glance it appears as if then `Storage Provider` is the most lucrative role. However, with a second look, one can see that you can instead try to become both a `Curator` and a `Validator`, as the up front stake and cost for the former totals `3,700 tJOY`, whereas the latter two totals just `3,000 tJOY`.

There are still a lot of factors that should be considered, such as time spent, transaction fees, risk of not getting or keeping your role, tokens being locked in for staking, reputation and knowledge building, etc. When all these are weighed against each other, one must assume that the various roles will be ranked differently depending on ones skills, costs, risk tolerance and other preferences.


## KPIs

### Structure
All KPIs will be defined in a manner similar to the one below:

**Purpose**:
In order to have a reliable network, a KPI focusing on block production and validator stability is required. Timestamp of each block can be found using the block explorer.

- Text:
  - `average block time < 6.02s, maintain 10 validators`
- Measurement period:
  - `7 days`
- Active from:
  - Time and date: `dd:mm:yy - hh:mm GMT`
  - Blocknumber: `n`
- Success event 1:
  - `<timestamp_block n+100465> must be smaller than <timestamp_block n>+604,800,000`
- Success event 2:
  - `maintain at least 10 validators throughout the Measurement period`
- Annihilation:
  - `Block finalization lags more than 600 blocks behind head at any point throughout the Measurement period`  
- Max reward:
  - `$100`
- Grading:
  - `dd:mm:yy - hh:mm GMT *`

`*` + 8 days

In this case, the specifics of the success events may be a little hard to read. This is to ensure objective interpretation of the grading.

Unless noted otherwise, each success event that is achieved triggers an increase of the fiat pool corresponding to the "Max reward" divided by the number of "Success events". If the "Annihilation" event occurs, there will be no rewards regardless of the outcomes of the "Success events", and there may even be punishment (leading to a decrease in the fiat pool) if specified.

### Example KPIs

High level examples of KPIs which may be introduced include:

##### Network Stability

Maintain stability in block production and networking performance, as measured by:

-   block production
-   number of validators
-   network peers
-   etc.

##### Storage Benchmark

Keep the storage system robust, as measured by:

-   upload/download speed
-   high uptime
-   few lost connections
-   etc.

##### Proposal Clearance

A responsive and effective council, as measured by:

-   proposals made being dealt with in an appropriate, swift and effective manner
-   ensure the maximum number of open proposals (5) will not stop new proposals being made
-   etc.

##### Content Publication

In order for the platform to gain traction, it needs to get some amount of high quality content available, as measured by:

-   engagement with content by users
-   views
-   length of videos
-   etc.

##### Content Curation

-   rapid response to content which violates our terms of use
-   regularly setting "featured" content in order to highlight some of the best uploads to the platform
-   etc.

##### Attract, Fund and Complete Projects

-   in-depth discussions on the merits of projects and initiatives proposed by community members
-   funding proportional to requirements of project
-   etc.
