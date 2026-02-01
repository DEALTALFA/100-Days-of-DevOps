As part of a data migration project, the team lead has tasked the team with migrating data from an existing Azure Blob container to a new Blob container. The existing container contains a substantial amount of data that must be accurately transferred to the new container. The team is responsible for creating the new Blob container and ensuring that all data from the existing container is copied or synced to the new container completely and accurately. It is imperative to perform thorough verification steps to confirm that all data has been successfully transferred to the new container without any loss or corruption.

As a member of the Nautilus DevOps Team, your task is to perform the following:

Create a New Private Azure Blob Container: Name the container nautilus-dest-25786 under the storage account nautilusst30108.

Data Migration: Migrate the file nautilus.txt from the existing nautilus-source-175 container to the new nautilus-dest-25786 container.

Ensure Data Consistency: Ensure that both containers have the file nautilus.txt and confirm the file content is identical in both containers.
```bash
# Step 1: Create a new private Azure Blob Container
az storage container create --name nautilus-dest-25786 --account-name nautilusst30108 --public-access off


az storage container list --account-name nautilusst30108 -o table

```
```bash
# Step 2: Copy the file nautilus.txt from the existing container to the new container

az storage blob list --account-name nautilusst30108 -c nautilus-source-175 -o table

az storage blob copy start --source-container nautilus-source-175 --source-blob nautilus.txt --destination-container nautilus-dest-25786 --destination-blob nautilus.txt --account-name nautilusst30108
```
```bash
# Step 3: Verify that the file nautilus.txt exists in both containers
az storage blob show --container-name nautilus-source-175 --name nautilus.txt --account-name nautilusst30108
# or
az storage blob list --account-name nautilusst30108 -c nautilus-dest-25786 -o table

az storage blob show --container-name nautilus-dest-25786 --name nautilus.txt --account-name nautilusst30108
```
```bash
# Step 4: Download the files from both containers to verify content
az storage blob download --container-name nautilus-source-175 --name nautilus.txt --file nautilus-source.txt --account-name nautilusst30108
az storage blob download --container-name nautilus-dest-25786 --name nautilus.txt --file nautilus-dest.txt --account-name nautilusst30108
```
```bash
# Step 5: Compare the content of both files to ensure they are identical
diff nautilus-source.txt nautilus-dest.txt
```
```bash
# Note: If the diff command returns no output, it means the files are identical.
```
```bash
# Additional Note: Ensure that you have the necessary permissions to access and modify the storage account and containers.
```
```bash
# Additional Note: You may need to wait for the copy operation to complete before verifying the files.
```
```bash
