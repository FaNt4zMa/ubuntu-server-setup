# Timezone configuration

This guide covers different methods to set and verify the system timezone on Ubuntu.

## Method 1: Using `timedatectl` (Recommended)

### 1. List Available Timezones
To view all available timezones, run:
```bash
timedatectl list-timezones
```
Search for your desired timezone in the list (e.g., `America/New_York`).

### 2. Set the Timezone
Set the system timezone to your desired one by running:
```bash
sudo timedatectl set-timezone America/New_York
```
Replace `America/New_York` with the appropriate timezone.

### 3. Verify the Change
Check that the timezone has been successfully updated:
```bash
timedatectl
```

## Method 2: Using `dpkg-reconfigure` (Interactive)

### 1. Launch Timezone Configuration
Run the following command to open a menu for selecting your timezone:
```bash
sudo dpkg-reconfigure tzdata
```
### 2. Select Your Timezone
Follow the prompts to select your geographic region and then your specific timezone.

## Method 3: Manually Setting the Timezone
This method can be used when you need to manually set the timezone by creating a symbolic link.

### 1. Remove the Existing Localtime File
To remove the current localtime configuration:
```bash
sudo rm /etc/localtime
```
### 2. Create a Symbolic Link to the Correct Timezone File
Link the desired timezone file to `/etc/localtime`:
```bash
sudo ln -s /usr/share/zoneinfo/America/New_York /etc/localtime
```
Replace `America/New_York` with the correct timezone from `/usr/share/zoneinfo`.
### 3. Verify the Change
Ensure the timezone was correctly set by checking the date:
```bash
date
```