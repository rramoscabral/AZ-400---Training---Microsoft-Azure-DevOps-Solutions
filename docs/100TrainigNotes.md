---
layout: default
title: 'Training Notes'
nav_order: 10
has_children: false
---

# Training Notes
{: .no_toc }


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

<br/>

---

<br/>
The following training notes are provided to students during the training. Note that the entire conversation is not included, nor are all notes from all training sessions given.

<br/>

## Day one

```
Trainer resources: https://az-400.rramoscabral.com/

Course manual: https://az-400.rramoscabral.com/docs/CourseSyllabus202405.html

View Azure Pass balance: https://www.microsoftazuresponsorships.com/Balance


------------------------------------------

Hands-on labs instructions:
 
https://aka.ms/az400-labs
 
https://microsoftlearning.github.io/mslearn-devops/

------------------------------------------

Create or reuse GitHub account for Hands-on Labs
https://github.com/signup


------------------------------------------



[DevOps]

- Donovan Brown: https://twitter.com/donovanbrown
- What is DevOps? with Donovan Brown: https://devblogs.microsoft.com/devops/what-is-devops-donovan/
- Donovan Brown Slide Decks (erro de certificado): https://www.donovanbrown.com/page/slide-decks
- What is DevOps?: https://learn.microsoft.com/en-us/devops/what-is-devops

------------------------------------------

Agile principles
- https://agilemanifesto.org/iso/ptpt/principles.html
- https://www.agilealliance.org/agile101/the-agile-manifesto/


12 Principles Behind the Agile Manifesto
- https://www.agilealliance.org/agile101/12-principles-behind-the-agile-manifesto/

------------------------------------------


Azure DevOps (Enterprise)
https://dev.azure.com/[organization]/[project]

GitHub (Open-Source / Collaborative / Enterprise)
https://www.github.com/[username/organization]/[repository]

- Username: Person
- Organization: organizational account

------------------------------------------

[GIT]

ebook: https://git-scm.com/book/en/v2

------------------------------------------

[Semantic Versioning]

https://semver.org/

- New Application: 1.0.0
- Bug fix: 1.0.1
- New feature: 1.1.1
- Restructuring of the application incompatible with the previous version: 2.0.0


-----------------

[Git Large File Storage (LFS)]

Git extension for versioning large files: https://git-lfs.com/

https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github



-----------------


Scalar: A set of tools and extensions for Git to allow very large monorepos to run on Git without a virtualization layer
- https://github.com/microsoft/scalar


----------------

Visual Studio, Visual Studio Code, or Azure DevOps maketplace extensions

https://marketplace.visualstudio.com/azuredevops

-----------------

Advanced Security billing
https://learn.microsoft.com/en-us/azure/devops/repos/security/github-advanced-security-billing?view=azure-devops&pivots=standalone-ghazdo


About GitHub Advanced Security
https://docs.github.com/en/get-started/learning-about-github/about-github-advanced-security
-----------------


```

---

## Day Two

**GitHub Workflows**

<br/>

**GitHub Actions flow**

```mermaid
  graph TD;
    A[Events] -->|Trigger| B[Workflows]
    B --> |contains| C[Jobs]
    C --> |uses| D[Actions]

```
> Graph made using Mermaid markdown

<br/>

**Syntax elements**

* **Name:** The name of the workflow. GitHub displays the names of your workflows under your repository's "Actions" tab. If you omit name, GitHub displays the workflow file path relative to the root of the repository.
* **On:** To automatically trigger a workflow, use on to define which events can cause the workflow to run
* **Jobs:** A workflow run is made up of one or more jobs, which run in parallel by default. 
* **Runs-on:** Use jobs.<job_id>.runs-on to define the type of machine to run the job on.
* **Steps:** A job contains a sequence of tasks called steps. Steps can run commands, run setup tasks, or run an action in your repository, a public repository, or an action published in a Docker registry. Not all steps run actions, but all actions run as a step.
* **Uses:** Selects an action to run as part of a step in your job. An action is a reusable unit of code. You can use an action defined in the same repository as the workflow, a public repository, or in a published Docker container image.
* **Run:** Runs command-line programs that do not exceed 21,000 characters using the operating system's shell. If you do not provide a name, the step name will default to the text specified in the run command.

> Source: https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions

<br/>



**Workflow Status badge**

Example from [blank.yml](https://raw.githubusercontent.com/CloudAndDevelopment/GitHubWorkflows/main/.github/workflows/blank.yml)

[![CI](https://github.com/CloudAndDevelopment/GitHubWorkflows/actions/workflows/blank.yml/badge.svg)](https://github.com/CloudAndDevelopment/GitHubWorkflows/actions/workflows/blank.yml)


```

-----------------


Using the Agile Testing Quadrants: https://lisacrispin.com/2011/11/08/using-the-agile-testing-quadrants/

-----------------

Memaid Live editor: https://mermaid.live/edit

-----------------


Change project visibility to private
https://learn.microsoft.com/en-us/azure/devops/organizations/projects/make-project-public?view=azure-devops

----------------------

Dockerfile reference
https://docs.docker.com/engine/reference/builder/

--------------------

```


---

## Day Three

```

Feature Toggles (aka Feature Flags)
https://martinfowler.com/articles/feature-toggles.html

--------------------

Graph Explorer
https://developer.microsoft.com/en-us/graph/graph-explorer

--------------------

What are ARM templates?
https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/overview

What is Bicep?
https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/overview?tabs=bicep

--------------------

Security in DevOps (DevSecOps)
https://learn.microsoft.com/en-us/devops/operate/security-in-devops

Microsoft Security Development Lifecycle (SDL)
https://www.microsoft.com/en-us/securityengineering/sdl/?msockid=26301c20b6d3677c14cb0ab2b798665c

--------------------

Microsoft Threat Modeling Tool
https://learn.microsoft.com/en-us/azure/security/develop/threat-modeling-tool

--------------------

Open Source iniatitive
https://opensource.org/
 
--------------------

```