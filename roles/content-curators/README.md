<p align="center"><img src="img/content-curators.svg"></p>

<div align="center">
  <h4>This is a guide for applying, and working as a Content Curator on the latest
  <a href="https://testnet.joystream.org/pioneer">Joystream Testnet</a>.<h4>
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
- [Advanced](#advanced)
<!-- TOC END -->

# Overview

This page will contain all information on how to apply for the role of `Content Curator`, and how to perform the various tasks required for the job.

# Hiring Process

First, open [Pioneer](testnet.joystream.org/), and navigate to the `Working groups` sidebar. Here you will find an overview of the current `Content Curators`, and the `Content Lead`. In the `Opportunities` tab, you can get an overview of future, current and previous openings, along with terms, settings, status and history (if applicable). The terms and settings include the following parameters to consider:

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
While the opening is in the "Accepting applications", you can withdraw your application. As you can not apply twice with the same membership key, the only way to change your application, either to add more stake or update the application text, you need to withdraw you application first. This can be done by selecting the generated role key, navigate to the `My roles` tab under the `Working groups` sidebar, and clicking the `Cancel and withdraw stake` button.

### Applications in review

Once the `Content Curator Lead` is satisfied with the quality/quantity of the applications, the `Applications in review` stage will begin. In this stage, applicants can no longer withdraw their application. The `Content Curator Lead` can do a final review of the applicants, and will make a decision on how many and whom, if any, to hire for the role. As there is a `Max review period length`, inaction will default to no hires, and both the "Role stake" and "Application stake" will be returned in due time.

#### Track the status of your application
As in the [Accepting applications](#accepting-applications) stage, you can track the status of your application by selecting the role key, and navigate to the `My roles` tab under the `Working groups` sidebar.

## Hiring complete

Assuming the `Content Curator Lead` decides to hire one or more applicants, a transaction that hires a set of applicants and sets the `Reward policy` will be made. Successful applicants will appear in the `Workings groups` tab with their membership handle, their "Role stake" and, as time goes, the tokens earned for the job.

Once either the above happens, the lead chooses to terminate the opening without hiring anyone, or the `Max review period length` expires, the opening will go to the `Hiring complete` stage.

As in the the previous stages, regardless if your application was successful or not, you can check the status of your application by selecting the role key, and navigate to the `My roles` tab under the `Working groups` sidebar.

If your application was successful, you will now be able to curate content on the Joystream platform.

# Working as a curator

...

# Advanced

...
