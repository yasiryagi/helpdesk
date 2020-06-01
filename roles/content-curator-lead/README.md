<p align="center"><img src="img/content-curator-lead.svg"></p>

<div align="center">
  <h4>This is a guide to working as the Content Curator Lead on the latest
  <a href="https://testnet.joystream.org/">Joystream Testnet</a>.<h4>
</div>



Table of Contents
==

<!-- TOC START min:1 max:4 link:true asterisk:false update:true -->
- [Overview](#overview)
- [About The Curator Lead](#about-the-curator-lead)
- [Hiring Content Curators](#hiring-content-curators)
    - [Create Opening](#create-opening)
    - [Accepting Applications](#accepting-applications)
    - [Processing Applications](#processing-applications)
- [Working As Content Curator Lead](#working-as-content-curator-lead)
    - [Responsibilities](#responsibilities)
    - [Firing Curators](#firing-curators)
    - [Content Working Group Mint](#content-working-group-mint)
        - [Checking The Mint](#checking-the-mint)
        - [Replenishing The Mint](#replenishing-the-mint)
    - [Advanced Functions](#advanced-functions)
- [Troubleshooting](#troubleshooting)

<!-- TOC END -->

# Overview

This page will contain all information on how to become `Content Curator Lead`, and how to perform the various tasks required for the job.

# About The Curator Lead

Since the introduction of the proposals system for the Constantinople testnet, the `Council` has had the power to appoint a `Content Curator Lead` for the network.

Whenever a position as `Content Curator Lead` is available, `Council Members` will let other testnet participants know through the relevant communication channels, including our Telegram Group and the testnet forum. A forum post or other application process will then be made available in order for the council to consider application from interested parties.

At the moment, being both a `Council Member` and `Content Curator Lead` at the same time is not supported.

# Hiring Content Curators

As the person responsible for the platforms content curation, the Lead may find additional manpower is required, and can hire `Content Curators` to assist.

Consequently, the `Content Curator Lead` must be a role filled by a highly trusted member of our community. If you are interested in becoming Lead, you can make a [proposal](/proposals#set-content-curator-lead) to hire yourself.

In order to be compensated in tJOY directly for their work, the Content Curator Lead must hire themselves.

## Create Opening
The first step towards hiring `Content Curators` is creating a role opening (opportunity) on the platform. You can do this through the Pioneer user interface.

(1) Navigate to the admin panel for `Content Curator Lead`: https://testnet.joystream.org/#/working-groups/admin <br>
(2) Click `Create new opening...` and select the template you would like for the opening. You should probably communicate with the `Council` to determine the best balance of `application stake`, `role stake` and `applicant limit`. <br>
(3) A pop-up window will appear. While the first section covering stakes required to apply is rather self-explanatory, care must be taken with the application form design for curators (structured as a JSON). <br>
(4) To ensure your application form will work, you must validate the JSON against the schema [here](https://github.com/yourheropaul/pioneer/blob/feature/hiring-flow/packages/joy-types/src/hiring/schemas/role.schema.json).
You can use a JSON schema validator tool to do this (e.g. https://www.jsonschemavalidator.net/)

<br>
An example form could be structured as follows:
<br>

```
{
    "version": 1,
    "headline": "Become A Content Curator",
    "job": {
      "title": "Seeking Content Curators",
      "description": "Content Curators will one day be essential for ensuring that the petabytes of media items uploaded to Joystream are formatted correctly and comprehensively monitored and moderated. Our upcoming testnet allows this content monitoring to take place by giving users who are selected for the role administrative access to the Joystream content directory to make changes where necessary.",
    },
    "application": {
      "sections": [
        {
          "title": "About you",
          "questions": [
            {
              "title": "How can we get in touch with you? (Telegram/Keybase/Riot/Email...)",
              "type": "text",
            }
          ]
        },
        {
          "title": "Qualifications And Motivations",
          "questions": [
            {
              "title": "Why do you want to be a content curator?",
              "type": "text area",
            }
          ]
        }
      ]
    },
    "reward": "1 JOY per block",
    "creator": {
      "membership": {
        "handle": "ben",
      }
    },
    "process": {
      "details": [
        "Visit our GitHub repository to find out more about the technical requirements needed to participate in this role.",
        "This is the first time we have offered the content curator role, so this is an exciting opportunity to find out how it works and gain experience on our testnet.",
      ]
    }
  }
```
(5) Click `Create opening` and your opening should appear on the admin panel.


## Accepting Applications
Now that you have succesfully created an opening, the next step is to allow application from testnet participants. To do this, view your listing in the admin panel and click on the `Set stage` button at the bottom of the listing.

You should already have agreed with the council the number of applications you will accept and the deadline for receiving these.

Once these requirements have been met, or at your discretion as lead, you can begin processing the applications you have received. Decide on the applicants you will hire, and move on to the next step!

## Processing Applications

The first step in the processing of applications is to close the opening to any further applicants. You can do this by viewing your listing in the admin panel and clicking on the `Begin applicant review` button at the bottom of the listing.

You should then gather the `WG IDs` of the applicants you would like to hire (visible in the admin panel).

Unfortunately, the admin panel currently doesn't allow for setting a "reward policy" in the UI. To do this, the Lead needs to use an extrinsic.

1. You must then clone [this repo](https://github.com/Joystream/joystream-api-examples), and follow the build instructions.
2. After building, the file `lib/tohex.js` will appear.
3. The instructions in the comments will show you how to add the `WG ID`s as inputs, and copy the `0x...` output.
4. In the Extrinsics sidebar, select: `contentWorkingGroup -> fillCuratorOpening`
5. Find the `curator_opening_id` in the admin panel, paste the output from 3. as `successful_curator_application_ids`,  and set a `reward_policy`.

Once submitted, you have completed the hiring!

# Working As Content Curator Lead

## Responsibilities

Other than the hiring aspect of the role as `Content Curator Lead`, the lead should try to coordinate the actions of the other curators and decide on priorities for curation.

If necessary, upon discussing with the council, the `Content Curator Lead` can also decide to fire curators who are not performing their jobs adequately.

Most of the time however, the responsibilities of the `Content Curator Lead` will be very similar to those of a standard `Content Curator`. You can read about these responsibilities in [this section](../roles/content-curators#content) of the guide for `Content Curators`.


## Firing Curators
Unfortunately, it may sometimes be necessary to fire curators who are not doing their jobs correctly.

This process requries submitting an extrinsic: `contentWorkingGroup -> terminateCuratorRole`. You must include the `curatorId` of the curator you would like to fire alongside a short hex-encoded message explaining why the person was fired.

There are a number of [online tools](https://www.online-toolz.com/tools/text-hex-convertor.php) which make it easy to encode messages into hex.

## Content Working Group Mint

### Checking The Mint
To check the details of the current Content Working Group Mint:

(1) Use the following chain state query to determine the current mint ID: `contentWorkingGroup -> mint`.<br>
(2) Check the details of the mint using the following query: `minting -> mints`.

### Replenishing The Mint
It will sometimes be necessary to replenish the Content Working Group Mint. This can be done through a proposal [here](https://testnet.joystream.org/#/proposals/new) which must be approved by the council in order to take effect.

For this reason it is best to discuss these sorts of proposals with the council before making them.

## Advanced Functions
If you need help with some of the more advanced operations associated with being the `Content Curator Lead` (e.g. maintaining the versioned store), please simply ask for help in the Telegram group or get in touch with one of the Jsgenesis team directly.

---

# Troubleshooting
If you had any issues setting up this role, you may find your answer here!
