# Challenge 00 - Prerequisites - Ready, Set, GO!

**[Home](../README.md)** - [Next Challenge >](./Challenge-01.md)

## Introduction

Thank you for participating in the PortableAppWithRadius What The Hack. Before you can hack, you will need to set up some prerequisites on your workstation and verify access to an Azure subscription.

This hack has you playing the role of a platform engineer building a portable application platform with [Radius](https://radapp.io). The tools you install here are used in every subsequent challenge — take the time to verify each one is working before moving on.

## Common Prerequisites

We have compiled a list of common tools and software that will come in handy to complete most What The Hack Azure-based hacks!

<!-- If you are editing this template manually, be aware that these links are only designed to work if this Markdown file is in the /xxx-HackName/Student/ folder of your hack. -->

- [Azure Subscription](../../000-HowToHack/WTH-Common-Prerequisites.md#azure-subscription)
- [Windows Subsystem for Linux](../../000-HowToHack/WTH-Common-Prerequisites.md#windows-subsystem-for-linux)
- [Managing Cloud Resources](../../000-HowToHack/WTH-Common-Prerequisites.md#managing-cloud-resources)
  - [Azure Portal](../../000-HowToHack/WTH-Common-Prerequisites.md#azure-portal)
  - [Azure CLI](../../000-HowToHack/WTH-Common-Prerequisites.md#azure-cli)
    - [Note for Windows Users](../../000-HowToHack/WTH-Common-Prerequisites.md#note-for-windows-users)
  - [Azure Cloud Shell](../../000-HowToHack/WTH-Common-Prerequisites.md#azure-cloud-shell)
- [Visual Studio Code](../../000-HowToHack/WTH-Common-Prerequisites.md#visual-studio-code)

## Description

Your coach will provide you with a **Resources.zip** file that contains sample application code, Dockerfiles, and Bicep snippets used in later challenges. Unpack it on your workstation before proceeding. If you are using Azure Cloud Shell, upload and unpack it there instead.

### Azure requirements

You need an Azure subscription in which you have permission to:

- Create and manage resource groups, AKS clusters, Azure Container Registry (ACR), Azure Key Vault, and Azure Storage accounts.
- Create role assignments (i.e., Owner or User Access Administrator on the subscription or resource group).
- Register Azure resource providers (the `Microsoft.ContainerService`, `Microsoft.ContainerRegistry`, `Microsoft.KeyVault`, and `Microsoft.Storage` providers must be registered).

### Workstation tools

Install the following tools on every team member's workstation:

- **Azure CLI** (`az`) — version 2.60 or later. Verify with `az version`.
- **kubectl** — the Kubernetes command-line tool, compatible with the AKS version you will provision. Verify with `kubectl version --client`.
- **Bicep CLI** — installed as an Azure CLI extension (`az bicep install`) or as a standalone binary. Verify with `az bicep version`.
- **Docker** (or an equivalent container build tool such as Buildah) — required in Challenge 03 to build and push the .NET 10 application container image. Verify with `docker version`.
- **.NET 10 SDK** — required in Challenge 03 to build and run the sample web application. Download from [https://dot.net](https://dot.net). Verify with `dotnet --version`.
- **Visual Studio Code** (recommended) with the [Bicep extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-bicep) for authoring recipes in Challenges 02 and 03.

> **NOTE:** The `rad` CLI (the Radius command-line tool) is installed as part of Challenge 01 — you do not need it here.

## Success Criteria

To complete this challenge successfully, you should be able to:

- Verify that `az account show` returns your target Azure subscription and that your account has at least Contributor + User Access Administrator (or Owner) on it.
- Verify that `kubectl version --client` returns a valid client version.
- Verify that `az bicep version` returns a valid Bicep version.
- Verify that `docker version` (or your chosen container build tool) returns a valid version.
- Verify that `dotnet --version` returns a .NET 10.x version.
- Show that the Resources.zip file has been unpacked and the `Challenge-03` folder containing the sample application source code is accessible.

## Learning Resources

- [What is Radius?](https://docs.radapp.io/concepts/) — overview of the platform you will be building on throughout this hack.
- [Azure CLI — get started](https://learn.microsoft.com/cli/azure/get-started-with-azure-cli)
- [Install kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Install the Bicep CLI](https://learn.microsoft.com/azure/azure-resource-manager/bicep/install)
- [.NET 10 downloads](https://dot.net/download)
- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
