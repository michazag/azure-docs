---
title: Managing the Azure Storage Lifecycle
description: Learn how to create lifecycle policy rules to transition aging data from Hot to Cool and Archive tiers.
services: storage
author: yzheng-msft
ms.service: storage
ms.topic: conceptual
ms.date: 3/20/2019
ms.author: yzheng
ms.subservice: common
---

# Manage the Azure Blob storage Lifecycle

Data sets have unique lifecycles. Early in the lifecycle, people access some data often. But the need for access drops drastically as the data ages. Some data stays idle in the cloud and is rarely accessed once stored. Some data expires days or months after creation, while other data sets are actively read and modified throughout their lifetimes. Azure Blob storage lifecycle management offers a rich, rule-based policy for GPv2 and Blob storage accounts. Use the policy to transition your data to the appropriate access tiers or expire at the end of the data's lifecycle.

The lifecycle management policy lets you:

- Transition blobs to a cooler storage tier (hot to cool, hot to archive, or cool to archive) to optimize for performance and cost
- Delete blobs at the end of their lifecycles
- Define rules to be run once per day at the storage account level
- Apply rules to containers or a subset of blobs (using prefixes as filters)

Consider the scenario where a data set gets frequent access during the early stages of the lifecycle, but then only occasionally after two weeks. Beyond the first month, the data set is rarely accessed. In this scenario, hot storage is best during the early stages. Cool storage is most appropriate for occasional access, and archive storage is the best tier option after the data ages over a month. By adjusting storage tiers in respect to the age of data, you can design the least expensive storage options for your needs. To achieve this transition, lifecycle management policy rules are available to move aging data to cooler tiers.

## Storage account support

The lifecycle management policy is available with both General Purpose v2 (GPv2) accounts and Blob storage accounts. In the Azure portal, you can upgrade an existing General Purpose (GPv1) account to a GPv2 account via a simple one-click process. For more information about storage accounts, see [Azure storage account overview](../common/storage-account-overview.md).  

## Pricing 

The lifecycle management feature is free of charge. Customers are charged the regular operation cost for the [List Blobs](https://docs.microsoft.com/rest/api/storageservices/list-blobs) and [Set Blob Tier](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) API calls. Delete operation is free. For more information about pricing, see [Block Blob pricing](https://azure.microsoft.com/pricing/details/storage/blobs/).

## Regional Availability 
The lifecycle management feature is available in all public Azure regions. 


## Add or remove a policy 

You can add, edit, or remove a policy by using the Azure portal, [Azure PowerShell](https://github.com/Azure/azure-powershell/releases), the Azure CLI, [REST APIs](https://docs.microsoft.com/en-us/rest/api/storagerp/managementpolicies), or a client tool. This article shows how to manage policy by using the portal and PowerShell methods.  

> [!NOTE]
> If you enable firewall rules for your storage account, lifecycle management requests may be blocked. You can unblock these requests by providing exceptions. The required bypass are: `Logging,  Metrics,  AzureServices`. For more information, see the Exceptions section in [Configure firewalls and virtual networks](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).

### Azure portal

1. Sign in to the [Azure portal](https://portal.azure.com).

2. Select **All resources** and then select your storage account.

3. Under **Blob Service**, select **Lifecycle management** to view or change your policy.

### PowerShell

```powershell
#Install the latest module
Install-Module -Name Az -Repository PSGallery 

#Create a new action object

$action = Add-AzStorageAccountManagementPolicyAction -BaseBlobAction Delete -daysAfterModificationGreaterThan 2555
$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -BaseBlobAction TierToArchive -daysAfterModificationGreaterThan 90
$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -BaseBlobAction TierToCool -daysAfterModificationGreaterThan 30
$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -SnapshotAction Delete -daysAfterCreationGreaterThan 90

# Create a new filter object
# PowerShell automatically sets BlobType as “blockblob” because it is the only available option currently
$filter = New-AzStorageAccountManagementPolicyFilter -PrefixMatch ab,cd 

#Create a new rule object
#PowerShell automatically sets Type as “Lifecycle” because it is the only available option currently
$rule1 = New-AzStorageAccountManagementPolicyRule -Name Test -Action $action -Filter $filter

#Set the policy 
$policy = Set-AzStorageAccountManagementPolicy -ResourceGroupName $rgname -StorageAccountName $accountName -Rule $rule1

```


## Policy

A lifecycle management policy is a collection of rules in a JSON document:

```json
{
  "rules": [
    {
      "name": "rule1",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {...}
    },
    {
      "name": "rule2",
      "type": "Lifecycle",
      "definition": {...}
    }
  ]
}
```


A policy is a collection of rules:

| Parameter name | Parameter type | Notes |
|----------------|----------------|-------|
| rules          | An array of rule objects | At least one rule is required in a policy. You can define up to 100 rules in a policy.|

Each rule within the policy has several parameters:

| Parameter name | Parameter type | Notes | Required |
|----------------|----------------|-------|----------|
| name           | String |A rule name can include up to 256 alphanumeric characters. Rule name is case-sensitive.  It must be unique within a policy. | True |
| enabled | Boolean | An optional boolean to allow a rule to be temporary disabled. Default value is true if it is not set. | False | 
| type           | An enum value | The current valid type is `Lifecycle`. | True |
| definition     | An object that defines the lifecycle rule | Each definition is made up of a filter set and an action set. | True |

## Rules

Each rule definition includes a filter set and an action set. The [filter set](#rule-filters) limits rule actions to a certain set of objects within a container or objects names. The [action set](#rule-actions) applies the tier or delete actions to the filtered set of objects.

### Sample rule
The following sample rule filters the account to run the actions on objects that exist inside `container1` **AND** start with `foo`.  

- Tier blob to cool tier 30 days after last modification
- Tier blob to archive tier 90 days after last modification
- Delete blob 2,555 days (seven years) after last modification
- Delete blob snapshots 90 days after snapshot creation

```json
{
  "rules": [
    {
      "name": "ruleFoo",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "container1/foo" ]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
            "delete": { "daysAfterModificationGreaterThan": 2555 }
          },
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}

```

### Rule filters

Filters limit rule actions to a subset of blobs within the storage account. If more than one filter is defined, a logical `AND` runs on all filters.

Valid filters include:

| Filter name | Filter type | Notes | Is Required |
|-------------|-------------|-------|-------------|
| blobTypes   | An array of predefined enum values. | The current release supports `blockBlob`. | Yes |
| prefixMatch | An array of strings for prefixes to be match. Each rule can define up to 10 prefixes. A prefix string must start with a container name. For example, if you want to match all blobs under "https://myaccount.blob.core.windows.net/container1/foo/..." for a rule, the prefixMatch is `container1/foo`. | If you don't define prefixMatch, the rule applies to all blobs within the storage account.  | No |

### Rule actions

Actions are applied to the filtered blobs when the run condition is met.

Lifecycle management supports tiering and deletion of blobs and deletion of blob snapshots. Define at least one action for each rule on blobs or blob snapshots.

| Action        | Base Blob                                   | Snapshot      |
|---------------|---------------------------------------------|---------------|
| tierToCool    | Support blobs currently at hot tier         | Not supported |
| tierToArchive | Support blobs currently at hot or cool tier | Not supported |
| delete        | Supported                                   | Supported     |

>[!NOTE] 
>If you define more than one action on the same blob, lifecycle management applies the least expensive action to the blob. For example, action `delete` is cheaper than action `tierToArchive`. Action `tierToArchive` is cheaper than action `tierToCool`.

The run conditions are based on age. Base blobs use the last modified time to track age, and blob snapshots use the snapshot creation time to track age.

| Action run condition | Condition value | Description |
|----------------------------|-----------------|-------------|
| daysAfterModificationGreaterThan | Integer value indicating the age in days | Valid condition for base blob actions |
| daysAfterCreationGreaterThan     | Integer value indicating the age in days | Valid condition for blob snapshot actions | 

## Examples
The following examples demonstrate how to address common scenarios with lifecycle policy rules.

### Move aging data to a cooler tier

This example shows how to transition block blobs prefixed with `container1/foo` or `container2/bar`. The policy transitions blobs that haven't been modified in over 30 days to cool storage, and blobs not modified in 90 days to the archive tier:

```json
{
  "rules": [
    {
      "name": "agingRule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "container1/foo", "container2/bar" ]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}
```

### Archive data at ingest 

Some data stays idle in the cloud and is rarely, if ever, accessed once stored. The following lifecycle policy is configured to archive data once it is ingested. This example transitions block blobs in the storage account within container `archivecontainer` into an archive tier. The transition is accomplished by acting on blobs 0 days after last modified time:

```json
{
  "rules": [
    {
      "name": "archiveRule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "archivecontainer" ]
        },
        "actions": {
          "baseBlob": {
              "tierToArchive": { "daysAfterModificationGreaterThan": 0 }
          }
        }
      }
    }
  ]
}

```

### Expire data based on age

Some data is expected to expire days or months after creation to reduce costs or meet government requirements. You can configure a lifecycle management policy to expire data by deletion based on data age. The following example shows a policy that deletes all block blobs (with no prefix specified) older than 365 days.

```json
{
  "rules": [
    {
      "name": "expirationRule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ]
        },
        "actions": {
          "baseBlob": {
            "delete": { "daysAfterModificationGreaterThan": 365 }
          }
        }
      }
    }
  ]
}
```

### Delete old snapshots

For data that is modified and accessed regularly throughout its lifetime, snapshots are often used to track older versions of the data. You can create a policy that deletes old snapshots based on snapshot age. The snapshot age is determined by evaluating the snapshot creation time. This policy rule deletes block blob snapshots within container `activedata` that are 90 days or older after snapshot creation.

```json
{
  "rules": [
    {
      "name": "snapshotRule",
      "enabled": true,
      "type": "Lifecycle",      
    "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "activedata" ]
        },
        "actions": {
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}
```
## FAQ - I created a new policy, why are the actions not run immediately? 

The platform runs the lifecycle policy once a day. Once you configure a policy, it can take up to 24 hours for some actions (such as tiering and deletion) to run for the first time.  

## Next steps

Learn how to recover data after accidental deletion:

- [Soft delete for Azure Storage blobs](../blobs/storage-blob-soft-delete.md)
