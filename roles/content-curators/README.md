<p align="center"><img src="img/content-curators.svg"></p>

<div align="center">
  <h4>This is a guide for applying, and working as a Content Curator on the latest
  <a href="https://testnet.joystream.org/">Joystream Testnet</a>.<h4>
  <h4>If you are interested in the Content Curator Lead role, please instead visit this
    <a href="../content-curator-lead">dedicated guide</a>.<h4>
</div>



Table of Contents
==
<!-- TOC START min:1 max:4 link:true asterisk:false update:true -->
- [Overview](#overview)
- [Hiring Process](#hiring-process)
  - [Applying for a Role](#applying-for-a-role)
    - [Accepting applications](#accepting-applications)
      - [Track the status of your application](#track-the-status-of-your-application)
      - [Withdraw or change your application](#withdraw-or-change-your-application)
    - [Applications in review](#applications-in-review)
      - [Track the status of your application](#track-the-status-of-your-application-1)
  - [Hiring complete](#hiring-complete)
- [Working as a curator](#working-as-a-curator)
  - [Curation Policy](#curation-policy)
    - [Regular Checks](#regular-checks)
      - [Example](#example)
    - [Council Reports](#council-reports)
      - [Example](#example-1)
    - [Policy](#policy)
      - [Videos](#videos)
      - [Channels](#channels)
    - [Discretion](#discretion)
  - [Curation](#curation)
    - [Censoring](#censoring)
    - [Updating](#updating)
    - [Removing](#removing)
<!-- TOC END -->

# Overview

This page will contain all information on how to apply for the role of `Content Curator`, and how to perform the various tasks required for the job.

# Hiring Process

First, open [Pioneer](https://testnet.joystream.org/), and navigate to the `Working groups` sidebar. Here you will find an overview of the current `Content Curators`, and the `Content Lead`. In the `Opportunities` tab, you can get an overview of future, current and previous openings, along with terms, settings, status, and history (if applicable). The terms and settings include the following parameters to consider:

- `Max active applicants` - The maximum number of active applicants for an opening
- `Max review period length` - The maximum length (in blocks) of the review period
- `Application staking policy` - This stake applies for the application only
  - A fixed or minimum stake for the application
  - The length (in blocks) from an application has been resolved until the stake is returned
- `Role staking policy` - This stake applies for the role itself, and successful applicants will have these funds locked while being in the role
  - A fixed or minimum stake for the role
  - The length (in blocks) from an unsuccessful application has been resolved, or a successful applicant has quit or gotten fired, until the stake is returned
- `Reward policy` - The rewards (in `X [JOY]/N [blocks]`) for successful applicants


## Applying for a Role

In order to be able to successfully apply for a role as a `Content Curator`, you have to register a membership, which requires tokens. It should also be expected that there will be staking requirements, which will require more tokens. The exact terms should be clear from the listing.

You will be able to complete the application process as long as you have generated a keypair, but your application transaction will be discarded by the runtime for a variety of reasons:
- If you are not a member.
- If you attempt to apply twice with the same `MemberId`.
- If you attempt to apply with insufficient stake.
- If you try to stake more tokens than you can (note the application transaction costs 1 JOY)
- If you apply for a role with a maximum number of slots, with fixed or no stake requirements, and the maximum application slots have been reached.
- If you try to apply manually through the `Extrinsics` sidebar, you can fail for a variety of other reasons.

### Accepting applications

Only openings in the "Accepting applications" stage can be applied for. Assuming there is one or more openings in this stage that is of interest (and you have sufficient tokens) you can apply for the role. Click the green `APPLY NOW` button inside the Opening box, and follow the staking instructions and write the requested information.

After you have completed this, you will be sent to a confirmation page, where you can submit the application. Once you click `Make transaction and submit application`, a new key will be generated for you. This will be the `controller` key for the role, which you will need to use for all curation transactions if you are hired. It will by default not have a password, and be named `<TITLE OF JOB OPENING> ROLE KEY`. You can rename, set a password, and download a backup .json file of this key in the `My Keys` sidebar.

#### Track the status of your application
Once your application is submitted, you can check the status of your application by selecting this key, and navigate to the `My roles` tab under the `Working groups` sidebar.

#### Withdraw or change your application
While the opening is in the "Accepting applications", you can withdraw your application. As you can not apply twice with the same membership key, the only way to change your application, either to add more stake or update the application text, you need to withdraw your application first. This can be done by selecting the generated role key, navigate to the `My roles` tab under the `Working groups` sidebar, and clicking the `Cancel and withdraw stake` button.

### Applications in review

Once the `Content Curator Lead` is satisfied with the quality/quantity of the applications, the `Applications in review` stage will begin. In this stage, applicants can no longer withdraw their application. The `Content Curator Lead` can do a final review of the applicants, and will make a decision on how many and whom, if any, to hire for the role. As there is a `Max review period length`, inaction will default to no hires, and both the "Role stake" and "Application stake" will be returned in due time.

#### Track the status of your application
As in the [Accepting applications](#accepting-applications) stage, you can track the status of your application by selecting the role key, and navigate to the `My roles` tab under the `Working groups` sidebar.

## Hiring complete

Assuming the `Content Curator Lead` decides to hire one or more applicants, a transaction that hires a set of applicants and sets the `Reward policy` will be made. Successful applicants will appear in the `Workings groups` tab with their membership handle, their "Role stake" and, as time goes, the tokens earned for the job.

Once either the above happens, the lead chooses to terminate the opening without hiring anyone, or the `Max review period length` expires, the opening will go to the `Hiring complete` stage.

As in the previous stages, regardless if your application was successful or not, you can check the status of your application by selecting the role key, and navigate to the `My roles` tab under the `Working groups` sidebar.

If your application was successful, you will now be able to curate content on the Joystream platform.

# Working as a curator

The main job for curators is to regularly check the content directory and channels, and checking that rules and guidelines are being followed.

## Curation Policy
Due to the large influx of users, and incentives for video curation, we need a formal workflow for the `Content Curators`, and a curation policy.

Note that parts of this policy only applies to this testnet (Babylon).

The exact intervals and delegation procedure should be agreed upon by the Council and the Lead, and the Lead and the Curators respectively.

The "Regular Checks", in lack of a better term, are critical. Although discussions can be had around the exact format, something very close to this is required.

The "Council Reports" are a lot more comprehensive and challenging. Unlike the "Regular Checks", these are primarily meant for the Council, and the format and scope here may be reduced somewhat.

### Regular Checks
At some defined (minimum) frequency, the Curator reviews all new videos and channels uploaded.

To avoid everyone stepping on each others toes, the Lead should consider splitting the workload between the team, eg. by day/time and if required, also by language, category, etc.

Each review of this kind should be reported in a designated thread on the Forum, for both the Council and Channel Owners on the Forum.

#### Example
- *Date:* `<dd.mm.yy>`
- *Snapshot:* `block #number`
- *Last report:* `<link.to.previous.post>`
- *Author:* `<ID/Handle/WorkerId>`
- *Overall Statistics:*
  - *Total entities:* `2974`
  - *Total channel entities:* `149`
  - *Total video entities:* `535`
- *Since Last Report:*
  - *Total new entities:* `12`
  - *Total new channel entities:* `2`
  - *Total new video entities:* `2`

##### New Entities Since Last Report
|Entity ID |Class/ID      |Owner [memberId/Handle]|Title                          |Curator Status                                  |Reference        |
|:--------:|:------------:|:---------------------:|:-----------------------------:|:----------------------------------------------:|:---------------:|
|`<ID>`    |Video/10      |`<ID>/<handle>`        |`<title>`                      |Censored - requires attribution -`1`            |`#<number>`      |
|`<ID>`    |Video/10      |`<ID>/<handle>`        |`<title>`                      |Approved                                        |NA               |
|`<ID>`    |Channel/1     |`<ID>/<handle>`        |`<title>`                      |Hidden - Missing Avatar                         |`#<number>`      |
|`<ID>`    |Channel/1     |`<ID>/<handle>`        |`<title>`                      |Approved                                        |NA               |
|`<ID>`    |Video/10      |`<ID>/<handle>`        |`<title>`                      |Poor thumbnail, improve by `#<number>` - `2`    |NA               |

1. All licenses of this kind requires attribution.
2. The thumbnail is misleading, and in low resolution.

---

In the table, the "Entity Id" refers to the video or channel `entityId`, where the former also includes the other (4-5) entities created when a video is uploaded.
The "owner" and "title" is also included, to make it easier for the uploader to find out what the status of their channel is.

The "curator status" should indicate either `Approved`, or what action, if any, was taken. In that case, a reference to the block height where the transaction change occurred.

The Curator team should also monitor this thread, as it would be where the channel owner reports back to them if they made change(s) requested, disagrees with some action taken, or simply has any questions.

### Council Reports
At some agreed interval, assumed to be at least once for each Council Term, _all_ videos and channels must be "checked in" on. There could be many reasons why  to verify that they are still acceptable. In some cases, some details may have been overlooked in previous "check ups", but there is also the chance that a Channel owner or a (rogue) Curator makes a change that requires an action.

To avoid having to go through thousands of entities every time, the Lead, or some other curator could perhaps deploy a script that checks for certain types of transaction that makes changes to the content directory.

To avoid everyone stepping on each others toes, the Lead should consider splitting the workload between the team, eg. by language, category, etc.

The results from this check in should be reported to the Council and the Channel Owners on the Forum.

#### Example

##### Introduction
- *Report ID:* `<ID>`
- *Date:* `<dd.mm.yy>`
- *Snapshot:* `block #number`
- *Last report:* `<link.to.previous.post>`
- *Author:* `<ID/Handle/WorkerId>`
- *Overall Statistics:*
  - *Total entities:* `2974`
  - *Total channel entities:* `149`
  - *Total video entities:* `535`
- *Changes Since Last Report:*
  - *Total new entities:* `200`
  - *Total new channel entities:* `10`
  - *Total new video entities:* `38`

##### Changes Made Since Last Report - Owner
|Entity ID |Class/ID      |Owner [memberId/Handle]|Title                          |Changed                 |Consequence            |Reference (block/post)    |
|:--------:|:------------:|:---------------------:|:-----------------------------:|:----------------------:|----------------------:|:------------------------:|
|`<ID>`    |Channel/1     |`<ID>/<handle>`        |`<title>`                      |Updated banner          |NA                     |`#<number>`               |
|`<ID>`    |Video/10      |`<ID>/<handle>`        |`<title>`                      |Changed thumbnail       |No longer censored     |`#<number>/<postId>`      |
|`<ID>`    |Video/10      |`<ID>/<handle>`        |`<title>`                      |Added thumbnail         |No longer hidden       |`#<number>/<postId>`      |

##### Changes Made Since Last Report - Curators
|Entity ID |Class/ID      |Owner [memberId/Handle]|Title                          |Curator Status                                  |Reference (block/post)    |
|:--------:|:------------:|:---------------------:|:-----------------------------:|:----------------------------------------------:|:------------------------:|
|`<ID>`    |Video/10      |`<ID>/<handle>`        |`<title>`                      |Attribution added, no longer censored           |`#<number>/<postId>`      |
|`<ID>`    |Channel/1     |`<ID>/<handle>`        |`<title>`                      |Hidden - Missing Avatar                         |`#<number>/<postId>`      |
|`<ID>`    |Video/10      |`<ID>/<handle>`        |`<title>`                      |Censored - requires attribution                 |`#<number>/<postId>`      |
|`<ID>`    |Video/10      |`<ID>/<handle>`        |`<title>`                      |Changed thumbnail, no longer censored           |`#<number>/<postId>`      |
|`<ID>`    |Video/10      |`<ID>/<handle>`        |`<title>`                      |Added thumbnail, no longer hidden               |`#<number>/<postId>`      |

##### All Non-Playable Videos
|Entity ID |Channel ID    |Owner [memberId/Handle]|Title                          |Reason                                          |Reference (block/post)    |
|:--------:|:------------:|:---------------------:|:-----------------------------:|:----------------------------------------------:|:------------------------:|
|`1834`    |`1829`        |`<ID>/<handle>`        |`<title>`                      |Text Property too long at creation              |`#2175058/NA`             |
|`1839`    |`1829`        |`<ID>/<handle>`        |`<title>`                      |Video deleted by owner                          |`#2175164/NA`             |
|`<ID>`    |`<title>`     |`<ID>/<handle>`        |`<title>`                      |Censored, Copyright                             |`#<number>/<postId>`      |
|`<ID>`    |`<ID>`        |`<ID>/<handle>`        |`<title>`                      |Set as not public by owner                      |`#<number>/<postId>`      |
|`<ID>`    |`<ID>`        |`<ID>/<handle>`        |`<title>`                      |Censored - requires attribution                 |`#<number>/<postId>`      |
|`2402`    |`2396`        |`<ID>/<handle>`        |`<title>`                      |Added to "empty" channels                       |`#2215006/NA`             |

##### All Non-Visible Channels
|Entity ID |Videos        |Owner [memberId/Handle]|Title                          |Reason                                          |Reference (block/post)    |
|:--------:|:------------:|:---------------------:|:-----------------------------:|:----------------------------------------------:|:------------------------:|
|`1840`    |`2402`        |`<ID>/<handle>`        |`<title>`                      |Rejected - channel name not unique              |`#2214830/NA`             |
|`<ID>`    |`<ID,ID,ID>`  |`<ID>/<handle>`        |`<title>`                      |Hidden - Missing Avatar                         |`#<number>/<postId>`      |
|`<ID>`    |-             |`<ID>/<handle>`        |`<title>`                      |No videos in channels                           |`#<number>/<postId>`      |
|`2396`    |`<ID,ID,ID>`  |`<ID>/<handle>`        |`<title>`                      |Rejected - Text property (title?) too long      |`#2175058/NA`             |

##### Notes
Some notes from the curator.

---

This may look like a _lot_ of work, but all the data needed can be found using a script that checks all blocks from `n` to `m`, and looks at all events with `event.section == contentDirectory`, and returns these blocks (or the full data). A basic version of this will be made available in the [community repo](https://github.com/Joystream/community-repo).

- The CLI can be used to find all video, `content-directory:entities <10|Video>` and channel, `content-directory:entities <1|Channel>` entities in the content directory.
- The data in "Changes Made Since Last Report - Owner" can be found by said script.
- The data in "Changes Made Since Last Report - Curators" can be found using the same script, in addition to copy/pasting data the from the [Regular Checks](#regular-checks) - which _should_ be correct if all actions were reported there..
- "All Non-Playable Videos" and "All Non-Visible Channels" should be extractable from older reports, and data already filled in.

### Policy

`warning`
Means posting a note in the forum, as part of the [Regular Checks](#regular-checks), that something has to be corrected within a certain time period

`hidden`
Means updating the status (`isPublic`) for a video from `true` to `false`. Note that this can be changed back to `true` by the owner!

`censor`
Means updating the status (`isCensored`) for a video from `true` to `false`. Unlike `hidden`, this can not be changed back by anyone but another curator.

#### Videos
**When to (only) issue a `warning`:**
- Missing artwork
  - If the thumbnail is missing, or just really poor, the video can be `hidden`, or issued a `warning` (Council decides u.n.o.).
- Suspicious license
  - If the Curator suspects, but is not able to verify that a video is incorrectly licensed, a `warning` can be issued, leaving the channel owner some defined deadline to respond and provide more information or evidence.

**When to make a video `hidden`:**
- The content must be as "advertised"
  - If the title, description, category and thumbnail implies a baking video, the video should not be a documentary about Bitcoin
- Missing artwork
  - If thumbnail is missing, or just really poor, the the video can be hidden, or given a warning (Council decides u.n.o.)
- Poor quality
  - If the video quality is "unreasonably" low

**When to `censor` a video:**
- License requires attribution
  - If the selected license requires attribution, but none is given the, or attribution is incorrect
- Suspected copyright violation
  - If it's not clear that the video is in violation, but this is strongly suspected by the Curator
- Status change by owner without resolving issue
  - If a video is `hidden` by a curator, and the channel owner makes it public again without correcting the issue described by the curators
- Terms of Service violation
  - If the video is in violation of other parts of the [ToS](https://testnet.joystream.org/#/pages/tos)

#### Channels
**When to (only) issue a `warning`:**
- Missing artwork
  - If the avatar or cover is missing, or just really poor, the channel can be `hidden`, or issued a `warning` (Council decides).

**When to make a channel `hidden`:**
- No videos
  - If there are no videos, that is *not* `censored` or `hidden`, or simply no uploads to a channel that wasn't made very recently
- Multiple infractions
  - If multiple videos are currently `censored` or `hidden`, and the owner has made no efforts to fix this
- Recurring infractions
  - The channel owner continuously uploads new videos requiring curation

**When to `censor` a channel:**
- Multiple or recurring *serious* infractions
  - If multiple videos are currently `censored` and the owner has made no efforts to fix this

In this case, `limiting` the channel owners right to create new channels/videos may be required.
As only the [Lead](/roles/content-curator-lead/README.md#increasing-or-decreasing-limits-for-members) can do this.


### Discretion
These rules are not clearly defined in all cases, so it's important that curators are able to use discretion. In many cases, it's preferable to try and get in touch with the channel owner first, rather than immediately pull the trigger. If in doubt, contact the `Lead` first. In other cases, immediate action may be required.

"Speak softly, and carry a big stick"
- Theodore Roosevelt

## Curation
Curation can mean either censoring, updating or altogether removing content. It can also involve putting content in a "featured" state on the platform.

A key part of understanding Curation is to understand the basics of the Content Directory using the [CLI](https://github.com/Joystream/joystream/tree/master/cli). Examples:

```
# Info about a specific class:
$ joystream-cli content-directory:class <CLASSNAME|CLASSID>

# Info about a specific entity:
$ joystream-cli content-directory:entity <ENTITYID>

# List all classes:
$ joystream-cli content-directory:classes

# List all entities in a class:
$ joystream-cli content-directory:entities <CLASSNAME|CLASSID>
```

### Censoring
The first step to perform if a video or channel is not in line with the testnet Terms of Service is to censor it.

If the Curator has "Maintainer" permission for a Channel or Video, this is done by the following [CLI](https://github.com/Joystream/joystream/tree/master/cli) command `joystream-cli media:curateContent`. Suppose you want to censor a Video with `entityId` 300:
```
$ joystream-cli media:curateContent --className 10 --status Censored --id 300
```

If it's a minor issue, you can try to reach out to the creator, and have them make the modification required. If the issue gets resolved, you can reverse the censoring by:

```
$ joystream-cli media:curateContent --className 10 --status Accepted --id 300
```

If it's a more serious infraction, contact the Lead and let them know. In general, it's preferable to share the information with other curators regardless.

### Updating
If you have the permission, you can update a Channel (with `entityId` 100) or Video (with `entityId` 200) like so:
```
$ joystream-cli media:updateChannel 100 --asCurator
$ joystream-cli media:updateVideo 200 --asCurator
```
This is particularly useful if the Channel owner is using the wrong "License", or has forgotten "Attribution".


### Removing
If you have the permission, you can remove a Channel (with `entityId` 100) like so:
```
$ joystream-cli content-directory:removeEntity 100 --context <Curator|Lead>
```
A Video will usually have 5 or more entities for each upload, so a Curator will need to check which entities are associated with the Video, and delete them "backwards". If you want to remove the Video with `entityId` 200...
```
$ joystream-cli content-directory:entity 200
$ joystream-cli content-directory:removeEntity 200

$ joystream-cli content-directory:entity 199
$ joystream-cli content-directory:removeEntity 199

$ joystream-cli content-directory:entity 198
$ joystream-cli content-directory:removeEntity 198

$ joystream-cli content-directory:entity 197
$ joystream-cli content-directory:removeEntity 197

$ joystream-cli content-directory:entity 196
$ joystream-cli content-directory:removeEntity 196

$ joystream-cli content-directory:entity 195
# When you a see a new Video or Channel entity (or something else unrelated), you are clear and must stop
```
