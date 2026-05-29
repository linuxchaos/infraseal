---
title: "Cloud Hygiene for client environments"
date: 2026-05-29
---

It's important to review and improve the overall health and hygiene of the cloud environment. With Azure, services retire and deprecate throughout their lifecycle.

I would like to share a real world example of this which led to an internal initiaive.

We support a range of client environments, some are built from the ground
up (greenfield), and many others already have an existing environment in which we take over and provide support. We are starting on the back foot with many. 

I led a larger effort to provide a sustainable way for our engineering teams to assess and improve our clients' environments.

A large portion of the process was created, by working with a client of ours organically. 

Startd during discussions of doing a review of a clients Azure environment. I provided my findings when I engaged the client and shared my report on key items that needed attention. The client was happy to get going and
improve their environment.

There were multiple services and resources within their Azure environment that were
retiring/deprecating within a years time. I led the overall project dedicated to getting their
environment in a healthy state. This would require coordination with multiple teams with multiple
projects tied together..

This involved multiple meetings and discussion that spanned throughout the year.

A few examples:
VMs use unmanaged disks.
IPs are in a sku that will be deprecated.
Load balancers are in a sku that will be retired.
The client would like to opt for a different load balancing solution.
Their storage accounts (and applications interacting with the storage account) are using an
outdated version which Azure will be removing.
Their VMs are on an EOL version, and some are getting there. Their domain controllers will be
on an unsupported version very soon, needing Active Directory/Domain controller migration..

Among other things..

This involved careful planning, research, and testing, on how to get their services on supported
versions, or how to move/migrate them off. And the timeline for addressing each item. They also
had an audit coming up for compliance that they were trying to pass. As well as upgrading their
firewall appliance.

For example, upgrading their VMs to a new supported OS version, involved a lot of things.
Firstly, their VMs are in availability sets, and if you want to migrate the VMs to get onto managed
disk, you would need to upgrade the availability set as well. 
https://learn.microsoft.com/en-us/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks

Some VMs were able to do this, while others had to wait. 

There were two pairs of VMs that were being used as file servers, however
they were tied to the DCs/AD. So, we needed to document what the url /path will be when
authenticated and mounted, and if it needed to be updated. 
https://learn.microsoft.com/en-us/windows-server/storage/dfs-namespaces/dfs-overview?tabs=server-manager

Speaking of DCs, had to research the differences of the forest structure between 2012r and 2022.

Looking at the following docs, it appears that 2022 is compatible with 2012r forest:

https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/active-directory-functional-levels#windows-server-2012-r2-functional-levels
https://learn.microsoft.com/en-us/answers/questions/1609978/upgrading-moving-the-ad-server-from-2012-r2-datace
https://learn.microsoft.com/en-us/answers/questions/1858256/can-i-have-a-mix-of-server-2008-2012-and-server-20

There are some considerations, for example, making sure to migrate SYSVOL from FRS to DFSR.

https://serverfault.com/questions/1147588/replacing-domain-controller-with-server-2022
https://learn.microsoft.com/en-us/windows-server/storage/dfs-replication/migrate-sysvol-to-dfsr

(e.g.making sure FSMO roles are moved to new DCs.)

The size of the VMs disks can also play a factor in how long it will take to upgrade to managed disks. 

A lot of technical nuance like this occurred during the upgrades and migration for their resources.

We made a game plan after as much research and testing, to group VMs together and do a
“phased” approach. This meant something like:
DC VMs first, then test the file servers, then prod/application servers, etc.
The VMs needed to be redeployed as a new VM, in place upgrades usually break within Azure.
So for the ones that needed OS upgrade as well, that would alter some of the instructions and
processes.

The client and I needed to make tradeoff decisions.\

For example, the unmanaged disk retirement was going to take effect before the unsupported
OS version work can be followed through completely. So, would we rebuild the VMs that way it
gets on a supported OS version, and also have managed disk by default? Or, do we worry
about getting the VMs on managed disks, then later worry about the unsupported version?

It came down to real world situations. The clients development team needed to have enough
time to get their workloads running on the new VMs, if we went the rebuild route. That will take
longer than just upgrading the disks from unmanaged to managed. But, it will kill two birds with
one stone. (I don’t like this phrase, I like birds).

We made the decision to rebuild the VMs we know the developers were able to get their
workloads on quickly before the unmanaged disks retirement date. The others, we opted to do
the unmanaged disk upgrade first. The few VMs we were able to do rebuild, we kept the old
VMs to test the unmanaged migration steps.

Side note: At the last possible second, Microsoft Azure pulled back the retirement date to a later
time.. This would have been nice to know ahead of time so we didn’t spend needless time
making the tradeoff decisions, but Microsoft overlords have the final say..

A doc on retirement announcements:
https://azure.microsoft.com/en-us/updates?filters=%5B%22Retirements%22%5D


I give this example, because throughout each phase and different upgrades/migration projects,
nuanced discussion and understanding the clients needs throughout (both business needs but
also technical requirements) adjusted the way we approached the work done, the teams
needing to be involved, the criteria for success, and completed the projects.

Coordinating between the various teams is crucial because of the step by step phases and working relationship
with the client on their needs and environment holistic view. All the projects were integrated in
some way. If a project was completed before a scheduled maintenance, or changes were made
without knowing about the other work being performed, it could take down the live production
systems.

These are some of the tools I use:

https://github.com/dolevshor/azure-orphan-resources
https://learn.microsoft.com/en-us/azure/advisor/advisor-workbook-service-retirement
https://github.com/mathijsvermaat/Defender-AMA-coverage/blob/main/Defender_vs_AMA.json
