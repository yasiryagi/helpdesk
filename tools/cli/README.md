Table of Content
---
<!-- TOC START min:1 max:4 link:true asterisk:false update:true -->
- [Overview](#overview)
  - [Install](#install)
    - [Install with NPM](#install-with-npm)
    - [Build Yourself](#build-yourself)
    - [Both](#both)
  - [Getting Started](#getting-started)
  - [account](#account)
    - [account:choose](#accountchoose)
    - [account:create](#accountcreate)
    - [account:current](#accountcurrent)
    - [account:export](#accountexport)
    - [account:forget](#accountforget)
    - [account:import](#accountimport)
    - [account:transferTokens](#accounttransfertokens)
  - [api](#api)
    - [api:getUri](#apigeturi)
    - [api:inspect](#apiinspect)
    - [api:setUri](#apiseturi)
  - [council](#council)
    - [council:info](#councilinfo)
  - [help](#help)
  - [working-groups](#working-groups)
    - [working-groups:application](#working-groupsapplication)
    - [working-groups:createOpening](#working-groupscreateopening)
    - [working-groups:decreaseWorkerStake](#working-groupsdecreaseworkerstake)
    - [working-groups:evictWorker](#working-groupsevictworker)
    - [working-groups:fillOpening](#working-groupsfillopening)
    - [working-groups:increaseStake](#working-groupsincreasestake)
    - [working-groups:leaveRole](#working-groupsleaverole)
    - [working-groups:opening](#working-groupsopening)
    - [working-groups:openings](#working-groupsopenings)
    - [working-groups:overview](#working-groupsoverview)
    - [working-groups:slashWorker](#working-groupsslashworker)
    - [working-groups:startAcceptingApplications](#working-groupsstartacceptingapplications)
    - [working-groups:startReviewPeriod](#working-groupsstartreviewperiod)
    - [working-groups:terminateApplication](#working-groupsterminateapplication)
    - [working-groups:updateRewardAccount](#working-groupsupdaterewardaccount)
    - [working-groups:updateRoleAccount](#working-groupsupdateroleaccount)
    - [working-groups:updateWorkerReward](#working-groupsupdateworkerreward)
<!-- TOC END -->

# Overview

The CLI tool is currently quite rough, and its main function is to let the Storage Provider Lead perform their duties in a more user-friendly manner than the [extrinsics](https://testnet.joystream.org/#/extrinsics) tab. For this reason, the guide will focus primarily on this side of the tool's functionality.


## Install

There are two ways of installing the CLI.

If you are, or planning to, run a `storage-node`, build your own node/runtime, or host your own instance of `Pioneer` the CLI is bundled in the [joystream-monorepo](https://github.com/Joystream/joystream). In that case, go [here](#build-yourself). If not, you may have an easier time using the [NPM-package](#install-with-npm).


### Install with NPM
If you have [NPM](https://www.npmjs.com/get-npm) installed:

```
$ npm install -g @joystream/cli
```

### Build Yourself

To get the CLI up and running, on a Mac or Linux based system, you need `yarn`. On Debian based Linux, you will not have much success using `apt`, but you can check out [this guide](/roles/storage-providers/README.md#install-yarn-and-node-on-linux) for help.

```
$ cd ~/
$ git clone https://github.com/Joystream/joystream.git
$ cd joystream
$ yarn install
$ cd cli
$ yarn link
```

### Both
```
# Test that it's working:
$ joystream-cli help
```
Which should return the output below:

```
Command Line Interface for Joystream community and governance activities

VERSION
  joystream-cli/0.0.0 linux-x64 node-v12.18.2

USAGE
  $ joystream-cli [COMMAND]

COMMANDS
  account         Accounts management - create, import or switch currently used account
  api             Inspect the substrate node api, perform lower-level api calls or change the current api provider uri
  council         Council-related information and activities like voting, becoming part of the council etc.
  help            display help for joystream-cli
  working-groups  Working group lead and worker actions
```

A table with links to each command is shown below:

| Commands                           | Explanation                                                                                         |
|------------------------------------|-----------------------------------------------------------------------------------------------------|
| [account](#account)                |Accounts management - create, import or switch currently used account                                |
| [api](#api)                        |Inspect the substrate node api, perform lower-level api calls or change the current api provider uri |
| [council](#council)                |Council-related information and activities like voting, becoming part of the council etc.            |
| [help](#help)                      |display help for joystream-cli                                                                       |
| [working-groups](#working-groups)  |Working group lead and worker actions                                                                |

## Getting Started

The first time you run a command, you will be prompted to set your API-endpoint. This will determine which node you are talking to. If you are running a node locally, you can choose `localhost`. If not, you can connect to the public node, or select a custom endpoint. You can also go the the [api](#api) section to do it manually.

The first time you want to perform an action that requires a key, you will be asked to import one. You can also go the the [account](#account) section to do it manually.

Note that your imports and setting are stored locally at:
- `/home/<Username>/.local/share/joystream-cli` (Linux)
- `c:\Users\<Username>\AppData\Roaming\joystream-cli` (Windows)
- `/Users/<Username>/Library/Application Support/joystream-cli` (Mac OS)

For each command, try `--help` for info on `args` and `options`. For an overview of all `help` outputs, and more info on the CLI, go [here](https://github.com/Joystream/joystream/tree/master/cli).

## account
These commands lets you perform a variety of functions related to accounts you create or import.
For each command, try `--help` for info on `args` and `options`. Example: `joystream-cli account:choose --help`.

```
$ joystream-cli account
# will return:

account:choose          Choose default account to use in the CLI
account:create          Create new account
account:current         Display information about currently choosen default account
account:export          Export account(s) to given location
account:forget          Forget (remove) account from the list of available accounts
account:import          Import account using JSON backup file
account:transferTokens  Transfer tokens from currently choosen account
```

### account:choose
### account:create

Note that all new accounts are created with `type ed25519`, and you will not be able to extract the seed.

### account:current
### account:export
### account:forget
### account:import
### account:transferTokens

## api
These commands lets you choose your API endpoint, and can also be used to perform chain state queries.
For each command, try `--help` for info on `args` and `options`. Example: `joystream-cli api:getUri --help`.

```
$ joystream-cli api
# will return:

api:getUri   Get current api WS provider uri
api:inspect  Lists available node API modules/methods and/or their description(s), or calls one of the API methods (depending on provided arguments and flags)
api:setUri   Set api WS provider uri
```
### api:getUri
### api:inspect
### api:setUri

## council
Prints the current council information.

```
$ joystream-cli council
# will return:

council:info  Get current council and council elections information
```

### council:info

## help
Prints the helper.

## working-groups
As the main focus of the CLI tool, this is the way for the Storage Lead to perform most of their tasks, and is also helpful for current Storage Providers to manage their role. Note that despite the name `working-groups`, it only applies to the Storage Working Group, for now.

For each command, try `--help` for info on `args` and `options`. Example: `joystream-cli working-groups:application --help`.

```
$ joystream-cli working-groups
# will return:

working-groups:application                 Shows an overview of given application by Working Group Application ID
working-groups:createOpening               Create working group opening (requires lead access)
working-groups:decreaseWorkerStake         Decreases given worker stake by an amount that will be returned to the worker role account. Requires lead access.
working-groups:evictWorker                 Evicts given worker. Requires lead access.
working-groups:fillOpening                 Allows filling working group opening that's currently in review. Requires lead access.
working-groups:increaseStake               Increases current role (lead/worker) stake. Requires active role account to be selected.
working-groups:leaveRole                   Leave the worker or lead role associated with currently selected account.
working-groups:opening                     Shows an overview of given working group opening by Working Group Opening ID
working-groups:openings                    Shows an overview of given working group openings
working-groups:overview                    Shows an overview of given working group (current lead and workers)
working-groups:slashWorker                 Slashes given worker stake. Requires lead access.
working-groups:startAcceptingApplications  Changes the status of pending opening to "Accepting applications". Requires lead access.
working-groups:startReviewPeriod           Changes the status of active opening to "In review". Requires lead access.
working-groups:terminateApplication        Terminates given working group application. Requires lead access.
working-groups:updateRewardAccount         Updates the worker/lead reward account (requires current role account to be selected)
working-groups:updateRoleAccount           Updates the worker/lead role account. Requires member controller account to be selected
working-groups:updateWorkerReward          Change given worker's reward (amount only). Requires lead access.
```
### working-groups:application

This command lets you review an application. The application ID can be found using the [opening](#working-groupsopening) command.

### working-groups:createOpening

Only works with current Storage Lead role key.
Filling out this is not the most user friendly experience, so please review the guide for the [Storage Lead](/roles/storage-lead), and make sure you first create a draft before publishing the Opening. Also note that when prompted to `Provide value for version` use `1`.

### working-groups:decreaseWorkerStake

If the Lead wants to decrease the stake of a Worker, it can be done using this command. Note that the Lead can't increase the stake. This can only be done by the Worker themselves.

### working-groups:evictWorker

If the Lead wants to fire a Worker, it can be done using this command. This also allows for slashing the role stake of a Worker.

### working-groups:fillOpening

Once an Opening is in the "Review Period", this is the way to hire none, one or more applicants as Worker(s), and set their reward.

### working-groups:increaseStake

This allows a Worker to increase their own stake. Can be useful if the Lead requires a bigger role stake, and the alternative is to fire and re-hire the Worker under new conditions.

### working-groups:leaveRole

This allows a Worker or the Lead to terminate their role, and get their stake back after a predetermined time.

### working-groups:opening

Lists the status of a specific Opening by its ID, and displays the Membership Handle, Application ID and Role key of each applicant.

### working-groups:openings

Lists all current and historical Openings.

### working-groups:overview

Lists all current Workers by their Worker ID, Membership Handle, and Role key.

### working-groups:slashWorker

Allows the Lead to slash all or a part of a Workers stake, without firing them.

### working-groups:startAcceptingApplications

Lets the Lead set an Opening in the Accepting Application stage before the specified block height.

### working-groups:startReviewPeriod

Lets the Lead close an Opening for applications, and start reviewing them before making a decision on whom (if any) to hire.

### working-groups:terminateApplication

Lets the Lead terminate an application by its Application ID if in the Accepting Applications stage.

### working-groups:updateRewardAccount

Lets the a Worker change their reward destination for their Role. Requires the Membership key of the Worker.

### working-groups:updateRoleAccount

Lets the a Worker change their Role Account. Useful if the key is lost, or some other problem occur. Requires the Membership key of the Worker.

### working-groups:updateWorkerReward

Lets the Lead change the reward for a Worker by its Worker ID.
