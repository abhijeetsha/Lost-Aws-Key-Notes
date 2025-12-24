# Lost-Aws-Key-Notes
Different Key Attach to Aws Instance
## ✅ SOLUTION: Attach New Key via Root Volume (Recommended)
### 🧩 High-Level Steps
* Stop the instance
* Detach root volume
* Attach volume to helper EC2
* Add new public key
* Reattach volume
* Start instance & SSH

### 🛠 Step-by-Step (Ubuntu EC2)
### 1️⃣ Stop the Problem Instance
* AWS Console → EC2 → Instances
* Select instance
* Actions → Instance state → Stop
* ⚠️ Do NOT terminate

### 2️⃣ Detach Root Volume
* Go to EC2 → Volumes
* Find volume attached to your instance (/dev/xvda)
* Actions → Detach volume

### 3️⃣ Launch Helper EC2
* Launch temporary EC2
* Same AZ
* Same OS (Ubuntu)
* SSH into helper instance

### 4️⃣ Attach Old Volume to Helper
* EC2 → Volumes
* Select detached volume
* Attach volume
* Instance: helper EC2
* Device: /dev/sdf

### 5️⃣ Mount Volume on Helper EC2
* lsblk
* sudo mkdir /mnt/recovery
* sudo mount /dev/xvdf1 /mnt/recovery

### 6️⃣ Add New Public Key
* Go to:
* cd /mnt/recovery/home/ubuntu/.ssh
* If .ssh doesn’t exist:
* sudo mkdir .ssh
* sudo chmod 700 .ssh
* Add your new public key:
* sudo nano authorized_keys

* Paste:
* ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQ...

* Set permissions:
* sudo chmod 600 authorized_keys
* sudo chown -R ubuntu:ubuntu .ssh

### 7️⃣ Unmount & Detach Volume
* sudo umount /mnt/recovery
* AWS Console → Detach volume from helper

### 8️⃣ Reattach Volume to Original Instance
* Attach volume back to original EC2
* Device: /dev/xvda

### 9️⃣ Start Instance & SSH
* ssh -i new-key.pem ubuntu@PUBLIC_IP
* 🎉 Access restored!

### 🔁 Alternative (Easier): EC2 Instance Connect (If Enabled)
* Works only if:
* Instance supports it
* SSH port open
* Instance metadata enabled

Not always available → root volume method is safest.
