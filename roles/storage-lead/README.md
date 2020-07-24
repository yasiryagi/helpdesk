<p align="center"><img src="img/storage-lead.svg"></p>

<div align="center">
  <h4>This is a guide to working as the Storage Provider Lead on the latest
  <a href="https://testnet.joystream.org/">Joystream Testnet</a>.<h4>
</div>



Table of Contents
==

<!-- TOC START min:1 max:4 link:true asterisk:false update:true -->
- [Overview](#overview)
- [About The Storage Lead](#about-the-storage-lead)
- [Hiring Storage Lead](#hiring-storage-lead)
    - [Proposals](#proposals)
      - [Create Opening](#create-opening)
      - [Review Applications](#review-applications)
      - [Processing Applications](#processing-applications)
- [Hiring Storage Providers](#hiring-storage-providers)
  - [Using the CLI](#using-the-cli)
    - [Create Opening](#create-opening-1)
    - [Accepting Applications](#accepting-applications-1)
    - [Processing Applications](#processing-applications-1)
- [Working As Storage Lead](#working-as-storage-lead)
    - [Responsibilities](#responsibilities)
    - [Useful Commands](#useful-commands)
- [Troubleshooting](#troubleshooting)

<!-- TOC END -->

# Overview

This page contains all the information required on becoming `Storage Provider Lead`, and how to perform the various tasks required for the job.

# About The Storage Lead

The `Storage Working Group Lead`, `Storage Provider Lead` or simply `Storage Lead` is a new role introduced as part of the _Nicaea_ upgrade to our Constantinople testnet. The `Storage Lead` is hired directly by the council through the proposals system and is responsible for the hiring, firing and wider management of `Storage Providers` on the network.

# Hiring Storage Lead

## Proposals

Hiring the `Storage Lead` is the responsibility of the `Council` through the proposals system. Three new proposal types have been introduced to support the hiring process, and more have also been added to allow the `Council` to effectively manage the lead once "in office", through slashing, setting the mint capacity, decreasing stake and firing etc.

### Create Opening

The first step from the Council's perspective is creating an opening where prospective `Storage Leads` can apply for the role.
Within [Pioneer](https://testnet.joystream.org), navigate first to the proposals tab and select `New Proposal`.

To create an opening, the `Add Working Group Leader Opening` proposal must be selected.

Once selected, the proposer can decide on a number of variables which can be set as part of the proposal, including 

For the JSON schema, which reflects what applicants will see in the UI and what information they will have to submit, it is recommended to use a [JSON validator](https://jsonlint.com/) to ensure that the opening is valid.

### Review Applications

In order to formally "close" the opening to further applicants and inform the existing candidates that their submissions are currently being considered, the status of the opening must be changed to "In Review".

This can be done very easily through the creation of another proposal by the `Council`, this time with the `Begin Review Working Group Leader Application` proposal type. The main thing to pay attention to here is the `Working Group Opening ID` created earlier. Helpfully there is a dropdown box for choosing among the currently active openings, in case you have forgotten the ID.

### Processing Applications

The final step in hiring the `Storage Lead` is to create a `Fill Working Group Leader Opening`. The requirements here simply to choose the relevant opening from the drop-down menu and choose betwen the candidate applications (in JSON format) shown on the page.

Once a candidate has been chosen and the final proposal has passed, the focus is now on the new `Storage Lead`...

# Hiring Storage Providers

## Using the CLI
Our newly developed Command-Line Interface (CLI) is an essential tool for the Storage Lead, as it is by far the simplest way to hire and manage `Storage Providers` and applicants for this role. The program and its instructions for use can be found [here](https://github.com/Joystream/joystream/tree/master/cli).

All of the useful commands which can be executed by the `Storage Lead` will require the lead to import their "role" key rather than their "member" key. Consequently, in the CLI the `account:import` and `account:choose` commands will need to be used.

### Create Opening

To create an opening, the lead needs to run the `working-groups:createOpening` command using their role key.

There are some options for specific purposes which can be selected, as shown below:
```
OPTIONS
  -c, --createDraftOnly      If provided - the extrinsic will not be executed. 
                             Use this flag if you only want to create a draft.

  -d, --useDraft             Whether to create the opening from existing draft.
                             If provided without --draftName - the list of 
                             choices will be displayed.

  -g, --group=group          (required) [default: storageProviders] The working 
                             group context in which the command should be 
                             executed
                             Available values are: storageProviders.

  -n, --draftName=draftName  Name of the draft to create the opening from.

  -s, --skipPrompts          Whether to skip all prompts when adding from draft 
                             (will use all default values)
```
Once this command is run, the prompts to set up the opening are self-explanatory.

### Accepting Applications

### Processing Applications

# Working As Storage Lead

## Responsibilities


## All Commands

Within the CLI, all of the relevant commands for the Storage Lead can be found through the following query:<br>
`working-groups --help`

For convenience, the output of this command is listed below to give a sense of the powers and responsibilities of the Storage Lead:
```
  working-groups:application                 Shows an overview of given
                                             application by Working Group
                                             Application ID
  working-groups:createOpening               Create working group opening
                                             (requires lead access)
  working-groups:decreaseWorkerStake         Decreases given worker stake by an
                                             amount that will be returned to the
                                             worker role account. Requires lead
                                             access.
  working-groups:evictWorker                 Evicts given worker. Requires lead
                                             access.
  working-groups:fillOpening                 Allows filling working group
                                             opening that's currently in review.
                                             Requires lead access.
  working-groups:increaseStake               Increases current role
                                             (lead/worker) stake. Requires
                                             active role account to be selected.
  working-groups:leaveRole                   Leave the worker or lead role
                                             associated with currently selected
                                             account.
  working-groups:opening                     Shows an overview of given working
                                             group opening by Working Group
                                             Opening ID
  working-groups:openings                    Shows an overview of given working
                                             group openings
  working-groups:overview                    Shows an overview of given working
                                             group (current lead and workers)
  working-groups:slashWorker                 Slashes given worker stake.
                                             Requires lead access.
  working-groups:startAcceptingApplications  Changes the status of pending
                                             opening to "Accepting
                                             applications". Requires lead
                                             access.
  working-groups:startReviewPeriod           Changes the status of active
                                             opening to "In review". Requires
                                             lead access.
  working-groups:terminateApplication        Terminates given working group
                                             application. Requires lead access.
  working-groups:updateRewardAccount         Updates the worker/lead reward
                                             account (requires current role
                                             account to be selected)
  working-groups:updateRoleAccount           Updates the worker/lead role
                                             account. Requires member controller
                                             account to be selected
  working-groups:updateWorkerReward          Change given worker's reward
                                             (amount only). Requires lead
                                             access.
```

---

# Troubleshooting
If you had any issues setting up this role, you may find your answer here!
