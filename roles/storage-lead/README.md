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
    - [Buckets](#buckets)
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
<!-- TOC END -->


## The Storage and Distribution System
Storage and Distribution system is well documented on github. A simple version will be outlined here.

### Bags
Whenever a new channel is created, a new "bag" is created. Each time a new asset is added under this channel, the bag "grows" in size, and each time an asset is removed, the bag shrinks.

Currently, we only support `dynamic` bags (`static` will be added), on the `channel` level. Channels are identified by a Channel ID (`channelId`) that is incremented by 1 for each new channel as transaction are made.

If a new channel is created and given the `channelId` 1337, the new bag will be `dynamic:channel:1337`. Even if there are no assets in the bag, it will persist until the channel is deleted.

### Buckets
Both the Storage and Distribution implementation uses the concept of `buckets`. A `bucket` can be populated with multiple `bags`.

#### Storage Buckets
A Storage Bucket can only be created by the Storage Lead. A single worker can only be invited, (and thus accept) to be the operator of a single bucket at the time. Simultaneously, the Lead can not invite more than a single worker to be the operator of a single bucket. Only if the operator is removed, or the invitation is cancelled, can a new operator be invited, or the operator be invited to another bucket.

When creating them, a limit on both the number of files it can contain, and the total size of the files, must be set. If this capacity is exceeded, any new assets added will be rejected.

The consequence of that is that any channels whose bag is assigned to this bucket, will not be able to create new videos. It will fail on the runtime level, even if the Storage Node assigned to the bucket has the storage space to accept them, or that there are other buckets whose limits are not exceeded that also is assigned that bag.

#### Deleting or Re-assigning Storage Buckets
As long as there are any bags in a bucket, the bucket can not be deleted. And a worker that is operating a bucket can not be fired from a role, before they are removed from the bucket. In order to


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
- allow new bags
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
yarn storage-node leader:set-bucket-limits -i 0 -k /path/to/storage-lead-role-key.json -o 1000 -s 100000000000
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
- Unlike the `deletionPrice` (which are reimbursed when you delete an asset) the fees are burned.

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
yarn storage-node leader:update-dynamic-bag-policy -n 1 -k /path/to/storage-lead-role-key.json -t Channel
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
yarn storage-node leader:update-voucher-limits -k /path/to/storage-lead-role-key.json -n 1000 -s 100000000000
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
yarn run storage-node operator:accept-invitation -i 3 -k /path/to/storage-lead-role-key.json -w 1 -t 5StorageOperatorKey
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
yarn run storage-node operator:set-metadata -i 3 -k /path/to/storage-lead-role-key.json -w 1 -j /path/to/metadata.json
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
