<p align="center"><img src="img/storage-lead.svg"></p>

<div align="center">
  <h4>This is a guide to working as the Storage Provider Lead on the latest
  <a href="https://testnet.joystream.org/">Joystream Testnet</a>.<h4>
</div>


Table of Contents
==
<!-- TOC START min:1 max:3 link:true asterisk:false update:true -->
  - [The Storage and Distribution System](#the-storage-and-distribution-system)
    - [Bags](#bags)
    - [Storage Buckets](#storage-buckets)
    - [Distribution Buckets](#distribution-buckets)
- [Storage CLI](#storage-cli)
  - [Leader](#leader)
    - [cancel-invite](#cancel-invite)
    - [create-bucket](#create-bucket)
    - [delete-bucket](#delete-bucket)
    - [invite-operator](#invite-operator)
    - [remove-operator](#remove-operator)
    - [set-bucket-limits](#set-bucket-limits)
    - [set-global-uploading-status](#set-global-uploading-status)
    - [update-bag](#update-bag)
    - [update-bag-limit](#update-bag-limit)
    - [update-blacklist](#update-blacklist)
    - [update-bucket-status](#update-bucket-status)
    - [update-data-fee](#update-data-fee)
    - [update-dynamic-bag-policy](#update-dynamic-bag-policy)
    - [update-voucher-limits](#update-voucher-limits)
  - [Operator](#operator)
    - [Accept Invitation](#accept-invitation)
    - [Set Metadata](#set-metadata)
  - [Initial Configurations - Giza](#initial-configurations---giza)
  - [Good Practice](#good-practice)
    - [New Buckets](#new-buckets)
    - [Dynamic Bag Creation](#dynamic-bag-creation)
    - [When to Disable a Bucket from Accepting new Bags](#when-to-disable-a-bucket-from-accepting-new-bags)
    - [When to Update Bags](#when-to-update-bags)
<!-- TOC END -->


## The Storage and Distribution System
Storage and Distribution system is well documented on github. A simple version will be outlined here.

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
A bucket family is a collection of distribution buckets associated with a geographical region (eg. from larger to smaller: World, Europe, Norther Europe, Scandinavia, Norway, Oslo). This is meant to reduce latency when serving content.

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
- 1 out of `[0:0, 0:1, 0:2]`, selected psuedo-randomly
- `[1:1, 1:4]`
- `[3:0, 3:1, 3:2]`

As with Storage Buckets, the Lead can add or remove bags from bucket as they wish (assuming it doesn't violate some other constraint).

In general, the distributor node is a little more flexible than the storage node, as errors in the configuration in the former can cause permanent data loss, whereas the "worst" case scenario with the latter is some content not being served temporarily.

# Storage CLI
## Leader
```
COMMANDS
  leader:cancel-invite                Cancel a storage bucket operator invite. Requires storage working group leader permissions.
  leader:create-bucket                Create new storage bucket. Requires storage working group leader permissions.
  leader:delete-bucket                Delete a storage bucket. Requires storage working group leader permissions.
  leader:invite-operator              Invite a storage bucket operator. Requires storage working group leader permissions.
  leader:remove-operator              Remove a storage bucket operator. Requires storage working group leader permissions.
  leader:set-bucket-limits            Set VoucherObjectsSizeLimit and VoucherObjectsNumberLimit for the storage bucket.
  leader:set-global-uploading-status  Set global uploading block. Requires storage working group leader permissions.
  leader:update-bag                   Add/remove a storage bucket from a bag (adds by default).
  leader:update-bag-limit             Update StorageBucketsPerBagLimit variable in the Joystream node storage.
  leader:update-blacklist             Add/remove a content ID from the blacklist (adds by default).
  leader:update-bucket-status         Update storage bucket status (accepting new bags).
  leader:update-data-fee              Update data size fee. Requires storage working group leader permissions.
  leader:update-dynamic-bag-policy    Update number of storage buckets used in the dynamic bag creation policy.
  leader:update-voucher-limits        Update VoucherMaxObjectsSizeLimit and VoucherMaxObjectsNumberLimit for the Joystream node storage.
```

### cancel-invite
```
yarn storage-node leader:cancel-invite --help

  -h, --help                   show CLI help
  -i, --bucketId=bucketId      (required) Storage bucket ID
  -k, --keyFile=keyFile        Key file for the account. Mandatory in non-dev environment.
  -m, --dev                    Use development mode
  -p, --password=password      Key file password (optional). Could be overriden by ACCOUNT_PWD environment variable.
  -u, --apiUrl=apiUrl          [default: ws://localhost:9944] Runtime API URL. Mandatory in non-dev environment.
  -y, --accountUri=accountUri  Account URI (optional). Has a priority over the keyFile and password flags. Could be overriden by ACCOUNT_URI environment variable.
```

#### Example
```
yarn storage-node leader:cancel-invite -i 2 -k /path/to/storage-lead-role-key.json
```

#### Notes
Can be used to cancel an invitation. Useful if:
- the worker is not responding
- the wrong worker was invited

### create-bucket
```
yarn storage-node leader:create-bucket --help

  -a, --allow                  Accepts new bags
  -h, --help                   show CLI help
  -i, --invited=invited        Invited storage operator ID (storage WG worker ID)
  -k, --keyFile=keyFile        Key file for the account. Mandatory in non-dev environment.
  -m, --dev                    Use development mode
  -n, --number=number          Storage bucket max total objects number
  -p, --password=password      Key file password (optional). Could be overriden by ACCOUNT_PWD environment variable.
  -s, --size=size              Storage bucket max total objects size
  -u, --apiUrl=apiUrl          [default: ws://localhost:9944] Runtime API URL. Mandatory in non-dev environment.
  -y, --accountUri=accountUri  Account URI (optional). Has a priority over the keyFile and password flags. Could be overriden by ACCOUNT_URI environment variable.
```
#### Example
```
yarn storage-node leader:create-bucket -a -i 1 -k /path/to/storage-lead-role-key.json -n 1000 -s 100000000000
```
Means you are creating a new bucket, set to:
- allow new bags (should be disabled on new buckets)
- inviting worker with ID 1
- hold up to 1000 files
- store up to 100 GB of files

#### Notes
- Not possible for `-s` or `-n` to be larger than their ["global" limits](update-voucher-limit).

### delete-bucket
```
 yarn storage-node leader:delete-bucket --help

USAGE
  $ storage-node leader:delete-bucket

OPTIONS
  -h, --help                   show CLI help
  -i, --bucketId=bucketId      (required) Storage bucket ID
  -k, --keyFile=keyFile        Key file for the account. Mandatory in non-dev environment.
  -m, --dev                    Use development mode
  -p, --password=password      Key file password (optional). Could be overriden by ACCOUNT_PWD environment variable.
  -u, --apiUrl=apiUrl          [default: ws://localhost:9944] Runtime API URL. Mandatory in non-dev environment.
  -y, --accountUri=accountUri  Account URI (optional). Has a priority over the keyFile and password flags. Could be overriden by ACCOUNT_URI environment variable.
```

#### Example
```
yarn storage-node leader:delete-bucket -i 1 -k /path/to/storage-lead-role-key.json
```
Means you want to delete bucket 1.

#### Notes
- Not possible to delete non-empty bucket - even if not assigned to anyone.
- Not possible to delete a bucket that is assigned to a worker.

### invite-operator
```
Invite a storage bucket operator. Requires storage working group leader permissions.

USAGE
  $ storage-node leader:invite-operator

OPTIONS
  -h, --help                   show CLI help
  -i, --bucketId=bucketId      (required) Storage bucket ID
  -k, --keyFile=keyFile        Key file for the account. Mandatory in non-dev environment.
  -m, --dev                    Use development mode
  -p, --password=password      Key file password (optional). Could be overriden by ACCOUNT_PWD environment variable.
  -u, --apiUrl=apiUrl          [default: ws://localhost:9944] Runtime API URL. Mandatory in non-dev environment.
  -w, --operatorId=operatorId  (required) Storage bucket operator ID (storage group worker ID)
  -y, --accountUri=accountUri  Account URI (optional). Has a priority over the keyFile and password flags. Could be overriden by ACCOUNT_URI environment variable.
```

#### Example
```
yarn storage-node leader:invite-operator -i 1 -k /path/to/storage-lead-role-key.json -w 3
```
Means you are inviting worker 3, to operate bucket 1.

#### Notes
- Can not invite multiple people, or invite to a bucket when it is assigned.
- Can invite an SP to multiple buckets

### remove-operator
```
yarn storage-node leader:remove-operator --help

Remove a storage bucket operator. Requires storage working group leader permissions.

USAGE
  $ storage-node leader:remove-operator

OPTIONS
  -h, --help                   show CLI help
  -i, --bucketId=bucketId      (required) Storage bucket ID
  -k, --keyFile=keyFile        Key file for the account. Mandatory in non-dev environment.
  -m, --dev                    Use development mode
  -p, --password=password      Key file password (optional). Could be overriden by ACCOUNT_PWD environment variable.
  -u, --apiUrl=apiUrl          [default: ws://localhost:9944] Runtime API URL. Mandatory in non-dev environment.
  -y, --accountUri=accountUri  Account URI (optional). Has a priority over the keyFile and password flags. Could be overriden by ACCOUNT_URI environment variable.
```

#### Example
```
yarn storage-node leader:remove-operator -k /path/to/storage-lead-role-key.json -i 4
```
Means you are removing the operator of bucket 4.

#### Notes

### set-bucket-limits

```
yarn storage-node leader:set-bucket-limits --help

  -h, --help                   show CLI help
  -i, --bucketId=bucketId      (required) Storage bucket ID
  -k, --keyFile=keyFile        Key file for the account. Mandatory in non-dev environment.
  -m, --dev                    Use development mode
  -o, --objects=objects        (required) New 'voucher object number limit' value
  -p, --password=password      Key file password (optional). Could be overriden by ACCOUNT_PWD environment variable.
  -s, --size=size              (required) New 'voucher object size limit' value
  -u, --apiUrl=apiUrl          [default: ws://localhost:9944] Runtime API URL. Mandatory in non-dev environment.
  -y, --accountUri=accountUri  Account URI (optional). Has a priority over the keyFile and password flags. Could be overriden by ACCOUNT_URI environment variable.
```

#### Example
```
yarn storage-node leader:set-bucket-limits -i 0 -o 5000 -s 1000000000000 -k /path/to/storage-lead-role-key.json
```
Means you want to change the bucket limits of bucket 1 to:
- hold up to 1000 files
- store up to 100 GB of files

#### Notes
- Can not be set lower than what the bucket currently contains.
- Not possible for `-s` or `-o` to be larger than their ["global" limits](update-voucher-limit).

### set-global-uploading-status
```
 yarn storage-node leader:set-global-uploading-status --help

Set global uploading block. Requires storage working group leader permissions.

USAGE
  $ storage-node leader:set-global-uploading-status

OPTIONS
  -h, --help                   show CLI help
  -k, --keyFile=keyFile        Key file for the account. Mandatory in non-dev environment.
  -m, --dev                    Use development mode
  -p, --password=password      Key file password (optional). Could be overriden by ACCOUNT_PWD environment variable.
  -s, --set=(on|off)           (required) Sets global uploading block (on/off).
  -u, --apiUrl=apiUrl          [default: ws://localhost:9944] Runtime API URL. Mandatory in non-dev environment.

  -y, --accountUri=accountUri  Account URI (optional). Has a priority over the keyFile and password flags. Could be overriden by
                               ACCOUNT_URI environment variable.
```

#### Notes
Useful if you want to stop all uploads.

### update-bag
```
yarn storage-node leader:update-bag --help

Add/remove a storage bucket from a bag (adds by default).

USAGE
  $ storage-node leader:update-bag

OPTIONS
  -a, --add=add
      [default: ] ID of a bucket to add to bag

  -h, --help
      show CLI help

  -i, --bagId=bagId
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

  -k, --keyFile=keyFile
      Key file for the account. Mandatory in non-dev environment.

  -m, --dev
      Use development mode

  -p, --password=password
      Key file password (optional). Could be overriden by ACCOUNT_PWD environment variable.

  -r, --remove=remove
      [default: ] ID of a bucket to remove from bag

  -u, --apiUrl=apiUrl
      [default: ws://localhost:9944] Runtime API URL. Mandatory in non-dev environment.

  -y, --accountUri=accountUri
      Account URI (optional). Has a priority over the keyFile and password flags. Could be overriden by ACCOUNT_URI environment variable.
```

#### Example
```
# Remove
yarn storage-node leader:update-bag -i dynamic:channel:191 -k /path/to/storage-lead-role-key.json -r 1

# Add
yarn storage-node leader:update-bag -i dynamic:channel:191 -k /path/to/storage-lead-role-key.json -a 1
```
Means you are either removing, or adding, bag `dynamic:channel:191` from bucket 1.


#### Notes
- Can only add OR remove 1 at the time
- Can remove a bag, even if the bucket is the only one assigned
- Can add a bag, that no else has
- Can add a bag to a bucket w/o metadata
- Can NOT add a bag too MORE buckets than what is set in `updage-bucket-limit`
- Can add a bag to a bucket with no operators, or invited operators
- can NOT add a bag if `update-bucket-status` is set to `off`

### update-bag-limit
```
yarn storage-node leader:update-bag-limit --help

Update StorageBucketsPerBagLimit variable in the Joystream node storage.

USAGE
  $ storage-node leader:update-bag-limit

OPTIONS
  -h, --help                   show CLI help
  -k, --keyFile=keyFile        Key file for the account. Mandatory in non-dev environment.
  -l, --limit=limit            (required) New StorageBucketsPerBagLimit value
  -m, --dev                    Use development mode
  -p, --password=password      Key file password (optional). Could be overriden by ACCOUNT_PWD environment variable.
  -u, --apiUrl=apiUrl          [default: ws://localhost:9944] Runtime API URL. Mandatory in non-dev environment.
  -y, --accountUri=accountUri  Account URI (optional). Has a priority over the keyFile and password flags. Could be overriden by ACCOUNT_URI environment variable.
```

#### Example
```
yarn storage-node leader:update-bag-limit -l 5 -k /path/to/storage-lead-role-key.json
```
Means you are changing the "bag limit", namely the maximum number of buckets a single bag can be in, to 5.

#### Notes
- Runtime Config 4<value<20
- If the `bag` that is held by the MOST SPs is `n`, then `update-bag-limit` can't be set lower than `n`.

### update-blacklist
```
yarn storage-node leader:update-blacklist --help

Add/remove a content ID from the blacklist (adds by default).

USAGE
  $ storage-node leader:update-blacklist

OPTIONS
  -a, --add=add                [default: ] Content ID to add
  -h, --help                   show CLI help
  -k, --keyFile=keyFile        Key file for the account. Mandatory in non-dev environment.
  -m, --dev                    Use development mode
  -p, --password=password      Key file password (optional). Could be overriden by ACCOUNT_PWD environment variable.
  -r, --remove=remove          [default: ] Content ID to remove
  -u, --apiUrl=apiUrl          [default: ws://localhost:9944] Runtime API URL. Mandatory in non-dev environment.
  -y, --accountUri=accountUri  Account URI (optional). Has a priority over the keyFile and password flags. Could be overriden by ACCOUNT_URI environment variable.
```

#### Example
```
yarn storage-node leader:update-blacklist -a gW2PvsndeSMCqfVF8j5imGE6a8WSLNC1fS8d8TuToUbsMM -k /path/to/storage-lead-role-key.json
```
Means that if someone tries to upload a file with the `ipfsHash` above, it will get rejected. Any channel or video that gets created or changed to have that asset, will see the transaction rejected.

#### Notes
- Takes ipfs hash as argument
- Does not remove existing files with said hash, only blocks adding it.

### update-bucket-status
```
yarn storage-node leader:update-bucket-status --help

Update storage bucket status (accepting new bags).

USAGE
  $ storage-node leader:update-bucket-status

OPTIONS
  -h, --help                   show CLI help
  -i, --bucketId=bucketId      (required) Storage bucket ID
  -k, --keyFile=keyFile        Key file for the account. Mandatory in non-dev environment.
  -m, --dev                    Use development mode
  -p, --password=password      Key file password (optional). Could be overriden by ACCOUNT_PWD environment variable.
  -s, --set=(on|off)           (required) Sets 'accepting new bags' parameter for the bucket (on/off).
  -u, --apiUrl=apiUrl          [default: ws://localhost:9944] Runtime API URL. Mandatory in non-dev environment.
  -y, --accountUri=accountUri  Account URI (optional). Has a priority over the keyFile and password flags. Could be overriden by ACCOUNT_URI environment variable.
```

#### Example
```
# stop accepting
yarn storage-node leader:update-bucket-status -k /path/to/storage-lead-role-key.json -i 0 -s off

# start accepting
yarn storage-node leader:update-bucket-status -k /path/to/storage-lead-role-key.json -i 0 -s on
```
Means you want bucket 0 to stop/start accepting new bags. Useful if the bucket is (getting) full.

#### Notes
- set to off to stop accepting NEW bags.
- set back to one
- if you try to add a new bag to a bucket set to off using [update-bag](#update-bag), it will fail
- will not randomly be assigned new bags

### update-data-fee
```
yarn storage-node leader:update-data-fee --help

Update data size fee. Requires storage working group leader permissions.

USAGE
  $ storage-node leader:update-data-fee

OPTIONS
  -f, --fee=fee                (required) New data size fee
  -h, --help                   show CLI help
  -k, --keyFile=keyFile        Key file for the account. Mandatory in non-dev environment.
  -m, --dev                    Use development mode
  -p, --password=password      Key file password (optional). Could be overriden by ACCOUNT_PWD environment variable.
  -u, --apiUrl=apiUrl          [default: ws://localhost:9944] Runtime API URL. Mandatory in non-dev environment.
  -y, --accountUri=accountUri  Account URI (optional). Has a priority over the keyFile and password flags. Could be overriden by ACCOUNT_URI environment variable.

yarn storage-node leader:update-data-fee -f 1 -k /path/to/storage-lead-role-key.json
```

#### Example
```
yarn storage-node leader:update-data-fee -f 1 -k /path/to/storage-lead-role-key.json
```
Means you want to change the fees to 1 tJOY per MB. Currently, this is 0.

#### Notes
- Please do not do this (yet), as these fees will rack up and discourage content creation.
- Unlike the `deletionPrize` (which are reimbursed when you delete an asset) the fees are burned.

### update-dynamic-bag-policy
```
yarn storage-node leader:update-dynamic-bag-policy --help

Update number of storage buckets used in the dynamic bag creation policy.

USAGE
  $ storage-node leader:update-dynamic-bag-policy

OPTIONS
  -h, --help                      show CLI help
  -k, --keyFile=keyFile           Key file for the account. Mandatory in non-dev environment.
  -m, --dev                       Use development mode
  -n, --number=number             (required) New storage buckets number
  -p, --password=password         Key file password (optional). Could be overriden by ACCOUNT_PWD environment variable.
  -t, --bagType=(Channel|Member)  (required) Dynamic bag type (Channel, Member).
  -u, --apiUrl=apiUrl             [default: ws://localhost:9944] Runtime API URL. Mandatory in non-dev environment.
  -y, --accountUri=accountUri     Account URI (optional). Has a priority over the keyFile and password flags. Could be overriden by ACCOUNT_URI environment variable.
```

#### Example
```
yarn storage-node leader:update-dynamic-bag-policy -n 2 -t Channel -k /path/to/storage-lead-role-key.json
```
Means that a new upload, from a new channel, will be assigned to 1 ("random", if multiple are available) SP only. (`-t Member` isn't relevant)

#### Notes
- `-n 0` means no uploads new bags can be created

### update-voucher-limits
```
yarn storage-node leader:update-voucher-limits --help

  -h, --help                   show CLI help
  -k, --keyFile=keyFile        Key file for the account. Mandatory in non-dev environment.
  -m, --dev                    Use development mode
  -o, --objects=objects        (required) New 'max voucher object number limit' value
  -p, --password=password      Key file password (optional). Could be overriden by ACCOUNT_PWD environment variable.
  -s, --size=size              (required) New 'max voucher object size limit' value
  -u, --apiUrl=apiUrl          [default: ws://localhost:9944] Runtime API URL. Mandatory in non-dev environment.
  -y, --accountUri=accountUri  Account URI (optional). Has a priority over the keyFile and password flags. Could be overriden by ACCOUNT_URI environment variable.
```

#### Example
```
yarn storage-node leader:update-voucher-limits -o 1000 -s 100000000000 -k /path/to/storage-lead-role-key.json
```
Means setting the Global limits to:
- hold up to 1000 files
- store up to 100 GB of files

#### Notes
- If the sum of all assets in all bags exceeds this number, no uploads is accepted
- A single bucket can not have limits that exceeds this number

## Operator
```
  operator:accept-invitation  Accept pending storage bucket invitation.
  operator:set-metadata       Set metadata for the storage bucket.
```

### Accept Invitation

```
yarn run storage-node operator:accept-invitation --help

Accept pending storage bucket invitation.

USAGE
  $ storage-node operator:accept-invitation

OPTIONS
  -h, --help                                     show CLI help
  -i, --bucketId=bucketId                        (required) Storage bucket ID
  -k, --keyFile=keyFile                          Key file for the account. Mandatory in non-dev environment.
  -m, --dev                                      Use development mode
  -p, --password=password                        Key file password (optional). Could be overriden by ACCOUNT_PWD environment variable.
  -t, --transactorAccountId=transactorAccountId  (required) Transactor account ID (public key)
  -u, --apiUrl=apiUrl                            [default: ws://localhost:9944] Runtime API URL. Mandatory in non-dev environment.
  -w, --workerId=workerId                        (required) Storage operator worker ID
  -y, --accountUri=accountUri                    Account URI (optional). Has a priority over the keyFile and password flags. Could be overriden by ACCOUNT_URI environment variable.
```

#### Example
```
yarn run storage-node operator:accept-invitation -i 3 -k /path/to/storage-worker-role-key.json -w 1 -t 5StorageOperatorKey
```
Means that worker 1 accepts the invitation to bucket 3, and sets their operator key to `5StorageOperatorKey`

#### Notes
- Can accept an invite to more buckets, having already accepted to another?
- The operator must run their storage node with this key as an argument to run

### Set Metadata
```
yarn run storage-node operator:set-metadata --help

Set metadata for the storage bucket.

USAGE
  $ storage-node operator:set-metadata

OPTIONS
  -e, --endpoint=endpoint      Root distribution node endpoint
  -h, --help                   show CLI help
  -i, --bucketId=bucketId      (required) Storage bucket ID
  -j, --jsonFile=jsonFile      Path to JSON metadata file
  -k, --keyFile=keyFile        Key file for the account. Mandatory in non-dev environment.
  -m, --dev                    Use development mode
  -p, --password=password      Key file password (optional). Could be overriden by ACCOUNT_PWD environment variable.
  -u, --apiUrl=apiUrl          [default: ws://localhost:9944] Runtime API URL. Mandatory in non-dev environment.
  -w, --operatorId=operatorId  (required) Storage bucket operator ID (storage group worker ID)
  -y, --accountUri=accountUri  Account URI (optional). Has a priority over the keyFile and password flags. Could be overriden by ACCOUNT_URI environment variable.
```
#### Example
```
yarn run storage-node operator:set-metadata -i 3 -k /path/to/storage-worker-role-key.json -w 1 -j /path/to/metadata.json
```
Means that worker 1 is setting the metadata to bucket 3.

#### Notes
Example file:
```json
{
  "endpoint": "https://giza-staging-b.joystream.app/storage/",
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


## Initial Configurations - Giza
When `giza` went live, some initial configurations was set for the migration script:
```
# Set "global" storage limits to 2000 GB and 200000 files:
yarn storage-node leader:update-voucher-limits -s 2000000000000 -o 20000 -k /path/to/storage-lead-role-key.json

# Create a bucket that accepts uploads, should hold everything and invite worker 10 (Jsgenesis):
yarn storage-node leader:create-bucket -a -i 10 -s 2000000000000 -n 20000 -k /path/to/storage-lead-role-key.json

# Update/set the dynamic bag policy:
yarn storage-node leader:update-dynamic-bag-policy -t Channel -n 1 -k /path/to/storage-lead-role-key.json

# Update the bag limit:
yarn storage-node leader:update-bag-limit -l 5 -k /path/to/storage-lead-role-key.json
```

This was a requirement for the content migration to work, as all content (and uploads) to go to the same bucket. (Not the only possible configuration, but a safe and easy one).


## Good Practice
### New Buckets
When creating a new bucket, it is wise to NOT have it allow new bags right away. The reason is that once you do that, it will be assumed to be ready to accept uploads right away. Which, of course is impossible before the invitation is accepted (no operator) and metadata is set (no url to upload to).

```
# avoid the -a
$ yarn storage-node leader:create-bucket -i 1 -k -n 1000 -s 100000000000 /path/to/storage-lead-role-key.json
```

Once the operator has accepted their invititation and set their metadata, you can check the url and confirm the node is running before you:
```
$ yarn storage-node leader:update-bucket-status -i 1 -s on -k /path/to/storage-lead-role-key.json
```

### Dynamic Bag Creation
This number should be a function of a multiple parameters.
- The ideal replication factor, `k_r`, meaning the number of backups we want of each bag held in storage.
- The number of buckets operated `b_a`.
- The capacity each of those `i` buckets are `b_i_c`.
To a lesser extent, and/or if we consider costs:
- How frequently storage nodes goes down, workers leave their role without "sufficiently long" unstaking periods, or how many workers are "close" to getting fired for underperfomance.
- How quickly we can deploy extra capacity, either by increasing the number of buckets, or expanding them.
- How much extra capacity we anticipate is needed in the near future (at some point, this will be based on statisics).
- How costly extra storage is.
- On a network with storage fees, (which the Lead can control), this can be scaled up or down to reduce or increase demand. (The storage lead will also have to pay fees for their transaction, although we may assume this is covered by the platform, it's still an expense.)
- Probably lots of other minor things as well, especially on an incentivezed testnet, where JSG will reward the Lead and Workers based on how well they reach certain goals.

It will be hard to create a simple formulae to calculate this, so we need to cut some corners in this simple example. Suppose:
- `k_r` = 4
- `b_a`= 10
- All i buckets, are at less than 60% capacity.

At this point, it seems obvious are `k_r` should be at least 4. We have spare capacity, so we can use 5. It becomes a question of probabilites, workload and costs.
As an example, if we chose 5, it's rather easy to remove bags from a bucket if we need extra capacity. If we chose 4, a worker leaves, and we don't have anyone that can take over their bucket right away, we instead to need to distribute all the individual bags they hold (which could be many), to the remaining 9 buckets. For each bag, we need to ensure which 3 of them already holds it, how many objects and the total size of each bag and bucket, etc. The first approach saves money along the way, but the latter is easier.

Note that changing this number will never affect existing bags. Only when a NEW channel is created, thus a new bag is created, will this number be adhered to by the runtime. It will "dynamically" assign a new bag to the number of buckets we set psuedo-randomly, assuming the buckets are set to accept new bags of course.

### When to Disable a Bucket from Accepting new Bags
Without proposing a specific number, a bucket should be disabled from this well before getting full. It will still have to get (and hold) any new data objects to any bags it already holds (eg. a new video is created to channel corresponding to a bag they are holding). If, by chance or design, the bags (channels) a specific bucket holds all happen to rarely/never add content, it can be a different ratio than for a bucket that holds bags that frequently creates new (large) videos.

Again, if EITHER amount of objects in the bags the bucket holds, OR the cumulative size of said objects reaches a single buckets limits, the upload will fail. Even if the bag in question are held by 10 other buckets, that all have "lots" of spare capacity.

### When to Update Bags
Related to the above, we sometimes need to move bags around. For example when we upgraded to the `giza` network, and content was migrated from the "old" storage system, all bags were assigned to a single bucket operated by jsgenesis. As we don't want rely on a single bucket (that could crash, or reach capacity, or in this specific case, we want to simply remove), we need to replicate all those bags over to other buckets. This is a slow process if done manually (you can only add OR remove a single bag from a single bucket in one transaction), but a simple `bash` script can do the work for us. An example follows below.

Suppose we have:
- a bucket single bucket (id 0), that holds bags 714-901 (eg. `dynamic:channel:714-dynamic:channel:901`).
- we want to add all bags covered by 4 other buckets (1-4) that currently all hold no bags
- we want to replicate all bags twice, not more
- although it's probably not ideal, we don't care if bucket 1 and 2 hold the "first half", and bucket 3 and 4 hold the "second half"
- finally, we want to remove all bags from bucket 0
For simplicity, we assume all buckets have the same (sufficient) capacity, and all bags are the same size.
(This would of course not be true).


```sh
#!/bin/bash
export AUTO_CONFIRM=true

cd /root/joystream/distributor-node/
for i in $(seq 714 808)
do
    yarn storage-node leader:update-bag -i dynamic:channel:$i -k /path/to/storage-lead-role-key.json -a 1
    yarn storage-node leader:update-bag -i dynamic:channel:$i -k /path/to/storage-lead-role-key.json -a 2
done

for i in $(seq 809 901)
do
    yarn storage-node leader:update-bag -i dynamic:channel:$i -k /path/to/storage-lead-role-key.json -a 3
    yarn storage-node leader:update-bag -i dynamic:channel:$i -k /path/to/storage-lead-role-key.json -a 4
done

for i in $(seq 714 901)
do
    yarn storage-node leader:update-bag -i dynamic:channel:$i -k /path/to/storage-lead-role-key.json -r 0
done
```
This is not a very complicated script, for a simple job, but it gets it done. However, there are many dangers with such an approach:
- If you make a mistake (easy with a "smart" version that cares about size and objects in each bag, and bucket capacity), you may do a lot of harm before you notice.
- If you do the last step first, or there is not sufficient time for buckets 1-4 to get at least one copy of EACH of the data objects before you start removing bucket 0, it may be hard (or impossible) to recover.


...TBD
