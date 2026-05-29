---
title: "Cloud Hygiene for client environments"
date: 2026-05-29
---

It's important to review and improve the overall health and hygiene of cloud environments. With Azure, services retire, operating systems reach end of support, and platform requirements change over time.

I would like to share a real-world example of this which ultimately led to an internal initiative.

We support a range of client environments. Some are built from the ground up, while many others are existing environments that we inherit and support. In many cases, we are starting on the back foot.

I led a larger effort to provide a sustainable way for our engineering teams to assess and improve our clients' environments. A large portion of that process was created organically while working through a client engagement.

## The Environment Review

The work started during discussions around performing a health review of a client's Azure environment.

I provided my findings and shared a report covering several areas that required attention. The client was eager to improve the environment and begin addressing the findings.

What initially appeared to be a straightforward environment review quickly turned into multiple interconnected projects.

There were several services and resources within the Azure environment that were approaching retirement or deprecation within the next year. At the same time, the client was preparing for a compliance audit and planning upgrades to their firewall infrastructure.

A few examples included:

* VMs using unmanaged disks
* Public IPs running on a retiring SKU
* Load Balancers running on a retiring SKU
* Storage accounts and applications using an API version Azure would eventually remove
* Domain Controllers approaching end of support
* Application servers approaching end of support

Among other items.

None of these recommendations could be evaluated independently. Many of them affected one another, which required careful planning around priorities, timelines, dependencies, testing, and maintenance windows.

## Retirement and Upgrade Planning

This involved multiple meetings and discussions throughout the year.

Upgrading VMs to a supported operating system version introduced several additional considerations.

The VMs were running in Availability Sets. If we wanted to migrate them from unmanaged disks to managed disks, we also needed to evaluate the Availability Set configuration.

https://learn.microsoft.com/en-us/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks

Some VMs were able to move forward immediately, while others had dependencies that prevented us from doing so.

There were also two pairs of VMs acting as file servers. These systems were tied closely to Active Directory and DFS namespaces.

Before making changes, we needed to document how users authenticated, what DFS paths were being used, and whether any of those paths would need to change during migration.

https://learn.microsoft.com/en-us/windows-server/storage/dfs-namespaces/dfs-overview?tabs=server-manager

## Active Directory Considerations

The Domain Controllers introduced another layer of planning.

We spent time researching the differences between the existing Windows Server 2012 R2 environment and the planned Windows Server 2022 deployment.

Based on the documentation below, Windows Server 2022 Domain Controllers could coexist with a Windows Server 2012 R2 forest.

https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/active-directory-functional-levels#windows-server-2012-r2-functional-levels

https://learn.microsoft.com/en-us/answers/questions/1609978/upgrading-moving-the-ad-server-from-2012-r2-datace

https://learn.microsoft.com/en-us/answers/questions/1858256/can-i-have-a-mix-of-server-2008-2012-and-server-20

However, there were still important considerations.

For example:

* Ensuring SYSVOL replication had been migrated from FRS to DFSR
* Validating replication health
* Planning FSMO role transfers
* Verifying application dependencies before introducing new Domain Controllers

https://serverfault.com/questions/1147588/replacing-domain-controller-with-server-2022

https://learn.microsoft.com/en-us/windows-server/storage/dfs-replication/migrate-sysvol-to-dfsr

These details may seem small individually, but they become important when multiple upgrade projects are occurring simultaneously.

## The Managed Disk Decision

The size of the VM disks also played a role in planning.

Larger disks meant longer migration windows. Some workloads were better candidates for a rebuild, while others were better candidates for an in-place migration to managed disks.

After extensive research and testing, we created a phased approach.

The plan looked something like:

1. Domain Controllers
2. File Servers
3. Test Servers
4. Production Application Servers

The order mattered because many of the systems depended on one another.

The client and I ultimately needed to make tradeoff decisions.

One example was the unmanaged disk retirement timeline.

We had two possible approaches:

* Rebuild the VM on a supported operating system and automatically land on managed disks.
* Migrate the existing VM to managed disks first and address the unsupported operating system later.

The first option solved multiple problems at once. However, it required application owners and developers to validate their workloads on rebuilt systems.

The second option was quicker but would leave additional work for later.

This is where cloud hygiene becomes more than simply implementing a recommendation.

The client's development teams needed enough time to validate their applications. Some workloads could be rebuilt before the retirement deadline. Others could not.

We decided to rebuild the VMs where application validation could be completed in time. For the remaining systems, we performed the unmanaged disk migration first and scheduled the operating system upgrades separately.

The rebuilt systems also gave us an opportunity to validate portions of the unmanaged disk migration process before performing larger migrations.

> Note:
>
> Near the end of the project, Microsoft extended the unmanaged disk retirement timeline.
>
> While the additional time was helpful, much of the planning, testing, prioritization, and decision-making had already been completed.

Retirement announcements can be tracked here:

https://azure.microsoft.com/en-us/updates?filters=%5B%22Retirements%22%5D

## Why Cloud Hygiene Is More Than Recommendations

I share this example because cloud hygiene is about more than identifying recommendations.

Throughout each phase of this work, understanding the client's business needs, technical requirements, project timelines, compliance goals, and operational constraints influenced how the work was approached.

The recommendations themselves were only one part of the process.

Coordinating between teams was equally important. Many of the projects were connected in some way. If one project was completed without understanding the timing or impact of another, it could affect production systems or delay other work already in progress.

The technical work mattered, but so did understanding how each change fit into the larger environment.

Cloud hygiene is not simply finding issues.

It's understanding how to address them in a way that works for the environment, the people supporting it, and the business relying on it.

## Useful Tools

Some of the tools I use during environment reviews include:

https://github.com/dolevshor/azure-orphan-resources

https://learn.microsoft.com/en-us/azure/advisor/advisor-workbook-service-retirement

https://github.com/mathijsvermaat/Defender-AMA-coverage/blob/main/Defender_vs_AMA.json
