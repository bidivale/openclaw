# swap memory increase from 8gb to 32gb

## before change
skmindlab@skmindlab-Lat-7390:~$ free -h
               total        used        free      shared  buff/cache   available
Mem:            31Gi        10Gi       7.4Gi       7.0Gi        21Gi        20Gi
Swap:          8.0Gi          0B       8.0Gi
skmindlab@skmindlab-Lat-7390:~$ swapon --show
NAME      TYPE SIZE USED PRIO
/swap.img file   8G   0B   -2


### fix given by llm : 

Great! Thanks for the `free -h` output — here's your **current system status**:

| Resource     | Total | Used | Free |
|--------------|-------|------|------|
| **RAM**      | 31 GiB | 10 GiB | 7.4 GiB (real) + 21 GiB (buff/cache) |
| **Swap**     | 8 GiB | 0 B | 8 GiB |
| **Available** | —     | —    | **20 GiB** (Docker can use this) |

You have **plenty of RAM** (31 GB), but **OOM during `docker pull` of large images** is still happening because:

> **Docker decompresses image layers in RAM *before* writing to disk**  
> Even with 31 GB RAM, **pulling 3+ large layers at once** can spike to **15–25 GB RAM usage**  
> Your **8 GB swap is too small** to absorb the overflow → **OOM killer kills `dockerd`**

---

####
Your Fix: **Increase Swap to 32 GB** + **Limit Concurrent Downloads**

##### Step 1: **Increase Swap from 8 GB → 32 GB**

```bash
# Turn off current swap
sudo swapoff /swap.img

# Remove old 8G file
sudo rm /swap.img

# Create new 32 GB swap file
sudo fallocate -l 32G /swap.img
sudo chmod 600 /swap.img
sudo mkswap /swap.img
sudo swapon /swap.img
```

**Make permanent** (if not already in `/etc/fstab`):
```bash
# Check if line exists
grep swap.img /etc/fstab

# If not, add it
echo '/swap.img none swap sw 0 0' | sudo tee -a /etc/fstab
```

Verify:
```bash
free -h
swapon --show
```

**Expected output**:
```
              total        used        free
Mem:           31Gi        10Gi       7.4Gi
Swap:          32Gi         0B        32Gi
```

---

##### Step 2: **Limit Docker to 1 concurrent layer download**

This is **critical** — even with 32 GB swap, pulling 5 layers at once can still OOM.

```bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json > /dev/null <<EOF
{
  "max-concurrent-downloads": 1,
  "max-concurrent-uploads": 1
}
EOF
```

Restart Docker:
```bash
sudo systemctl restart docker
```

> This makes `docker pull` download **one layer at a time** → **RAM usage drops from 20 GB → ~5–8 GB peak**

---

##### Step 3: **(Optional) Monitor During Pull**

```bash
# Terminal 1: Watch memory
watch -n 1 "free -h | grep -E 'Mem|Swap' && echo '---' && ps -p 1 -o pid,cmd,%mem --sort=-%mem | head -5"

# Terminal 2: Pull a large image
docker pull nvidia/cuda:12.6.0-devel-ubuntu24.04
# or your actual large image
```

You’ll see:
- RAM usage: ~8–12 GB peak
- Swap: may use 1–5 GB
- **No OOM**

---

#### Final Recommended Config

```json
// /etc/docker/daemon.json
{
  "max-concurrent-downloads": 1,
  "max-concurrent-uploads": 1
}
```

```bash
# /etc/fstab (add if missing)
/swap.img none swap sw 0 0
```

```bash
# Swap file
/swap.img  → 32 GB
```

---

#### Why This Works

| Problem | Fix |
|-------|-----|
| 3+ layers decompressing → 20+ GB RAM | `max-concurrent-downloads: 1` |
| Swap too small (8 GB) | 32 GB swap absorbs overflow |
| OOM killer kills Docker | Now has breathing room |

---

#### Test It Now

```bash
# Apply changes
sudo swapoff /swap.img && sudo rm /swap.img
sudo fallocate -l 32G /swap.img && sudo chmod 600 /swap.img && sudo mkswap /swap.img && sudo swapon /swap.img
echo '/swap.img none swap sw 0 0' | sudo tee -a /etc/fstab

sudo tee /etc/docker/daemon.json > /dev/null <<EOF
{ "max-concurrent-downloads": 1 }
EOF

sudo systemctl restart docker

# Now pull ANY large image
docker pull pytorch/pytorch:2.4.0-cuda12.1-cudnn9-devel
```

**It will work.**

---

Let me know the **exact image** you're pulling — I’ll tell you expected RAM/swap usage!


## What have I actually done?
First, I turned it off by : 

$ sudo swapoff /swap.img

It asked for the password and turned it off.

Then I removed old 8gb file : 
$ sudo rm /swap.img


Then created 32 gb swap file.

First pasted these 2 commands one after another : 

$ sudo fallocate -l 32G /swap.img

$ sudo chmod 600 /swap.img

Then ran this : 

$ sudo mkswap /swap.img


This gave me the following result : 
'Setting up swapspace version 1, size = 32 GiB (34359734272 bytes)
no label, UUID=0add2355-c02f-4c60-870a-304cb113bc3a'

Then I ran this : 
$ sudo swapon /swap.img



#### Then I checked by the following commands and ensured that the changes are permanent - so did not give any extra effort to make it permanent : 

skmindlab@skmindlab-Lat-7390:~$ grep swap.img /etc/fstab
/swap.img	none	swap	sw	0	0
skmindlab@skmindlab-Lat-7390:~$ ls -lh /swap.img
-rw------- 1 root root 32G Oct 30 00:29 /swap.img
skmindlab@skmindlab-Lat-7390:~$ swapon --show
NAME      TYPE SIZE USED PRIO
/swap.img file  32G   0B   -2


#### fianlly checked it to make sure everything is done : 
skmindlab@skmindlab-Lat-7390:~$ free -h | grep Swap
Swap:           31Gi          0B        31Gi
skmindlab@skmindlab-Lat-7390:~$ swapon --show
NAME      TYPE SIZE USED PRIO
/swap.img file  32G   0B   -2
skmindlab@skmindlab-Lat-7390:~$ grep swap.img /etc/fstab
/swap.img	none	swap	sw	0	0

## Note that I have only followed step 1, did not follow step 2 of the prompt  - that is pull one at a time - because I never run multiple pull at a time.


# If I ever want to go back to my default setting undoing this current setting what I have done now, I need to give these following 3 propmts : 

## 1. Turn off the big 32 GB swap
sudo swapoff /swap.img

## 2. Delete the 32 GB swap file
sudo rm /swap.img

## 3. Remove the swap line from fstab (so it doesn't come back after reboot)
sudo sed -i '/swap.img/d' /etc/fstab



