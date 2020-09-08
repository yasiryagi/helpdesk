

<!-- TOC START min:1 max:3 link:true asterisk:false update:true -->
- [Overview](#overview)
  - [Token Issuance](#token-issuance)
    - [Inflationary Forces](#inflationary-forces)
    - [Deflationary Forces](#deflationary-forces)
  - [Fiat Pool](#fiat-pool)
    - [Weekly Replinshment](#weekly-replinshment)
    - [KPI Payouts](#kpi-payouts)
  - [Tokenomics Examples](#tokenomics-examples)
    - [Example A](#example-a)
    - [Example B](#example-b)
  - [KPIs](#kpis)
    - [Structure](#structure)
    - [Example KPIs](#example-kpis)
<!-- TOC END -->


# Overview

At launch of a new testnet, the token (tJOY) issuance will be set/calculated, and an initial USD denominated fiat pool will be created to back it. The key concept is outlined below:

- All roles will be rewarded directly in tJOY
- tJOY will be backed by a USD fiat pool, and redeemable via monero at the convenience of participants
- This means that tJOY - only needed for staking and fees on the previous testnet - will now function more like the mainnet token
- The exchange rate will simply be amount of tokens issued divided by the size of the fiat pool
- When a user exchanges their tJOY for USD, the tokens will be burned immediately, thus **not affecting the exchange rate**
- New tokens rewards are minted to pay for the various roles on the network
- The fiat pool will be topped up weekly, without minting new tokens
- Jsgenesis will create KPIs, each assigned a USD value that will be added to the fiat pool (without minting new tokens), if the goals are achieved

The overall value flows can be summarized by the figure below:

<p align="center"><img src="../img/flows.png" width="1000"></p>

Examples of the system through the eyes of a user can be found [here](#tokenomics-examples). In order for community members to get a grasp of the tokenomics, a [spreadsheet](https://docs.google.com/spreadsheets/d/13Bf7VQ7-W4CEdTQ5LQQWWC7ef3qDU4qRKbnsYtgibGU/edit?usp=sharing) has been made.


## Token Issuance

The issuance of new tJOY will in practice be a cost incurred for all holders of tJOY, both passive and active. Because of this, it is in the interest of all holders to ensure the platform's resources are well managed, and keep tabs on the [Council Members'](../roles/council-members) spending. As this would lead to a situation where there was no rational reason whatsoever to reward any of the [roles](../roles/), Jsgenesis will try to mimic the market forces of supply and demand, by rewarding good behavior through [KPIs](#kpis).

### Inflationary Forces
There are many inflationary forces in the network, but not all of them will impact the exchange rate. The main ones are recurring rewards, and other costs associated with achieving KPIs.

Recurring rewards are currently paid to:
- [Validators](../roles/validators) for finding blocks, and keeping the network up and running
- [Council Members](../roles/council-members) for running the day to day operations, and managing spending and the budget through proposals
- [Storage Providers](../roles/storage-providers) for receiving, storing and serving content
- [Content Curators](../roles/content-curators) for curating said content, and the `Curator Lead` as the party responsible for managing them and the groups budget

Examples of other costs for achieving KPIs can be to [Content Creators](../roles/content-creators) for adding content, contributors rewarded through spending proposals, etc.

Tokens will also be minted for other purposes, such as faucets, competitions, [bounties](../roles/builders), etc. If the system is changed, some of these can impact the exchange rate in the future, but users will be warned in due time.

### Deflationary Forces
Tokens that are slashed, e.g. for not performing in their roles, malicious or "spammy" proposals or undesirable behavior, will simply be burned, and increasing the exchange rate for everyone else. Transaction fees and role application costs are also burned.

When an exchange takes place, the tokens are also burned, but this will have no impact on the exchange rate as the fiat pool will decrease simultaneously.

## Fiat Pool
The fiat pool, denominated in USD, will start at a set value, but will change continuously.

### Weekly Replinshment
Every week, an amount of USD will be added to the pool, without changing the tJOY issuance. The size of this will be determined the week before by Jsgenesis.

### KPI Payouts
Every week, a new set of KPIs (Key Performance Indicators) are announced. The period over which they are active and evaluated can wary, but will mostly follow a single `Council` term, starting at 1 week. As outlined earlier, the purpose of the the KPI system is to incentivize the community, via the `Council`, to act in a responsible manner. The KPIs will initially focus on ensuring the network functionality, and maintain a good experience for users. However, the set of KPIs will soon grow in order to promote other desirable actions, both long and short term. These KPIs aim to be precise, so as to avoid confusion and room for interpretation in both objective and grading. A single KPI will come with a set of conditions that determines what constitutes a success. Details and examples can be found [here](kpis).

KPIs are closely related to the new proposal system, so that the users can calculate the expected value of achieving individual KPIs, versus the anticipated (inflationary) cost of doing so.

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

A user has `1,500 tJOY`, and after weighing their options and calculating the rate, they decide to cash in `500 tJOY`. They go to [this page](https://www.joystream.org/testnet/), verify the exchange rate, and follow the instructions on how to cash in. After sumbitting and signing the transaction, a new pending exchange is displayed on the [page](https://www.joystream.org/testnet/):

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

The user assume that on average, the KPIs will return 150 USD this week, and decides to calculate their own expected return for each option, assuming:

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
