---
layout: default
title: 'Lab 3: Configure Agent Pools and Understand Pipeline Style
nav_order: 6.3
has_children: false
parent: 'Hands-on Labs Issues'
---

# Software list for Hands-On Labs
{: .no_toc }


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

<br/>

---

<br/>

# Lab 3: Configure Agent Pools and Understand Pipeline Style


This situation was detected on 2026-07-06.


If no organization appears when accessing Azure DevOps, try the following URLs:
- `https://dev.azure.com/ADOCourseOrg01/`
- `https://dev.azure.com/ADOCourseOrg03/`
- `https://dev.azure.com/ADOCourseOrg04/`




In the "Create agents and configure agent pools" step, instead of choosing **Presets**, choose  **Virtual Machine**.

> Note: Azure Presets is pre-built configurations based on workload.
 
Settings 
- **Subscription:** `Ignite-lod52862840`
- **Resource group:** `rg-eshoponweb-agentpool`
- **Virtual machine name:** `eshoponweb-vm`
- **Region:** `East US 2`
- **Availability options:** `No infrastructure redundancy`
- **Security type:** `Standard`
- **Image:** `Windows Server 2022 Datacenter: Azure Edition - Gen2 VM architecture x64`
- **Size:** `Standard D2s v3 (2 vcpus, 8 GiB memory)`
- **Enable Hibernation:** `No`
- **Username:** `microsoft`
- **Password:** `Pa55w.rd2026`
- **Public inbound ports:** `RDP`
- **Already have a Windows license?:** `No`
- **Azure Spot:** `No`



