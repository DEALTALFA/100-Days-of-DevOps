The Nautilus DevOps team needs to expand the storage capacity of an existing virtual machine and add an additional data disk to support increased workloads. This task requires resizing the existing VM disk and mounting a new data disk to the VM.

As a member of the team, perform the following steps:

1. Expand the existing VM devops-vm disk from 32Gi to 64Gi.

2. Also create a new standard HDD data disk named devops-disk of 64Gi and mount the disk to VM devops-vm at location /mnt/devops-disk.


```bash
# Step 1: Connect to the VM and mount the new data disk
# SSH into the VM
ssh azureuser@<devops-vm-public-ip>
```

```bash
# Inside the VM, identify the new disk
sudo fdisk -l
```

```bash
# Assuming the new disk is /dev/sdc, create a partition
sudo fdisk /dev/sdc
# Follow the prompts to create a new partition
```

```bash
# Format the new partition
sudo mkfs.ext4 /dev/sdc1
```
```bash
# Create a mount point
sudo mkdir -p /mnt/devops-disk
# Mount the new disk
sudo mount /dev/sdc1 /mnt/devops-disk
```
```bash
# Verify the mount
df -h
```
```bash
# To make the mount persistent, add it to /etc/fstab
echo '/dev/sdc1 /mnt/devops-disk ext4 defaults 0 0' | sudo tee -a /etc/fstab
```
```bash
# Exit the VM
exit
```