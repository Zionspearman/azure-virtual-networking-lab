# Implement Virtual Networking 

This repository supports my video tutorial on implementing **Azure Virtual Networking**, including subnetting, NSGs/ASGs, and Azure DNS (public + private).  
📺 [Watch the lab video on YouTube](https://youtu.be/Lcv1bvN4TRg)

---

## 🔍 Lab Overview

In this lab, you’ll learn how to:
- Design Azure virtual networks with scalable address spaces and subnets
- Deploy a second virtual network using an exported **ARM template**
- Secure network traffic using **Network Security Groups (NSGs)** and **Application Security Groups (ASGs)**
- Configure **Public DNS Zones** and **Private DNS Zones** in Azure
- Validate DNS name resolution using `nslookup`

These skills are foundational for real-world Azure networking and directly support the **AZ-104** exam objectives.

---

## 🧪 Lab Scenario

A global organization is implementing Azure virtual networks to support current workloads and future growth. The immediate goal is to accommodate existing resources while planning for expansion.

You are tasked with:
- Building a large core services network with room to scale
- Deploying a manufacturing network designed for internal connected devices
- Enforcing secure traffic flow between workloads
- Configuring both public and private DNS name resolution

---

## 🛠️ Key Tasks

### ✅ Task 1: Create a Virtual Network with Subnets (Portal)
- Create resource group: `az104-rg4`
- Create VNet: `CoreServicesVnet` in **East US**
- Configure address space: `10.20.0.0/16`
- Create subnets:
  - `SharedServicesSubnet` → `10.20.10.0/24`
  - `DatabaseSubnet` → `10.20.20.0/24`
- Export the deployment template (ARM)

---

### ✅ Task 2: Create a Virtual Network and Subnets (ARM Template)
- Modify exported template to deploy `ManufacturingVnet`
- Update address space to `10.30.0.0/16`
- Create subnets:
  - `SensorSubnet1` → `10.30.20.0/24`
  - `SensorSubnet2` → `10.30.21.0/24`
- Deploy using **Deploy a custom template**

---

### ✅ Task 3: Secure Communication Using ASG + NSG
- Create Application Security Group: `asg-web`
- Create Network Security Group: `myNSGSecure`
- Associate NSG to: `CoreServicesVnet` → `SharedServicesSubnet`
- Add inbound rule to allow web traffic from ASG:
  - Allow **TCP 80,443** from `asg-web` (Priority **100**)
- Add outbound rule to deny internet access:
  - Deny destination **Internet** using Service Tag (Priority **4096**)

---

### ✅ Task 4: Configure Public and Private Azure DNS Zones
**Public DNS Zone**
- Create DNS zone: `contoso.com` *(or adjusted name if unavailable)*
- Create `A` record:
  - `www` → `10.1.1.4` *(demo IP)*
- Validate with:
  ```bash
  nslookup www.contoso.com <Azure-DNS-name-server>
