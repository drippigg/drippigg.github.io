```markdown
[ReturnHome](/)
# 🚀 Deploying a Custom NixOS Image on DigitalOcean


## 📌 Step 1: Create a DigitalOcean VM
1. Log into [DigitalOcean](https://cloud.digitalocean.com/)
2. Navigate to **Droplets** → **Create Droplet**
3. Choose **Ubuntu 22.04 (or any Linux distro)**
4. Select the **cheapest plan** (e.g., $6/mo shared CPU)
5. Choose a **datacenter region**
6. Set up **SSH access**
7. Click **Create Droplet**

## 📌 Step 2: Prepare the VM for NixOS Installation
SSH into the newly created droplet:
```bash
ssh root@your-droplet-ip
```
Update and install necessary packages:
```bash
apt update && apt install -y parted wget
```
Download the latest **NixOS minimal ISO**:
```bash
wget https://channels.nixos.org/nixos-24.11/latest-nixos-minimal-x86_64-linux.iso -O nixos.iso
```

## 📌 Step 3: Create a NixOS Image
Create a **raw disk image**:
```bash
dd if=/dev/zero of=nixos.img bs=1G count=8  # Adjust size as needed
mkfs.ext4 nixos.img  # Format the image
```
Mount and install NixOS:
```bash
mkdir /mnt/nixos
mount -o loop nixos.img /mnt/nixos
# Download and extract the NixOS ISO contents here
```

### **Configure NixOS**
1. Copy the default NixOS config:
```bash
cp -r /etc/nixos /mnt/nixos/
```
2. Modify `/mnt/nixos/configuration.nix` to suit your needs.
3. Unmount:
```bash
umount /mnt/nixos
```
4. Compress the image:
```bash
gzip nixos.img
```

## 📌 Step 4: Upload to DigitalOcean Spaces
1. Create a **Spaces Bucket** in DigitalOcean.
2. Install `s3cmd`:
```bash
apt install s3cmd
```
3. Configure it:
```bash
s3cmd --configure
```
4. Upload the image:
```bash
s3cmd put nixos.img.gz s3://your-space-name/
```

## 📌 Step 5: Create a Custom Snapshot in DigitalOcean
1. Go to **Images** → **Custom Images**
2. Click **Import Image**
3. Paste the **public URL** of `nixos.img.gz`
4. Click **Start Import** and wait for completion

## 🎉 Step 6: Deploy Your NixOS Droplet!
1. Go to **Droplets** → **Create Droplet**


3. Select **Custom Images**
4. Choose your **NixOS image**
5. Set up networking & SSH
6. Click **Create**

## ✅ BOOOYAH! NixOS is now running on DigitalOcean! 🚀

[ReturnHome](/)
```

