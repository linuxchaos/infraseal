---
title: "Cloud Hygiene for Client Environments"
date: 2026-05-29
---

Cloud environments need regular review. Services retire, operating systems reach end of support, platform requirements change, and decisions that made sense a few years ago may need to be revisited.

I want to share a real-world example that eventually led to a broader internal initiative around client environment reviews.

We support a range of client environments. Some are built from the ground up. Others are environments we inherit, support, and improve over time. In inherited environments, we are often starting with unknown dependencies, older design decisions, and operational habits that were never fully documented.

## The Environment Review

This work started during discussions about performing a health review of a client's Azure environment.

I shared a findings report that covered several areas needing attention. The client wanted to improve the environment and begin addressing the recommendations. What first looked like a straightforward review quickly became several connected projects.

At the time of the review, multiple services and resources were approaching retirement, deprecation, or support deadlines. The client was also preparing for a compliance audit and planning firewall upgrades.

Examples included:

- VMs using unmanaged disks
- Public IPs running on a retiring SKU
- Load Balancers running on a retiring SKU
- Storage accounts and applications using API versions that would eventually need to change
- Domain Controllers approaching end of support
- Application servers approaching end of support

Those findings could not be treated independently. Many of them affected one another, so the work required planning around priority, timeline, dependency, testing, ownership, and maintenance windows.

## Retirement and Upgrade Planning

Upgrading VMs to supported operating system versions introduced additional decisions.

The VMs were running in Availability Sets. If we wanted to migrate them from unmanaged disks to managed disks, we also needed to evaluate the Availability Set configuration.

<https://learn.microsoft.com/en-us/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks>

Some VMs could move forward immediately. Others had dependencies that prevented a simple migration.

Two pairs of VMs were acting as file servers and were tied closely to Active Directory and DFS namespaces. Before making changes, we needed to document how users authenticated, which DFS paths were being used, and whether any paths would need to change during migration.

<https://learn.microsoft.com/en-us/windows-server/storage/dfs-namespaces/dfs-overview?tabs=server-manager>

That is where environment hygiene becomes more than a recommendation list. A disk migration, OS upgrade, or rebuild can affect users, authentication paths, file access, backup strategy, and application dependencies.

## Active Directory Considerations

The Domain Controllers added another planning layer.

We researched the differences between the existing Windows Server 2012 R2 environment and the planned Windows Server 2022 deployment. Based on Microsoft documentation and validation work, Windows Server 2022 Domain Controllers could coexist with a Windows Server 2012 R2 forest.

<https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/active-directory-functional-levels#windows-server-2012-r2-functional-levels>

<https://learn.microsoft.com/en-us/answers/questions/1609978/upgrading-moving-the-ad-server-from-2012-r2-datace>

<https://learn.microsoft.com/en-us/answers/questions/1858256/can-i-have-a-mix-of-server-2008-2012-and-server-20>

There were still important checks to complete:

- Confirm SYSVOL replication had been migrated from FRS to DFSR
- Validate replication health
- Plan FSMO role transfers
- Review application dependencies before introducing new Domain Controllers
- Confirm backup and rollback expectations before making identity changes

<https://serverfault.com/questions/1147588/replacing-domain-controller-with-server-2022>

<https://learn.microsoft.com/en-us/windows-server/storage/dfs-replication/migrate-sysvol-to-dfsr>

Each item may look small on its own. During a larger modernization effort, those details matter because several changes are happening close together.

## The Managed Disk Decision

The VM disk sizes also affected planning.

Larger disks meant longer migration windows. Some workloads were better candidates for a rebuild. Others were better candidates for an in-place migration to managed disks.

After research and testing, we built a phased approach:

1. Domain Controllers
2. File Servers
3. Test Servers
4. Production Application Servers

The order mattered because many systems depended on one another.

The client and I also had to make tradeoff decisions. For example, we had two possible approaches for systems affected by the unmanaged disk retirement timeline:

- Rebuild the VM on a supported operating system and land on managed disks as part of the rebuild.
- Migrate the existing VM to managed disks first and address the unsupported operating system later.

The first option solved more problems at once, but it required application owners and developers to validate their workloads on rebuilt systems.

The second option was quicker, but it left operating system upgrades for a later phase.

The right answer depended on the workload, the deadline, the available testing window, and the client's ability to validate applications. We rebuilt the systems where validation could be completed in time. For the remaining systems, we migrated unmanaged disks first and scheduled operating system upgrades separately.

The rebuilt systems also helped us validate parts of the migration process before performing larger changes.

> Near the end of the project, Microsoft extended the unmanaged disk retirement timeline. The extra time helped, but most of the planning, testing, prioritization, and decision-making had already been completed.

Retirement announcements can be tracked here:

<https://azure.microsoft.com/en-us/updates?filters=%5B%22Retirements%22%5D>

## Why Cloud Hygiene Is More Than Recommendations

Cloud hygiene starts with finding issues. The harder part is deciding how to address them.

The findings matter, but the environment determines how those findings should be addressed. Business needs, technical requirements, project timelines, compliance goals, support ownership, and operational constraints all influence the plan.

Coordinating across teams is also part of the work. Many projects are connected. If one change is completed without understanding the timing or impact of another, it can affect production systems or delay work already in progress.

The technical work matters, but so does understanding how each change fits into the larger environment.

Cloud hygiene means identifying risk, understanding dependencies, prioritizing the work, and making changes in a way that the client can actually support.

## Useful Tools

Some of the tools and references I use during environment reviews include:

<https://github.com/dolevshor/azure-orphan-resources>

<https://learn.microsoft.com/en-us/azure/advisor/advisor-workbook-service-retirement>

<https://github.com/mathijsvermaat/Defender-AMA-coverage/blob/main/Defender_vs_AMA.json>
