# What The Hack - PortableAppWithRadius

## Introduction

Modern applications need to run in multiple environments — cloud, on-premises, edge — without being rewritten for each one. This hack introduces [Radius](https://radapp.io), an open-source, cloud-native application platform that lets platform engineers define portable, environment-agnostic infrastructure abstractions that developers can consume without knowing the cloud plumbing underneath.

You will play the role of a platform engineering team at Contoso. Your mission: build a Radius-based platform that lets a .NET 10 web application and its PostgreSQL database run identically on Azure and on Azure Local — same container image, same resource declarations, zero application-level changes between environments.

## Learning Objectives

In this hack you will:

1. Install and operate the Radius control plane on an AKS cluster
2. Define user-defined Radius resource types and author environment-specific recipes using Bicep and Azure Verified Modules
3. Deploy a portable .NET 10 web application connected to a PostgreSQL database using Radius connections, running identically on Azure App Service and AKS

## Challenges

- Challenge 00: **[Prerequisites - Ready, Set, GO!](Student/Challenge-00.md)**
	 - Prepare your workstation to work with Azure.
- Challenge 01: **[Install and Configure the Radius Control Plane](Student/Challenge-01.md)**
	 - Prepare a Kubernetes cluster, install the `rad` CLI, and deploy the Radius control plane and an initial environment that will be used by the later challenges.
- Challenge 02: **[Define a PostgreSQL Resource Type and Author Recipes for Azure and Azure Local](Student/Challenge-02.md)**
	 - Define a user-defined `Radius.Resources/postgreSQL` resource type and author two recipes: one that provisions an Azure Database for PostgreSQL Flexible Server via an Azure Verified Module, and one that runs PostgreSQL as a container on AKS for an Azure Local environment.
- Challenge 03: **[Deploy a Portable .NET 10 Web App on Azure and Azure Local with Radius](Student/Challenge-03.md)**
	 - Define a `Radius.Resources/webApp` resource type, author Azure App Service and AKS recipes, and wire the web app to the PostgreSQL resource from Challenge 02 using Radius connections — with the same container image running unchanged in both environments.
- Challenge 04: **[Title of Challenge](Student/Challenge-04.md)**
	 - Description of challenge
- Challenge 05: **[Title of Challenge](Student/Challenge-05.md)**
	 - Description of challenge
- Challenge 06: **[Title of Challenge](Student/Challenge-06.md)**
	 - Description of challenge
- Challenge 07: **[Title of Challenge](Student/Challenge-07.md)**
	 - Description of challenge
- Challenge 08: **[Title of Challenge](Student/Challenge-08.md)**
	 - Description of challenge
- Challenge 09: **[Title of Challenge](Student/Challenge-09.md)**
	 - Description of challenge
- Challenge 10: **[Title of Challenge](Student/Challenge-10.md)**
	 - Description of challenge

## Prerequisites

- An Azure subscription with Owner (or Contributor + User Access Administrator) access
- Azure CLI (v2.60 or later)
- kubectl
- Bicep CLI (`az bicep install`)
- Docker (or equivalent container build tool)
- .NET 10 SDK
- Visual Studio Code with the [Bicep extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-bicep)

## Contributors

- Djong1

