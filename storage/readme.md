# Storage Configuration

Follow these steps to configure and mount a new storage drive on Ubuntu.

## Partitioning a New Drive

### 1. Identify the New Drive  
List all available drives to find the new one (e.g., `/dev/sdb`):
```bash
sudo fdisk -l
```
**Note:** If the drive is larger than **2TB**, use **GPT** instead of MBR.
### 2. Partition the Drive
Use `fdisk` for MBR or `gdisk` for GPT to create a partition.

**MBR Partition (for drives under 2TB)**
```bash
sudo fdisk /dev/sdb
```
* Type `n` to create a new partition.
* Type `p` for a primary partition.
* Accept the default values for partition size.
* Type `w` to write the changes and exit.

**GPT Partition (for drives over 2TB)**
```bash
sudo gdisk /dev/sdb
```
* Type `o` to create a new GPT partition table.
* Type `n` to create a new partition.
* Accept default values to use the entire disk.
* Type `w` to write the changes and exit.

## Formatting the Partition
Format the partition to ext4 (replace `/dev/sdb1` with the actual partition name):
```bash
sudo mkfs.ext4 /dev/sdb1
```

## Mounting the Drive
### 1. Create a Mount Point
Choose a directory where the drive will be mounted:
```bash
sudo mkdir -p /mnt/newdrive
```
### 2. Mount the Drive Temporarily
To verify the setup before making it permanent:
```bash
sudo mount /dev/sdb1 /mnt/newdrive
```
### 3. Auto-Mount on Boot (Persistent Mount)
**Find the Drive’s UUID**
```bash
sudo blkid /dev/sdb1
```
Copy the UUID from the output.

**Edit /etc/fstab to Make the Mount Persistent**
```bash
sudo nano /etc/fstab
```
Add this line at the end (replace `your-uuid-here` with the actual UUID):
```bash
UUID=your-uuid-here /mnt/newdrive ext4 defaults 0 2
```
**Apply and Verify the Mount**
```bash
sudo mount -a
df -h
```

## Reducing Filesystem Overhead (Optional)
By default, ext4 reserves 5% of the filesystem for the root user, which helps prevent system failures when storage is full. You can reduce this to 1% (useful for large storage drives):
```bash
sudo tune2fs -m 1 /dev/sdb1
```