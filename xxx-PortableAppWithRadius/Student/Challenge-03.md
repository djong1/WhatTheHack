# Challenge 03 - Deploy a Portable .NET 10 Web App on Azure and Azure Local with Radius

[< Previous Challenge](./Challenge-02.md) - **[Home](../README.md)** - [Next Challenge >](./Challenge-04.md)

## Pre-requisites

- Completion of Challenge 02: your `Radius.Resources/postgreSQL` resource type must be registered with working recipes for both the `azure` and `azurelocal` environments.
- Access to a container registry (Azure Container Registry or equivalent) that both your Azure App Service and AKS environments can pull from.
- The sample .NET 10 application source code and Dockerfile located in the `Challenge-03` folder of the Resources.zip file provided by your coach.

## Introduction

In Challenge 02, your team built the **data tier** of the Contoso portable application platform: a `Radius.Resources/postgreSQL` resource type backed by two environment-specific recipes. This challenge is the **application tier counterpart**.

The centerpiece of this challenge is a Radius concept called **connections**. A connection is a declarative dependency edge that a consuming resource (the web app) declares toward a providing resource (the database). When Radius processes the deployment, it reads the `values` and `secrets` outputs of the target resource — the `host`, `port`, `connectionString`, and other fields your team shaped in Challenge 02 — and injects them into the consuming resource as environment variables, automatically. The application developer writes `connections: { db: { source: pg.id } }` once. Whether that translates into an App Service connection string, a Kubernetes `EnvFrom`, a firewall rule, or a private endpoint is a *platform* decision — not an *application* decision.

The second key concept is **recipe parity**: the same .NET 10 container image and the same Radius resource declaration must run unchanged on both Azure App Service and an AKS cluster on Azure Local. If you find yourself writing environment-specific logic inside the application or in the Bicep resource block, that is the anti-pattern this challenge is designed to eliminate.

## Description

Contoso's platform engineering team must extend the Radius-based platform to support portable web applications. Development teams are shipping .NET 10 containerized services that need to run on Azure (cloud production workloads) and on Azure Local (regulated on-premises workloads) without any code changes between environments. The platform must abstract away all infrastructure differences — including how the web app reaches the database — so that application teams never need to think about VNets, firewall rules, or cluster topology.

Your team's mission is to design and implement the application tier of this portable platform.

You can find the sample .NET 10 web application and its Dockerfile in the `Challenge-03` folder of the Resources.zip file provided by your coach. Use it as the workload your platform will host.

### Define the `Radius.Resources/webApp` Resource Type

Design and publish a new resource type that captures the portable contract for a web application. Consider what properties an application author needs to declare (image reference, listening port, environment variables, replica count) and what outputs the platform should expose back to consumers (the public URL, an internal hostname). Think carefully about which capabilities the resource type must declare for Radius to process the `connections` block and to back the resource with environment-specific recipes.

### Author the Azure Recipe

Design a Bicep recipe that deploys the web app on **Azure App Service for Containers** using Azure Verified Modules (AVM). The recipe must provision the necessary App Service Plan and Web App from the AVM catalog, map the portable resource properties to the appropriate App Service settings, and emit the outputs Radius expects. Explore the AVM catalog at `https://aka.ms/avm` for the correct module references; check for the latest available version on the day of the event. Apply secure-by-default settings (HTTPS only, minimum TLS version, FTPS disabled) — examine what the AVM defaults give you before writing your own enforcement.

Consider how the App Service Plan should be structured so that multiple web apps in the same environment could share a pre-provisioned plan rather than each creating their own.

### Author the Azure Local Recipe

Design a Bicep recipe using the **Bicep `kubernetes` provider** that deploys the same container image to AKS. The recipe must expose the application through the cluster's ingress controller. The portable input properties and output shape must match the Azure recipe exactly — both recipes implement the same resource type contract.

### Connect the Web App to PostgreSQL

Use the `connections` block in your web app's Bicep resource declaration to express a dependency on the `postgreSQL` resource from Challenge 02. Once the connection is declared, investigate how Radius injects the database's published outputs into the web app — what environment variable names does the .NET 10 application need to read? Ensure the application reads its database configuration exclusively from those injected environment variables rather than from hard-coded values.

### Make the Networking Declarative

The networking path between the web app and the database must be **fully declarative and part of the recipes** — not applied by hand after `rad deploy` completes.

On **Azure**, investigate how App Service publishes its outbound IP addresses and how a firewall rule on the Postgres flexible server can be authored inside the recipe to allow traffic from exactly those IPs. On **Azure Local**, investigate how a Kubernetes `NetworkPolicy` can restrict database pod access to only the web app pod. In both cases, the rule must be created and torn down as part of the recipe lifecycle — not managed separately.

The database must remain inaccessible to the public internet throughout this challenge.

## Success Criteria

To complete this challenge successfully, demonstrate the following to your coach:

- Verify that the `Radius.Resources/webApp` resource type is registered in your Radius environment alongside the `Radius.Resources/postgreSQL` resource type from Challenge 02.
- Verify that a single `rad deploy` command (with no manual steps afterward) deploys the web app on **Azure** and the application successfully reads and writes data through the PostgreSQL connection.
- Verify that a single `rad deploy` command (with no manual steps afterward) deploys the web app on **Azure Local** and the application successfully reads and writes data through the PostgreSQL connection.
- Demonstrate that the **same container image and the same `connections` block** are used for both environments — show there is no environment-specific branching in the application code or in the Bicep resource declaration.
- Show that the firewall rule (Azure) or `NetworkPolicy` (Azure Local) permitting the web app to reach the database is **generated by the recipe**, not applied manually — for example, by deleting and redeploying the web app and observing that connectivity is automatically restored.
- Verify that the web app is accessible over HTTPS and returns a valid response at the published URL in both environments.
- Demonstrate that the database has **no public internet firewall rule** (no `0.0.0.0/0` allowance) in either environment.

## Learning Resources

- [Radius Connections — how application resources declare dependencies](https://docs.radapp.io/guides/author-apps/connections/)
- [Radius Resource Types and Recipes overview](https://docs.radapp.io/guides/recipes/)
- [Azure Verified Modules catalog](https://aka.ms/avm)
- [AVM module: `avm/res/web/site` (App Service Web App)](https://github.com/Azure/bicep-registry-modules/tree/main/avm/res/web/site)
- [AVM module: `avm/res/web/serverfarm` (App Service Plan)](https://github.com/Azure/bicep-registry-modules/tree/main/avm/res/web/serverfarm)
- [Bicep Kubernetes provider](https://learn.microsoft.com/azure/azure-resource-manager/bicep/bicep-extensibility-kubernetes-provider)
- [Configure a custom container on Azure App Service](https://learn.microsoft.com/azure/app-service/configure-custom-container)
- [Inbound and outbound IP addresses in Azure App Service](https://learn.microsoft.com/azure/app-service/overview-inbound-outbound-ips)
- [Kubernetes NetworkPolicy](https://kubernetes.io/docs/concepts/services-networking/network-policies/)
- [ASP.NET Core configuration via environment variables](https://learn.microsoft.com/aspnet/core/fundamentals/configuration/)

## Tips

**On the resource type:**
- A resource type that declares connections must advertise a specific capability in its manifest for Radius to honor the `connections` block — without it, connections are silently ignored. Explore the Radius resource type documentation to find the right capability name.
- Look at the resource type manifest you published in Challenge 02 as a structural reference. The `webApp` manifest follows the same shape; the key differences are the property schema and the capabilities list.

**On the Azure recipe:**
- App Service Free and Shared plan tiers do **not** publish a stable, enumerable set of outbound IP addresses. Use at least a Basic plan during this hack, or the networking stage will not work reliably.
- App Service needs to know which port your container listens on. If the application is not responding, check whether the correct app setting is configured to tell App Service the container's listening port — and verify what port the .NET 10 ASP.NET Core image actually binds to at startup.
- The AVM for App Service site accepts a `siteConfig` object for container settings and a separate `appSettingsKeyValuePairs` parameter for environment variables. Review the AVM parameter schema before designing your recipe inputs.

**On connections and networking:**
- Radius injects a connected resource's outputs as environment variables into the consumer following a predictable naming pattern. Inspect a deployed web app's environment variables (via the Azure portal or `kubectl exec`) to see exactly what names Radius generated — then verify your .NET 10 app is reading those names.
- Think carefully about *where* the firewall rule or `NetworkPolicy` belongs in the resource lifecycle. It is a **per-connection concern**: it should be created when the web app is deployed and removed when the web app is deleted. Ask yourself which recipe owns that lifecycle.
- On Azure Local, run `kubectl get ingressclass` and verify an ingress controller is installed before troubleshooting routing issues. Also confirm your AKS cluster's CNI plugin enforces `NetworkPolicy` — some CNI configurations ignore policy objects silently.

## Advanced Challenges

Too comfortable? Eager to do more? Try these additional challenges!

- Extend the `Radius.Resources/webApp` resource type to accept an optional `appServicePlanId` parameter, allowing multiple web app resources in the same environment to share a pre-provisioned App Service Plan rather than each creating their own.
- Add `minReplicas` and `maxReplicas` properties to the resource type and implement autoscaling in both recipes — KEDA-based horizontal pod autoscaling on AKS and built-in App Service autoscale rules on Azure.
- Register a third recipe for the `webApp` resource type that deploys the container as an **Azure Container Apps** revision, and validate it in a new Radius environment.
