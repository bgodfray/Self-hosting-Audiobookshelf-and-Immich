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
