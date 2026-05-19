# Challenge 00 - Prerequisites - Ready, Set, GO! - Coach's Guide

**[Home](./README.md)** - [Next Solution >](./Solution-01.md)

## Challenge Overview for Coaches

This challenge has no technical puzzle — its purpose is entirely operational: get every team member to a known-good starting state before the first real challenge begins. A team that skips or rushes through prerequisites will lose time to environment issues in Challenges 01–03 rather than learning Radius.

**Intended learning outcomes:**
- Teams understand the full tool chain they will use across the hack before any coding starts.
- Each team member has independently verified their own workstation (not just one person on the team).
- Teams have confirmed their Azure subscription permissions are sufficient — a common blocker that surfaces too late if ignored here.

**Key technologies and concepts:**
- Azure CLI, kubectl, Bicep CLI, Docker (or equivalent), .NET 10 SDK
- Azure subscription RBAC — Contributor + User Access Administrator (or Owner) required
- Azure resource provider registration
- Resources.zip packaging and distribution

---

## Learning Objectives Breakdown

| Task | Skills developed | Expected outcome |
|---|---|---|
| Install and verify CLI tools | Tool familiarity, environment hygiene | Each team member can run `az`, `kubectl`, `az bicep`, `docker`, and `dotnet` and get a valid version back |
| Verify Azure subscription access | Understanding Azure RBAC and subscription scope | Team knows they have sufficient permissions before hitting a wall in Challenge 01 |
| Register resource providers | Azure subscription management | No deployment failures in later challenges due to unregistered providers |
| Unpack Resources.zip | Awareness of provided assets | Team knows where the sample app, Dockerfiles, and Bicep snippets are before they need them |

---

## Expected Solution Approaches

There is no single prescribed sequence here. Teams may:

- Install tools in any order — none have hard dependencies on each other at this stage.
- Use package managers (Homebrew, winget, apt, Chocolatey) or direct downloads — both are valid.
- Use Azure Cloud Shell instead of a local workstation for `az` and `kubectl` — perfectly acceptable, though Docker and .NET 10 will still need to be on a local machine for Challenge 03.
- Share a single Azure subscription across the whole team or use individual subscriptions — either works as long as permissions are present.

The goal is a verifiable end state, not a prescribed installation path.

---

## Key Concepts & Teaching Points

- **Why all these tools?** Each tool maps to a specific challenge: `kubectl` is needed the moment Challenge 01 starts, Bicep CLI is needed in Challenge 02, and Docker + .NET 10 SDK land in Challenge 03. Installing them now prevents context-switching later.
- **Azure RBAC depth matters.** Contributor alone is not sufficient — creating role assignments (e.g., granting AcrPull to an AKS managed identity) requires User Access Administrator or Owner. This is a common gap in enterprise subscriptions provided for hacks.
- **Resource provider registration is a silent blocker.** `Microsoft.ContainerService`, `Microsoft.ContainerRegistry`, `Microsoft.KeyVault`, and `Microsoft.Storage` must all be registered. On a brand-new subscription they may not be. Registering takes only seconds but propagates asynchronously — up to 5 minutes.
- **`rad` CLI is intentionally absent here.** Installing Radius is the *learning objective* of Challenge 01, not a prerequisite to it. If a team asks to install it now, redirect them — the install experience is part of the next challenge's discovery.

---

## Common Pitfalls & Misconceptions

- **One person installs tools for the whole team.** Each team member must install and verify on their own workstation. A teammate's working `kubectl` does not help someone else in Challenge 01.
- **Skipping the version checks.** Teams often install a tool and assume it works. A stale `kubectl` or an old Azure CLI can cause subtle failures. Push teams to run the verify commands explicitly.
- **Assuming Contributor = sufficient.** Teams on corporate Azure subscriptions are frequently given Contributor without User Access Administrator. This will silently break role assignment steps in Challenges 01 and 02.
- **Forgetting to unpack Resources.zip.** Teams realize they need the sample app source mid-Challenge 03, then lose time finding it. Confirm the zip is unpacked now.
- **Installing the `rad` CLI in Challenge 00.** Not wrong, but it conflates two challenges. The installation experience — choosing a version, verifying the control plane — is intentional learning in Challenge 01. Early installs can produce version mismatches if the team installs a different version than what `rad install kubernetes` would pull.
- **Using Azure Cloud Shell as the only environment.** Cloud Shell does not have Docker or a .NET 10 SDK. Teams that rely solely on Cloud Shell will need to revisit their setup by Challenge 03.

---

## Coaching Guidance

Use Socratic questions to keep teams self-sufficient rather than creating a dependency on the coach.

**On tool installation:**
- "You've installed the Azure CLI — how would you confirm the version is recent enough?"
- "If `kubectl` is installed but `az aks get-credentials` hasn't been run yet, what will `kubectl get nodes` show?"

**On Azure permissions:**
- "What permission is needed to assign a role to a managed identity in Azure — and does your account have it?"
- "How would you find out whether the `Microsoft.ContainerService` provider is registered in your subscription before trying to create an AKS cluster?"

**On Resources.zip:**
- "Challenge 03 uses a sample .NET 10 app. Where is the source code right now on your workstation?"

**On team coordination:**
- "Has every team member verified their own setup, or just one person?"

---

## Hint Strategy Guidance

This challenge is low-stakes and largely mechanical. The hint bar is lower than in later challenges.

- **If a tool won't install:** It is appropriate to direct teams to the official install docs immediately — debugging package managers is not a learning objective. Do not let teams spend more than 5 minutes on a single tool install.
- **If Azure permissions are missing:** Ask the team to describe what `az role assignment list --assignee <upn>` shows before offering a fix. The goal is that they know *why* the permission is missing, not just that they need to ask their admin.
- **If resource providers are unregistered:** Ask them to run `az provider show --namespace Microsoft.ContainerService --query "registrationState"` first. Once they see `"NotRegistered"`, the fix (`az provider register`) is obvious. No need to give it unprompted.
- **General rule:** If a team is stuck on prerequisites for more than 10 minutes, step in — this challenge has no discovery value in its blocked state.

---

## Validation Guidance

A team has successfully completed this challenge when **every team member individually** can demonstrate:

- `az account show` returns the correct subscription, and the account has at least Contributor + User Access Administrator (or Owner).
- `kubectl version --client` returns a valid version.
- `az bicep version` returns a valid Bicep version.
- `docker version` (or equivalent) returns a valid version.
- `dotnet --version` returns a 10.x version.
- The Resources.zip has been unpacked and the `Challenge-03` folder with the sample application is accessible.

**Acceptable variations:**
- Any compatible version of each tool is fine — teams do not need to match a specific patch version.
- Docker Desktop, Podman Desktop, Rancher Desktop, or Buildah are all acceptable container build tools.
- WSL 2 on Windows is a valid environment for all tools.
- Azure Cloud Shell is acceptable for `az` and `kubectl` only — Docker and .NET 10 must be local.

**Not acceptable:**
- Only one team member has verified tools and the rest have not.
- The team is relying on "it should work" without running the verify commands.

---

## Optional Demo / Discussion Points

- **What is Radius, and why are we here?** If the team has not seen the Radius overview, this is a good moment for a 5-minute intro. The [What is Radius?](https://docs.radapp.io/concepts/) page is a concise starting point. Frame it as: "By the end of this hack, a developer writes one resource declaration and the platform decides whether it runs on Azure or on-premises — let's talk about how that works."
- **Azure subscription hygiene discussion.** Ask teams: "Have you worked with AKS or ACR before? What permissions did you need?" This surfaces any subscription concerns before they block Challenge 01.
- **Whiteboard: the tool chain.** A short whiteboard sketch mapping each tool to the challenge it first appears in helps teams understand why the list is what it is — and builds anticipation for the later challenges.
