The Nautilus Development Team is preparing for a major update to their database infrastructure. To ensure a smooth transition and to safeguard data, the team has requested the DevOps team to take a snapshot of the current RDS instance and restore it to a new instance. This process is crucial for testing and validation purposes before the update is rolled out to the production environment. The snapshot will serve as a backup, and the new instance will be used to verify that the backup process works correctly and that the application can function seamlessly with the restored data.

As a member of the Nautilus DevOps Team, your task is to perform the following:

Take a Snapshot: Take a snapshot of the datacenter-rds RDS instance and name it datacenter-snapshot (please wait datacenter-rds instance to be in available state).

Restore the Snapshot: Restore the snapshot to a new RDS instance named datacenter-snapshot-restore.

Configure the New RDS Instance: Ensure that the new RDS instance has a class of db.t3.micro.

Verify the New RDS Instance: The new RDS instance must be in the Available state upon completion of the restoration process.
```bash
# Step 1: Take a snapshot of the existing RDS instance
aws rds create-db-snapshot --db-instance-identifier datacenter-rds --db-snapshot-identifier datacenter-snapshot
```
```bash
# Step 2: Wait for the snapshot to be available
aws rds wait db-snapshot-available --db-snapshot-identifier datacenter-snapshot
```
```bash
# Step 3: Restore the snapshot to a new RDS instance
aws rds restore-db-instance-from-db-snapshot --db-instance-identifier datacenter-snapshot-restore --db-snapshot-identifier datacenter-snapshot --db-instance-class db.t3.micro
```
```bash
# Step 4: Wait for the new RDS instance to be available
aws rds wait db-instance-available --db-instance-identifier datacenter-snapshot-restore
```
```bash
# Step 5: Verify the new RDS instance is in Available state
aws rds describe-db-instances --db-instance-identifier datacenter-snapshot-restore --query "DBInstances[0].DBInstanceStatus"
```
```bash
# Note: Ensure that you have the necessary permissions to perform RDS operations in your AWS account.
```
```bash
# Additional Note: You can monitor the progress of the snapshot and restoration process through the AWS Management Console or AWS CLI.
```
```bash
# End of the steps for snapshot and restoration of RDS instance.
```