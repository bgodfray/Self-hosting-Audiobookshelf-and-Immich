# Oracle Cloud — Setup Guide
Goal: create an always free Oracle Cloud VM suitable for hosting Audiobookshelf + Immich and prepare for Hetzner/remote object storage.

* Oracle Cloud Free Tier gives a Free Trial with US$300 credits for 30 days and a set of Always Free resources that do not expire. 
* Ampere (ARM) A1 instances available that are effectively 4 OCPUs + 24 GB RAM (useful to run media/photo processing). 
* Always Free block storage: 200 GB total (boot + block volumes combined). 
* Network: 10 TB per month outbound data transfer allowance (per originating region). 

Important: These limits and offerings are published by Oracle and may change always check Oracle’s official Free Tier page if in doubt. [Oracle Cloud Free Tier](https://www.oracle.com/cloud/free/)

## Table of Contents
1. [Account Prerequisites & Signup Notes](#account-prerequisites--signup-notes)
2. [Region & Tenancy Considerations (Region Lock Warning)](#region--tenancy-considerations-region-lock-warning)
3. [Create Virtual Machine](#create-virtual-machine)
4. [Connect to Virtual Machine](#Connect-to-virtual-machine)

## Account Prerequisites & Signup Notes
* Sign up at https://cloud.oracle.com and start the Free Trial / Free Tier signup. 
* Fill in the account information and the verify your email.
* Select 'Individual' Customer type and Germany Central (Frankfurt) as the home region
* Fill in address and mobile details
* Fill in credit/debit card detail. Remember there is no charge.
* Agree to start your free trial then wait
  
![Screenshot](Images/Oracle/oracle.png)

## Region & Tenancy Considerations (Region Lock Warning)
Choose a region near your storage for my situation as Hetzner is in Germany then Frankfurt is a good choice for low latency.

## Create Virtual Machine
* log into [Oracle Cloud](https://www.oracle.com/cloud/sign-in.html)
* Under services, click on Instances -> Create instance
### Configuration:
### 1. Basic information
* Name - Selfhostingserver
* Placement - Select relevant domain
* Advanced options - skipped
* Image - Select Canonical Ubuntu 24.04 (Minimal) aarch64 or latest verion (Minimal selected as only going to be using as a selfhosting server)
* Shape - Select VM.Standard.A1.Flex (ARM) under Ampere
    * OCPUs to 4
    * Memory to 24GB
* Advanced options - skipped
### 2. Security
* Skipped
### 3. Networking
* VNIC - SelfhostVNIC
  * Create a new virtual cloud network
  * Name - SelfhostVCN
  * Subnet name - SelfhostSN
  * Advanced options - skipped
* SHH keys - Generate a key pair for me (download both sets of keys)
### 4. Storage
* Boot Volume - select Specify a custom boot volume size
    * Boot volume size - 180 (within the 200GB free teir limit)
    * Boot volume performance - 10 (left it as it is)
### 5. Review
* Create and wait

Unfortunately this did not work and I had to upgrade to a PAYG (pay as you go) account to be able to get a VM with the above specs which will still be free. You might get lucky and get to create your VM without the need of a PAYG account. So I created a budget alarm as a backup to ensure I stay in free tier.

### Create budget alarm
* Go to Billing & Cost Management
* Select Budgets
* Create Budget
* Name - FreeTier
* Description - Check for the any costs
* Budget Scope - Compartment
* Schedule - Monthly
* Budget Alert Rule - Actual Spend
* Threshold Type - Absolute Amount
* Threshold Amount - $1
* Email Recipients - Myself
* Email Message - Review Oracle account there is a charge

Recreate the above VM

## Connect to Virtual Machine
Now to make sure the VM instance works I need to connect to it.

I followed the instructions here for windows as that is what I was using [Oracle SSH Docs](https://docs.oracle.com/en-us/iaas/Content/Compute/Tasks/connect-to-linux-instance.htm)

As you can see I asscess the VM instance.

![Screenshot](Images/Oracle/oracleVMinstance.png)





  
