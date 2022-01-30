<p align="center"><img src="img/distributor.png"></p>

<div align="center">
  <h4>This is a guide to working as the Distributor Lead on the latest
    <a href="https://testnet.joystream.org/">testnet</a>.</h4><br>
</div>

Table of Contents
==
<!-- TOC START min:1 max:3 link:true asterisk:false update:true -->
  - [The Storage and Distribution System](#the-storage-and-distribution-system)
    - [Bags](#bags)
    - [Storage Buckets](#storage-buckets)
    - [Distribution Buckets](#distribution-buckets)
- [Distributor CLI](#distributor-cli)
  - [Leader](#leader)
    - [cancel-invitation](#cancel-invitation)
    - [create-bucket](#create-bucket)
    - [create-bucket-family](#create-bucket-family)
    - [delete-bucket](#delete-bucket)
    - [delete-bucket-family](#delete-bucket-family)
    - [invite-bucket-operator](#invite-bucket-operator)
    - [remove-bucket-operator](#remove-bucket-operator)
    - [set-bucket-family-metadata](#set-bucket-family-metadata)
    - [set-buckets-per-bag-limit](#set-buckets-per-bag-limit)
    - [update-bag](#update-bag)
    - [update-bucket-mode](#update-bucket-mode)
  - [update-bucket-status](#update-bucket-status)
    - [update-dynamic-bag-policy](#update-dynamic-bag-policy)
  - [Operator](#operator)
    - [accept-invitation](#accept-invitation)
    - [set-metadata](#set-metadata)
  - [Initial Configurations - Giza](#initial-configurations---giza)
  - [Good Practice](#good-practice)
    - [Bucket Families](#bucket-families-1)
    - [New Buckets](#new-buckets)
    - [Dynamic Bag Creation Policy](#dynamic-bag-creation-policy)
    - [When to Update Bags](#when-to-update-bags)
<!-- TOC END -->

## The Storage and Distribution System
Storage and Distribution system is well documented on GitHub. A simple version will be outlined here.

### Bags
A bag is a collection of assets associated (in most cases `*`) with a single `channel`.

A single channel cannot be in two bags, and a bag can only contain one channel.

`*` Currently, we only support `dynamic` bags (`static` will be added), on the `channel` level. Channels are identified by a Channel ID (`channelId`) that is incremented by 1 for each new channel as transaction are made.

#### Creation
Whenever a new channel is created, a new "bag" is created.

If a new channel is created and given the `channelId` 1337, the new bag will be `dynamic:channel:1337`.

#### Deletion
Even if there are no assets in the bag, it will persist until the channel is deleted.

#### Modification
Every time an asset is added (eg. a video is created), the bag grows larger.
Every time an asset is removed (eg. a video is deleted), the bag shrinks.

At all times, a bag will have `n` files, with the total size of `s` bytes associated with it.

### Storage Buckets
Both the Storage and Distribution implementation uses the concept of `buckets`. A `bucket` can be populated with multiple `bags`. A bag can be held by multiple buckets, and be added/removed to/from a bucket by the Lead.

#### Creation
A Storage Bucket can only be created by the Storage Lead. A single worker only can be invited, (and thus accept) to be the operator of a single bucket at the time. Only if the operator is removed, or the invitation is cancelled, can a new operator be invited. However, a worker can be invited to, and accept, to operate multiple buckets.

When creating them, a limit on both the number of files it can contain, and the total size of the files, must be set. If this capacity is exceeded, any new assets added will be rejected `*`.

`*` The consequence of that is that any channels whose bag is assigned to this bucket, will not be able to create new videos. It will fail on the runtime level, even if the Storage Node assigned to the bucket has the storage space to accept them, or that there are other buckets whose limits are not exceeded that also is assigned that bag.

#### Deleting or Re-assigning Storage Buckets
As long as there are any bags in a bucket, the bucket can not be deleted. And a worker that is operating a bucket can not be fired from a role, before they are removed from the bucket.

In order to delete a bucket, the Lead must first remove all the bags from it.

#### Modification
A bag can be added to, and removed from, a bucket at any time, as long as it doesn't violate some other parameter.

Note that this means a bag can be removed from all buckets, which makes it hard (if not impossible) to recover that data. Therefore, the Lead should never remove a bag from a bucket without knowing it is held (not "just" supposed to be held) by other buckets.


### Distribution Buckets
The distribution buckets work somewhat differently, as they also have the concept of `bucket families`.

#### Bucket Families
A bucket family is a collection of distribution buckets associated with a geographical region (eg. from larger to smaller: World, Europe, Northern Europe, Scandinavia, Norway, Oslo). This is meant to reduce latency when serving content.

At the time of writing, the Joystream Player picks distributors to serve content "randomly", giving much less importance to this concept. That doesn't mean they should be ignored, as it's not trivial to re-configure this in flight.

However, it's probably still more than sufficient to focus on 2-4 families for now, until the project and usage has grown significantly.

##### Creation
New bucket families are created, and it's metadata is set, by the Lead.

##### Modification
New buckets can be added to a family by the Lead.

##### Deletion
A bucket family can only be deleted if there are no buckets in it.

#### Buckets
A (distribution) buckets lives in a bucket family. Inside it, there are bags, which work identically to the bags in storage buckets.

##### Creation
When creating a new bucket, it must be added to an (existing) family.

##### Modification
A new bag that is created (`dynamic:channel:1336`) will be placed in distribution buckets according to the configurations. Suppose there are 3 families, with 3 buckets in each. Suppose the Lead has deleted the bucket family with id 2, and deleted two buckets from family 1 (for whatever reason), we may have the following (`family:index`):
- `[0:0, 0:1, 0:2  1:1, 1:3, 1:4  3:0, 3:1, 3:2]`

Suppose the system is configured (dynamic bag policy) to accept each new bag in to:
- 1 out 3 buckets in family `0`
- 2 out of 3 buckets in family `1`
- all 3 buckets in family `3`

Suppose bucket `1:3` is not accepting new bags.

Then, `dynamic:channel:1336` will be placed in to:
- 1 out of `[0:0, 0:1, 0:2]`, selected pseudo-randomly
- `[1:1, 1:4]`
- `[3:0, 3:1, 3:2]`

As with Storage Buckets, the Lead can add or remove bags from bucket as they wish (assuming it doesn't violate some other constraint).

In general, the distributor node is a little more flexible than the storage node, as errors in the configuration in the former can cause permanent data loss, whereas the "worst" case scenario with the latter is some content not being served temporarily.


# Distributor CLI
## Leader
```
yarn joystream-distributor leader --help

Commands for performing Distribution Working Group leader on-chain duties (like setting distribution module limits and parameters, assigning bags and buckets etc.)

USAGE
  $ joystream-distributor leader:COMMAND

COMMANDS
  leader:cancel-invitation           Cancel pending distribution bucket operator invitation.
  leader:create-bucket               Create new distribution bucket. Requires distribution working group leader permissions.
  leader:create-bucket-family        Create new distribution bucket family. Requires distribution working group leader permissions.
  leader:delete-bucket               Delete distribution bucket. The bucket must have no operators. Requires distribution working group leader permissions.
  leader:delete-bucket-family        Delete distribution bucket family. Requires distribution working group leader permissions.
  leader:invite-bucket-operator      Invite distribution bucket operator (distribution group worker).
  leader:remove-bucket-operator      Remove distribution bucket operator (distribution group worker).
  leader:set-bucket-family-metadata  Set/update distribution bucket family metadata.
  leader:set-buckets-per-bag-limit   Set max. distribution buckets per bag limit. Requires distribution working group leader permissions.
  leader:update-bag                  Add/remove distribution buckets from a bag.
  leader:update-bucket-mode          Update distribution bucket mode ("distributing" flag). Requires distribution working group leader permissions.
  leader:update-bucket-status        Update distribution bucket status ("acceptingNewBags" flag). Requires distribution working group leader permissions.
  leader:update-dynamic-bag-policy   Update dynamic bag creation policy (number of buckets by family that should store given dynamic bag type).
```

### cancel-invitation
```
 yarn joystream-distributor leader:cancel-invitation --help

Cancel pending distribution bucket operator invitation.

USAGE
  $ joystream-distributor leader:cancel-invitation

OPTIONS
  -B, --bucketId=bucketId      (required) Distribution bucket ID in {familyId}:{bucketIndex} format.
  -c, --configPath=configPath  [default: ./config.yml] Path to config JSON/YAML file (relative to current working directory)
  -w, --workerId=workerId      (required) ID of the invited operator (distribution group worker)
  -y, --yes                    Answer "yes" to any prompt, skipping any manual confirmations

DESCRIPTION
  Requires distribution working group leader permissions.
```

#### Example
```
yarn joystream-distributor leader:cancel-invitation -B 0:1 -w 5 -y
```
Means you want to cancel the pending invitation off worker 5 to bucket 0:1.

#### Notes
Can be used to cancel a pending invitation. Useful if:
- the worker is not responding
- the wrong worker was invited to the wrong bucket

### create-bucket
```
yarn joystream-distributor leader:create-bucket --help

Create new distribution bucket. Requires distribution working group leader permissions.

USAGE
  $ joystream-distributor leader:create-bucket

OPTIONS
  -a, --acceptingBags=(yes|no)  [default: no] Whether the created bucket should accept new bags
  -c, --configPath=configPath   [default: ./config.yml] Path to config JSON/YAML file (relative to current working directory)
  -f, --familyId=familyId       (required) Distribution bucket family id
  -y, --yes                     Answer "yes" to any prompt, skipping any manual confirmations
```

#### Example
```
yarn joystream-distributor leader:create-bucket -a yes -f 0 -y
```
Means you are creating a new bucket:
- in family `0`
- that accepts new bags
Returns the index of the bucket, eg. `1` meaning you created bucket `0:1`

#### Notes
- good practice to have new buckets not accept new bags - see [good practice](#good-practice)

### create-bucket-family
```
yarn joystream-distributor leader:create-bucket-family --help

  -c, --configPath=configPath  [default: ./config.yml] Path to config JSON/YAML file (relative to current working directory)
  -y, --yes                    Answer "yes" to any prompt, skipping any manual confirmations
```

#### Example
```
yarn joystream-distributor leader:create-bucket-family
```
Means you want to create a new bucket family. Returns the id of the new family created.

#### Notes
- bucket families by themselves have no impact on the system before buckets are created in said family

### delete-bucket
```
yarn joystream-distributor leader:delete-bucket --help

Delete distribution bucket. The bucket must have no operators. Requires distribution working group leader permissions.

USAGE
  $ joystream-distributor leader:delete-bucket

OPTIONS
  -B, --bucketId=bucketId      (required) Distribution bucket ID in {familyId}:{bucketIndex} format.
  -c, --configPath=configPath  [default: ./config.yml] Path to config JSON/YAML file (relative to current working directory)
  -y, --yes                    Answer "yes" to any prompt, skipping any manual confirmations
```

#### Example
```
yarn joystream-distributor leader:delete-bucket -B 0:0 -y
```
Means you want to delete bucket `0:0`

#### Notes
- Can delete a bucket despite it being non-empty and with an operator assigned.

### delete-bucket-family
```
yarn joystream-distributor leader:delete-bucket-family --help

Delete distribution bucket family. Requires distribution working group leader permissions.

USAGE
  $ joystream-distributor leader:delete-bucket-family

OPTIONS
  -c, --configPath=configPath  [default: ./config.yml] Path to config JSON/YAML file (relative to current working directory)
  -f, --familyId=familyId      (required) Distribution bucket family id
  -y, --yes                    Answer "yes" to any prompt, skipping any manual confirmations
```

#### Example
```
yarn joystream-distributor leader:delete-bucket-family -f 0 -y
```
Means you want to delete bucket family `0`

#### Notes
- cannot delete a bucket family that has buckets (you must delete them first)

### invite-bucket-operator
```
yarn joystream-distributor leader:invite-bucket-operator --help

Invite distribution bucket operator (distribution group worker).

USAGE
  $ joystream-distributor leader:invite-bucket-operator

OPTIONS
  -B, --bucketId=bucketId      (required) Distribution bucket ID in {familyId}:{bucketIndex} format.
  -c, --configPath=configPath  [default: ./config.yml] Path to config JSON/YAML file (relative to current working directory)
  -w, --workerId=workerId      (required) ID of the distribution group worker to invite as bucket operator
  -y, --yes                    Answer "yes" to any prompt, skipping any manual confirmations

DESCRIPTION
  The specified bucket must not have any operator currently.
     Requires distribution working group leader permissions.
```

#### Example
```
yarn joystream-distributor leader:invite-bucket-operator -B 0:1 -w 0 -y
```
Means you want to invite worker `0` to operate bucket `0:1`

#### Notes
- can invite multiple operators to the same bucket
- can invite same operator to multiple buckets

### remove-bucket-operator
```
yarn joystream-distributor leader:remove-bucket-operator --help

Remove distribution bucket operator (distribution group worker).

USAGE
  $ joystream-distributor leader:remove-bucket-operator

OPTIONS
  -B, --bucketId=bucketId      (required) Distribution bucket ID in {familyId}:{bucketIndex} format.
  -c, --configPath=configPath  [default: ./config.yml] Path to config JSON/YAML file (relative to current working directory)
  -w, --workerId=workerId      (required) ID of the operator (distribution working group worker) to remove from the bucket
  -y, --yes                    Answer "yes" to any prompt, skipping any manual confirmations

DESCRIPTION
  Requires distribution working group leader permissions.
```

#### Example
```
yarn joystream-distributor leader:remove-bucket-operator -B 0:1 -w 0 -y
```
Means you want to remove worker `0` from operating bucket `0:1`

#### Notes
- can remove an operator from buckets with data
- if the only operator of a bucket is removed, it's [good practice](#good-practice) to also turn the status and mode off.

### set-bucket-family-metadata
```
yarn joystream-distributor leader:set-bucket-family-metadata --help

Set/update distribution bucket family metadata.

USAGE
  $ joystream-distributor leader:set-bucket-family-metadata

OPTIONS
  -c, --configPath=configPath  [default: ./config.yml] Path to config JSON/YAML file (relative to current working directory)
  -f, --familyId=familyId      (required) Distribution bucket family id
  -i, --input=input            (required) Path to JSON metadata file
  -y, --yes                    Answer "yes" to any prompt, skipping any manual confirmations

DESCRIPTION
  Requires distribution working group leader permissions.
```

#### Example
With the example file below:
```
yarn joystream-distributor leader:set-bucket-family-metadata -f 0 -i /path/to/metadata.json -y
```
Means you want to set/update the metadata of bucket family `0`
```json
{
    "region": "eu-north-1",
    "description": "Central and northern Europe",
    "latencyTestTargets": [],
    "areas": [
        {
            "continentCode": "EU"
        }
    ]
}
```

#### Notes
- Go [here](https://github.com/Joystream/joystream/blob/master/distributor-node/docs/node/index.md#distributionbucketfamilymetadata) for more information.

### set-buckets-per-bag-limit
```
yarn joystream-distributor leader:set-buckets-per-bag-limit --help

Set max. distribution buckets per bag limit. Requires distribution working group leader permissions.

USAGE
  $ joystream-distributor leader:set-buckets-per-bag-limit

OPTIONS
  -c, --configPath=configPath  [default: ./config.yml] Path to config JSON/YAML file (relative to current working directory)
  -l, --limit=limit            (required) New limit value
  -y, --yes                    Answer "yes" to any prompt, skipping any manual confirmations
```

#### Example
```
yarn joystream-distributor leader:set-buckets-per-bag-limit -l 2
```
Means you want to restrict the number of buckets that holds a single bag to `2`

#### Notes
- if set to 2 as above, adding a bag two a 3rd bucket will be rejected
- however, if it was 3 and some bag(s) is already held by 3 buckets, setting it to 2 then will not change anything
  - does not work retroactively
- if a bag is held by 4 buckets, and the bag-limit is 3, you can't REMOVE a bag from a bucket before you have increased the limit first (will be fixed)


### update-bag
```
yarn joystream-distributor leader:update-bag --help

Add/remove distribution buckets from a bag.

USAGE
  $ joystream-distributor leader:update-bag

OPTIONS
  -a, --add=add
      [default: ] Index(es) (within the family) of bucket(s) to add to the bag

  -b, --bagId=bagId
      (required) Bag ID. Format: {bag_type}:{sub_type}:{id}.
          - Bag types: 'static', 'dynamic'
          - Sub types: 'static:council', 'static:wg', 'dynamic:member', 'dynamic:channel'
          - Id:
            - absent for 'static:council'
            - working group name for 'static:wg'
            - integer for 'dynamic:member' and 'dynamic:channel'
          Examples:
          - static:council
          - static:wg:storage
          - dynamic:member:4

  -c, --configPath=configPath
      [default: ./config.yml] Path to config JSON/YAML file (relative to current working directory)

  -f, --familyId=familyId
      (required) ID of the distribution bucket family

  -r, --remove=remove
      [default: ] Index(es) (within the family) of bucket(s) to remove from the bag

  -y, --yes
      Answer "yes" to any prompt, skipping any manual confirmations
```

#### Example
```
# Add
yarn joystream-distributor leader:update-bag -a 1 -b dynamic:channel:111 -f 0 -y

# Remove
yarn joystream-distributor leader:update-bag -r 1 -b dynamic:channel:111 -f 0 -y

# Mix
yarn joystream-distributor leader:update-bag -r 2 -r 3 -a 4 -b dynamic:channel:111 -f 0 -y
```
Means you want to add bag `dynamic:channel:111` to bucket `0:1`
Means you want to remove bag `dynamic:channel:111` from bucket `0:1`
Means you want to remove bag `dynamic:channel:111` from bucket `0:2` and `0:3`, and add it to bucket `0:4`

#### Notes
- useful for re-configuring the system
- cannot update multiple bags and/or add/remove to buckets across families in one transaction.

### update-bucket-mode
```
yarn joystream-distributor leader:update-bucket-mode --help

Update distribution bucket mode ("distributing" flag). Requires distribution working group leader permissions.

USAGE
  $ joystream-distributor leader:update-bucket-mode

OPTIONS
  -B, --bucketId=bucketId      (required) Distribution bucket ID in {familyId}:{bucketIndex} format.
  -c, --configPath=configPath  [default: ./config.yml] Path to config JSON/YAML file (relative to current working directory)
  -d, --mode=(on|off)          (required) Whether the bucket should be "on" (distributing) or "off" (not distributing)
  -y, --yes                    Answer "yes" to any prompt, skipping any manual confirmations
```

#### Example
```
# On
yarn joystream-distributor leader:update-bucket-mode -B 1:1 -d on

# Off
yarn joystream-distributor leader:update-bucket-mode -B 1:1 -d off
```
Means you want bucket `1:1` to start/stop distributing assets.

#### Notes
- assuming one follows [good practice](#good-practice) and set mode (and status) to off when creating a new bucket, or replacing the operator of a bucket, use this to have them distribute once the operator has deployed and tested their setup
- turn it off if a node goes down (eg. to maintenance), a worker leaves/gets fired/is reassigned


## update-bucket-status
```
yarn joystream-distributor leader:update-bucket-status --help

Update distribution bucket status ("acceptingNewBags" flag). Requires distribution working group leader permissions.

USAGE
  $ joystream-distributor leader:update-bucket-status

OPTIONS
  -B, --bucketId=bucketId       (required) Distribution bucket ID in {familyId}:{bucketIndex} format.
  -a, --acceptingBags=(yes|no)  (required) Whether the bucket should accept new bags
  -c, --configPath=configPath   [default: ./config.yml] Path to config JSON/YAML file (relative to current working directory)
  -y, --yes                     Answer "yes" to any prompt, skipping any manual confirmations
```

#### Example
```
yarn joystream-distributor leader:update-bucket-status -B 1:1 -a no -y
```
Means you want bucket `1:1` to accept new bags.

#### Notes
- assuming one follows [good practice](#good-practice) and set mode (and status) to off when creating a new bucket, or replacing the operator of a bucket, use this to have them distribute once the operator has deployed and tested their setup
- turn it off if a node goes down (eg. to maintenance), a worker leaves/gets fired/is reassigned

### update-dynamic-bag-policy
```
yarn joystream-distributor leader:update-dynamic-bag-policy --help

Update dynamic bag creation policy (number of buckets by family that should store given dynamic bag type).

USAGE
  $ joystream-distributor leader:update-dynamic-bag-policy

OPTIONS
  -c, --configPath=configPath  [default: ./config.yml] Path to config JSON/YAML file (relative to current working directory)
  -p, --policy=policy          [default: ] Key-value pair of {familyId}:{numberOfBuckets}
  -t, --type=(Member|Channel)  (required) Dynamic bag type
  -y, --yes                    Answer "yes" to any prompt, skipping any manual confirmations

DESCRIPTION
  Requires distribution working group leader permissions.
```

#### Example
```
yarn joystream-distributor leader:update-dynamic-bag-policy -t Channel -p 1:2 2:1 3:4
```
Means you want all *new* channels created, to be held by:
- 2 buckets from family `1`
- 1 buckets from family `2`
- 4 buckets from family `3`
  - assuming family 0 exists, leaving it blank is equivalent to `0:0`


Say there are 4 families (0, 1, 3), with 3 buckets in each all accepting new bags:
```
yarn joystream-distributor leader:update-dynamic-bag-policy -t Channel -p 0:3 1:2
```
Means that for all *new* channels created:
- all 3 buckets in family 0 will hold the new bag
- 2 of the 3 buckets in fam 1 will randomly be chosen for all new bags

If you add bucket to family 0 and remove one from family 1 without updating the dynamic bag policy:
- 3 of the 4 buckets in fam fam 1 will randomly be chosen for all new bags
- all buckets in family 1 will hold the new bag

#### Notes
- only affects new bags created
- will be "overridden" by `leader:set-buckets-per-bag-limit`, and/or the number of buckets (accepting new bags)
  - eg. if the bag limit is 3,
- if there are
- even if no worker has been invited to a bucket (nor accepted, nor metadata set) it will ("dynamically") accept new bags as long as the bucket exists,

## Operator

```
yarn joystream-distributor operator --help

Commands for performing node operator (Distribution Working Group worker) on-chain duties (like accepting bucket invitations, setting node metadata)

USAGE
  $ joystream-distributor operator:COMMAND

COMMANDS
  operator:accept-invitation  Accept pending distribution bucket operator invitation.
  operator:set-metadata       Set/update distribution bucket operator metadata.
```

### accept-invitation
```
yarn joystream-distributor operator:accept-invitation --help

Accept pending distribution bucket operator invitation.

USAGE
  $ joystream-distributor operator:accept-invitation

OPTIONS
  -B, --bucketId=bucketId      (required) Distribution bucket ID in {familyId}:{bucketIndex} format.
  -c, --configPath=configPath  [default: ./config.yml] Path to config JSON/YAML file (relative to current working directory)
  -w, --workerId=workerId      (required) ID of the invited operator (distribution group worker)
  -y, --yes                    Answer "yes" to any prompt, skipping any manual confirmations

DESCRIPTION
  Requires the invited distribution group worker role key
```

#### Example
```
yarn joystream-distributor operator:accept-invitation -B 0:1 -w 0 -y
```
Means you (worker `0`) wants to accept a pending invitation from the Lead to bucket `0:1`

#### Notes

### set-metadata
```
yarn joystream-distributor operator:set-metadata --help

Set/update distribution bucket operator metadata.

USAGE
  $ joystream-distributor operator:set-metadata

OPTIONS
  -B, --bucketId=bucketId      (required) Distribution bucket ID in {familyId}:{bucketIndex} format.
  -c, --configPath=configPath  [default: ./config.yml] Path to config JSON/YAML file (relative to current working directory)
  -e, --endpoint=endpoint      Root distribution node endpoint
  -i, --input=input            Path to JSON metadata file
  -w, --workerId=workerId      (required) ID of the operator (distribution group worker)
  -y, --yes                    Answer "yes" to any prompt, skipping any manual confirmations

DESCRIPTION
  Requires active distribution bucket operator worker role key.
```

#### Example
With the example file below:
```
yarn joystream-distributor operator:set-metadata -B 0:1 -w 0 -i /path/to/metadata.json
```
Means you (worker `0`) wants to set/update the metadata of bucket `0:1`
```json
{
  "endpoint": "https://<your.cool.url>/distributor/",
  "location": {
    "countryCode": "SG",
    "city": "Singapore",
    "coordinates": {
      "latitude": 1,
      "longitude": 104
    }
  },
  "extra": "Welcome to Joystream - Singapore branch!"
}
```

#### Notes
- Go [here](https://github.com/Joystream/joystream/blob/master/distributor-node/docs/node/index.md#distributionbucketfamilymetadata) for more information.

## Initial Configurations - Giza
When `giza` went live, some initial configurations was set for the migration script:
```
# Create Families
yarn joystream-distributor leader:create-bucket-family -y -c config-lead.yml
yarn joystream-distributor leader:create-bucket-family -y -c config-lead.yml

# Set family bucket metadata
yarn joystream-distributor leader:set-bucket-family-metadata -f 0 -i /path/to/fam-bucket-0.json -y -c config-lead.yml
yarn joystream-distributor leader:set-bucket-family-metadata -f 1 -i /path/to/fam-bucket-1.json -y -c config-lead.yml

# Create 4 buckets, 1 in fam 0, 3 in fam 1. All to accept new bags
yarn joystream-distributor leader:create-bucket -f 0 -a yes -y -c config-lead.yml
yarn joystream-distributor leader:create-bucket -f 1 -a yes -y -c config-lead.yml
yarn joystream-distributor leader:create-bucket -f 1 -a yes -y -c config-lead.yml
yarn joystream-distributor leader:create-bucket -f 1 -a yes -y -c config-lead.yml

# Invite bucket operator
yarn joystream-distributor leader:invite-bucket-operator -B 0:0 -w 1 -y -c config-lead.yml

# Set dynamic bag policy
yarn joystream-distributor leader:update-dynamic-bag-policy -t Channel -p 0:1 1:1 -y -c config-lead.yml

# Set initial buckets per bag limit
yarn joystream-distributor leader:set-buckets-per-bag-limit -l 2 -y -c config-lead.yml

# Turn off distribution on non-jsg nodes:
yarn joystream-distributor leader:update-bucket-mode -B 1:0 -d off -y -c config-lead.yml
yarn joystream-distributor leader:update-bucket-mode -B 1:1 -d off -y -c config-lead.yml
yarn joystream-distributor leader:update-bucket-mode -B 1:2 -d off -y -c config-lead.yml
```

Where,
- `fam-bucket-0.json`:
```json
{
  "region": "eu-0",
  "description": "Temporary Distributor",
  "latencyTestTargets": [
  ],
  "areas": [
    {
      "continentCode": "EU"
    }
  ]
}
```
- `fam-bucket-1.json`:
```json
{
  "region": "eu-north-1",
  "description": "Central and northern Europe",
  "latencyTestTargets": [
  ],
  "areas": [
    {
      "continentCode": "EU"
    }
  ]
}
```


## Good Practice

### Bucket Families
The main purpose of bucket families is to optimize the user experience by reducing the latency between a consumer and the distributor as much as possible without incurring too significant costs. This means having bucket families represent regions across the world, optimizing for:
- costs
- latency (all over the world)
- usage (in geographical regions)

As an example of the thought process:
Canada, the worlds second largest in terms of area, is over 3x the size of the Eurozone. If you go by latency alone, that would imply 3 bucket families in Canada and one in the Eurozone. However, the population of Canada is ~40M, whereas the Eurozone is the home of ~340M people. Furthermore, "almost everyone" in Canada lives practically on the US border, and most of them in the east. So it makes no economic sense to optimize that way.

A Lead will have to consider primarily the resources available, the current usage data, where one would expect, or target new users and how good/bad the quality of service is in the respective regions before deploying a coherent system of families and buckets.

Currently, we believe most users are located in the eurozone and the CIS. If one were to assume one could only have to families, it *may* make sense to deploy a bucket in the north-eastern parts of the eurozone (eg. Germany or Poland), and one in central Russia. The Former would provide good coverage across the Eurozone and western parts of the CIS, whereas the latter would provide decent coverage across the "rest of" the CIS, and large parts of Asia.

However, it may be better to place one bucket in North America, covering a large potential market there, and one somewhere like the Ukraine, covering the "current" userbase, assuming the latency wouldn't be too bad.

Note that this is just an example. It is certainly not true that we can only have two families, and it may not be true that a bucket in the Ukraine would be good enough for the existing market.

Further, one would have to consider the coverage of specific bags. Suppose now we have 5 families (number of buckets in each family) covering (note that this is not necessarily a good setup!):
1. Europe (5)
2. America (3)
3. Asia (5)
4. Africa (1)
5. Australia (1)

A bag (eg. channel) with content in Russian may be very poplar in Europe and Asia, but not in the other regions. It may then make sense to have it be served by 2 or 3 buckets in Europe and Asia, but not at all in the remaining regions. A user in other regions will still be able to play the content, but it will be slightly slower to render/play.

### New Buckets
When creating a new bucket, it is wise to NOT have it distribute bags right away. The reason is that once you enable this (and metadata is set), the player will assume it can serve content. Unless the configurations are correct, this will not be true.

Although less important, assuming your dynamic bag policy is robust (meaning the new bucket can't be the only bucket accepting any new bags), it may also be advisable to disable `--acceptingBags`.

Steps:

```
# create a new bucket in family 1, but don't have it accept new bags:
$ yarn joystream-distributor leader:create-bucket -a no -f 1 -y
# It will return the new bucket index in the family (let's say it's 3)

# set the -mode to off:
$ yarn joystream-distributor leader:update-bucket-mode -B 1:3 -d off

# invite operator with --workerId 5, to this bag:
$ yarn joystream-distributor leader:invite-bucket-operator -B 1:3 -w 5 -y

# await the operator having first accepted the invitation and set their metadata. Then have them start their node. Once they have, check that their api status URL looks good. Then, turn it on/on

$ yarn joystream-distributor leader:update-bucket-status -B 1:3 -a yes -y
$ yarn joystream-distributor leader:update-bucket-mode -B 1:3 -d on

# add some bag (in this example, bag 1337) to it:
$ yarn joystream-distributor leader:update-bag -f 1 -a 3 -b dynamic:channel:1337 -y

```
Try getting an asset in said bag from it. If it works -> all is well. If not, disable status and troubleshoot with the operator. If you are able to quickly fix the issue, enable the status again. If not disable mode.
Consider:
- giving the operator more time; or
- fire the operator (requires you to remove all bags first)

### Dynamic Bag Creation Policy
This number should be a function of a multiple parameters. When all the features of the system are utilized in the player, meaning the consumer app will consider which bucket family has the best latency to the consumer trying to fetch an asset, it would be even more complicated. As we are discussing the best practises, we will pretend this is still true.

Suppose we have the same setup as in [Bucket Families](#bucket-families-1).
One setup could be:

```
$ yarn joystream-distributor leader:update-dynamic-bag-policy -t Channel -p 0:1 1:1 2:1 3:1 4:1 5:1
```
Meaning all new bags are uploaded to a single (random) bucket in each family (if they all accept new bags). However, this would obviously mean that the bucket in australia would hold be assigned 5 times more (new) content than their counterparts in Asia and Europe.

As a distribution bucket does not need to hold all data, this may be fine, but at some point, it's not clear that a bag that is cached in an asian bucket wouldn't serve an australian user faster assuming the bucket isn't cached in that bucket.

Point is, it *may* be better to manually add bags that are popular to the australian, so it can focus on the popular content there. At the time, we don't know what would be the ideal setup here. It will have to be solved by extensive testing and big data.

I am sure many of you have noticed that even on Youtube, an "obscure" video may take a while to start playing, whereas the content on the front page will play right away.

### When to Update Bags
There are many situations where the Lead will have to move bags around. The four we assume will be most common:
1. A video goes "viral". Say a new channel, or one that hasn't had any videos watched more than a few times a day all of the sudden uploads an extremely popular video. If the Lead had done a "good" job optimizing before, the bag (channel) was likely held by a relatively small amount of buckets before. That would mean a user playing this video would frequently have a bad experience, unless the Lead manually updates the bag by adding it to more buckets.
2. After the "hype" around said video goes down, the channel is unable to maintain it's recent popularity, and their new videos gets fewer and fewer views. This would be a good time to scale back on the amount of buckets that holds that bag.
3. An worker quits or gets fired. In an extreme example, a couple of bags were only held by a/the bucket the worker operated. Unless the bucket can be assigned to another worker right away, this means you have add these bags (and maybe some others as well) to another bucket ASAP.
4. A channel uploads a new video every Tuesday, that is very popular in some region. For some reason, "everyone" wants to watch it right away, and as the days goes by, it gets exponentially fewer views. If that becomes the norm, it would make sense to add the bag to a good number of buckets each on that day, before gradually removing them. Ideally, one would even ask the creator to give a heads up an hour or two before they publish. The same would of course be true for a channel that uploads extremely popular videos rarely and at unpredictable times.
