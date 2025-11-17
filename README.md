# Self hosting Audiobookshelf and Immich server online
## Project Overview
Due to my old laptop no longer being supported by Windows 10 and incompatible with Windows 11, I decided to challenge myself by migrating my self-hosted applications (Audiobookshelf and Immich) to an online platform accessible from mobile devices at minimum cost.

# Why move to an online solution?
## Moving from a laptop to cloud services
* No electricity costs for running hardware 24/7
* Better uptime and reliability stop the service interruptions from Windows updates requiring restarts
* No hardware maintenance concerns
* Access services from any device with internet connectivity
* Find a true free service not just a free trial

## Learning Opportunity
* Setting up a new Audiobookshelf server from scratch
* Setting up a new Immich server server from scratch
* Setting up a cloud virtual machine and maintain
* Implementing secure remote access solutions

## Project Goals
* Successfully migrate existing Audiobookshelf and Immich data to a new cloud server
* Configure secure remote access (potentially use Tailscale)
* Document the entire process for personal reference and community benefit
* Stay within the free tier limits where possible
* Minimise storage costs. Currently require about 250GB minimum.


# Project Timeline 
## Project start 17Nov25

## Find a virtual machine service
While studying for the AWS Certified Cloud Practitioner certification, I discovered AWS EC2 virtual machines. On this basis I wondered if whether the AWS free tier services can support running Audiobookshelf and Immich servers. I researched the the AWS free service and found the below details

# AWS Free Tier Analysis
# Compute Resources:
* 750 hours/month of EC2 t2.micro or t3.micro instances
* 1 vCPU, 1GB RAM
* Choice of Linux or Windows
* Can run 1 instance 24/7 or multiple instances totaling 750 hours

# Storage:
* 30GB of Amazon EBS (Elastic Block Storage)
* 5GB S3 standard storage
* 20,000 GET requests and 2,000 PUT requests for S3

#Conclusion: 
Unfortunately, a 1 vCPU and 1GB RAM virtual machine is insufficient to support Audiobookshelf, Immich, and Tailscale. This led me to research other options such as Azure and Oracle Cloud alternatives.

### Cloud Provider Comparison
| Feature | AWS Free Tier | Azure Free Tier | Oracle Cloud Always Free |
|---------|---------------|-----------------|--------------------------|
| **Duration** | 6 months | 12 months | Unlimited (Forever) |
| **Credits** | $100-200 (6 months) | $200 (30 days) | $300 (30 days) + Always Free |
| **VM Specs** | 750 hrs/month t2.micro<br>(1 vCPU, 1GB RAM) | 750 hrs/month B1S<br>(1 vCPU, 1GB RAM) | 4 vCPU, 24GB RAM ARM<br>or 2x AMD VMs |
| **Storage** | 30GB EBS | 64GB x 2 Managed Disks | 200GB Block + Boot Volumes |
| **Bandwidth** | 100GB/month outbound | Limited | 10TB/month outbound |

## Why Oracle Cloud wins for hosting Audiobookshelf and Immich
* Resources: 4 cores + 24GB RAM >> 1 core + 1GB RAM (AWS/Azure)
* Storage: 200GB for your media library vs 30-64GB
* Bandwidth: 10TB/month for streaming vs 100GB
* Cost: Free forever vs bills after 6-12 months
* No time pressure: Build and optimize at your own pace
* Power: Can handle media transcoding, photo processing, and multiple services

# Key Advantages:
* Never expires always free
* Most generous free tier in terms of compute resources
* High bandwidth allowance (10TB vs 100GB on AWS)
* No forced upgrades or account closures

# Key Limitations:
* ARM architecture could limit software builds
* Regional lock once you select a home region you can not change it after account creation
* Account verification a valid credit/debit card needed (no virtual or prepaid cards)

## Finding a storage provider
Oracle Cloud provides 200GB of data which is enough to setup Audiobookshelf, Immich and Tailscale. However, not enough to store my photos and Audiobooks. 

### Storage Provider Comparison
| Feature | Oracle Cloud Always Free | Backblaze B2 | Wasabi | Cloudflare R2 | Hetzner Storage Box | Google Cloud Storage | Storj |
|---------|---------------|-----------------|--------------------------|--------------------------|--------------------------|--------------------------|--------------------------|
| **Cost** | 500GB = $7.65/month | $0.005/GB/month (500GB = $2.50/mo, 1TB = $5/mo) | $6.99/month for 1TB minimum | $0.015/GB/month (500GB = $7.50/mo, 1TB = $15/mo) | 1TB = €3.81/month | $0.01/GB/month (500GB = $5/mo, 1TB = $10/mo) |  $0.004/GB/month (500GB = $2/mo, 1TB = $4/mo) |
| **Integration** | Oracle | rclone support, S3-compatible API | rclone support, S3-compatible API | S3-compatible | Mount via rclone or native protocols | rclone support | rclone support, S3-compatible API |
| **Comments** |  | Free tier: 10GB storage, 1GB daily download free |  |  | WebDAV, SFTP, Samba/CIFS, rsync protocols |  |  |

Based on the above I have decided to use Hetzner Storage Box which has data centers in Germany and Finland.

## Summary of setup
* Oracle Cloud VM (ARM - 4 OCPU, 24GB RAM) - FREE
* 100-150GB Block Storage - OS, apps, databases, thumbnails - FREE
* Hetzner Storage Box (1TB) - Audiobooks + Immich originals - €3.81/mo
* Total Cost: €3.81/month (~$4/month)

### Why This Combination
### 24GB RAM more than enough for Immich + Audiobookshelf
* Immich can run ML models comfortably
* Fast photo processing

### 4 OCPUs - Plenty of CPU power
* Quick thumbnail generation
* Face detection works well
* Smooth audiobook streaming

### Storage Strategy
* Local 200GB: App files and Immich thumbnails
* Hetzner 1TB: Original photos/videos and audiobooks

### Important Considerations
### Location/Latency
* Oracle Cloud has European data centers (Frankfurt, Amsterdam, London, etc.)
* Choose Frankfurt to minimize latency to Hetzner (Germany)
* Should give excellent performance

### How Immich Would Work
* Upload photo → Stored on Hetzner
* Immich generates thumbnail storing it locally on Oracle block storage
* Viewing photos Thumbnails load instantly, originals load from Hetzner
* Face detection/ML works on the powerful Oracle VM

### How Audiobookshelf Would Work
* Audiobooks stored on Hetzner
* Metadata/database on local Oracle storage
* Streaming directly from Hetzner

## Detailed Setup Guides
- [Oracle Cloud VM Setup Guide](./Docs/ORACLE_SETUP)  Docs/ORACLE_SETUP




