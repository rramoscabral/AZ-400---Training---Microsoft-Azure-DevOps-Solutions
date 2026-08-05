---
layout: default
title: 'Lab 7: Configure Pipelines as Code with YAML'
nav_order: 6.07
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

# Lab 7: Configure Pipelines as Code with YAML

The AZ CLI to create the web app is missing the runtime parameter. This situation was detected on 2026-08-05.

## If no organization appears when accessing Azure DevOps

Try the following URLs:
- `https://dev.azure.com/ADOCourseOrg01/`
- `https://dev.azure.com/ADOCourseOrg03/`
- `https://dev.azure.com/ADOCourseOrg04/`



## Create Azure resources

```bash
$ LOCATION='westeurope'
$ RESOURCEGROUPNAME='az400m03l07-RG'
$ SERVICEPLANNAME='az400m03l07-sp1'
$ az appservice plan create --resource-group $RESOURCEGROUPNAME --name $SERVICEPLANNAME --sku B3 --location $LOCATION
```

```bash
$ az provider register --namespace Microsoft.Web
$ WEBAPPNAME=eshoponWebYAML63979820
$ az webapp list-runtimes --os-type linux --output tsv
$ az webapp create --resource-group $RESOURCEGROUPNAME --plan $SERVICEPLANNAME --name $WEBAPPNAME --runtime "DOTNETCORE|10.0"
```


