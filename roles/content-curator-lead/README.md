<p align="center"><img src="img/content-curator-lead.svg"></p>

<div align="center">
  <h4>This is a guide to working as the Content Curator Lead on the latest
  <a href="https://testnet.joystream.org/">Joystream Testnet</a>.<h4>
</div>



Table of Contents
==

<!-- TOC START min:1 max:4 link:true asterisk:false update:true -->
- [Overview](#overview)
- [Who Can Become Content Curator Lead](#who-can-become-content-curator-lead)
- [Hiring Content Curators](#hiring-content-curators)
    - [Create Opening](#create-opening)
    - [Accepting Applications](#accepting-applications)
    - [Processing Applications](#processing-applications)
- [Working As Content Curator Lead](#working-as-content-curator-lead)
- [Troubleshooting](#troubleshooting)

<!-- TOC END -->

# Overview

This page will contain all information on how to become `Content Curator Lead`, and how to perform the various tasks required for the job.

# Who Can Become Content Curator Lead

Since the introduction of the proposals system for the Constantinople testnet, the `Council` has had the power to appoint a `Content Curator Lead` for the network.

Whenever a position as `Content Curator Lead` is available, `Council Members` will let other testnet participants know through the relevant communication channels, including our Telegram Group and the testnet forum. A forum post or other application process will then be made available in order for the council to consider application from interested parties.

At the moment, being both a `Council Member` and `Content Curator Lead` at the same time is not supported.

# Hiring Content Curators

The main responsibility of the `Content Curator Lead` is hiring `Content Curators` for the platform. The lead determines the criteria for becoming a curator, handles applications and even fires curators who may not be performing their role in an acceptable manner.

Consequently, the `Content Curator Lead` must be a role filled by a highly trusted member of our community. If you are interested in becoming Lead, get in touch with us.

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


## Processing Applications


# Working As Content Curator Lead

Other than the hiring aspect of the role as `Content Curator Lead`, the lead should try to coordinate the actions of the other curators and decide on priorities for curation.

---

# Troubleshooting
If you had any issues setting up this role, you may find your answer here!
