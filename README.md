# AWS Cloud Cost Optimization - Identifying Stale Resources

## Identifying Stale EBS Snapshots

In this example, we'll create a Lambda function that identifies EBS snapshots that are no longer associated with any active EC2 instance and deletes them to save on storage costs.

**Description:**

**Retrieve EBS Snapshots:** 
The function starts by retrieving a list of all Amazon EBS (Elastic Block Store) snapshots owned by the AWS account that the Lambda function is running under. It uses the ‘self’ keyword to filter snapshots belonging specifically to this account, ignoring snapshots shared from other accounts.

**Identify Active EC2 Instances:**
Next, the function fetches a list of all EC2 (Elastic Compute Cloud) instances that are either in a "running" or "stopped" state. This ensures that the function identifies all instances that may currently or potentially use an EBS volume.

**Check Snapshot Associations:** 
For each EBS snapshot retrieved, the function checks whether the snapshot’s associated volume is in use by any of the identified active EC2 instances. It determines if there’s a direct link between the snapshot and any active volume attached to the EC2 instances in the account.

**Detect and Delete Stale Snapshots:**
If a snapshot is found that doesn’t have any active associations—meaning its associated volume is not in use by any running or stopped instances—the function classifies it as "stale." Stale snapshots are considered obsolete for recovery purposes, as they no longer have any relevance to currently active volumes or instances.

**Automated Deletion:**
For each stale snapshot detected, the function initiates a deletion process. This helps in reducing unnecessary storage costs, as old or unused snapshots can accumulate over time, leading to high storage charges.

This Lambda function automates EBS snapshot management by identifying unused snapshots (not associated with any active EC2 volumes) and removing them to help control and optimize cloud storage costs. This can lead to significant savings by eliminating storage charges associated with obsolete snapshots, especially in accounts with a large number of snapshots or frequent EBS volume changes.
