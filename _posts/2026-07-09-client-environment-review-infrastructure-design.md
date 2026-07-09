---
title: "What I Look For Before Designing or Taking Over an Environment"
date: 2026-07-09
---

When I walk into a client environment, I start with context before tools or migration plans.

I start by understanding what the environment is supposed to do, who depends on it, how people access it, where the data lives, and what would actually hurt the business if something failed.

That sounds simple, but it is the part that determines whether the rest of the work is useful. A cloud environment can look organized in the portal and still have gaps in backup, monitoring, access control, patching, cost, or disaster recovery. A design can look clean on paper and still miss the way users actually reach the application.

Inventory matters, but it needs to connect back to how the environment actually runs. I want enough context to make recommendations that fit the client, the workload, and the operational reality.

This is the type of thinking I use when I am entering a client environment, reviewing an existing setup, or helping design new infrastructure.

## Understand the Application First

Before touching infrastructure design, I want to understand the application flow.

What does the application do? Who uses it? Is it internal, public-facing, partner-facing, or used by a small group of administrators? What happens when a user signs in? What systems does the application talk to? What background jobs, APIs, queues, databases, and integrations are involved?

I want to know the basic request path:

- How does a user reach the application?
- Is there a public endpoint, private endpoint, VPN, ExpressRoute, Direct Connect, or some other access path?
- Is there a load balancer, application gateway, ingress controller, CDN, WAF, or reverse proxy in front?
- Why was that component chosen?
- Is traffic terminated there, passed through, inspected, or routed based on path or hostname?
- Where does authentication happen?
- What happens if one backend instance fails?

That context matters because infrastructure should support the way the application actually works. If the application is stateful, that affects scaling and failover. If sessions are stored locally, adding more instances may not be enough. If the application depends on a single database, the web tier can be highly available while the real risk still sits somewhere else.

## Know Where the Data Lives

Data is one of the first areas I care about.

Where is it stored? Which databases are in use? Are there storage accounts, file shares, managed disks, object storage buckets, data warehouses, search indexes, caches, or message queues? Is data being replicated? Is anything still stored on a VM that people assume is stateless?

I also want to understand ownership and sensitivity:

- What data is business-critical?
- What data is regulated, confidential, or customer-facing?
- Who owns the data?
- Who can access it?
- How is it backed up?
- How long does it need to be retained?
- Is encryption enabled and managed correctly?
- Are keys customer-managed, platform-managed, or handled somewhere else?

It is hard to make good decisions around security, DR, monitoring, or cost without understanding the data layer.

## Review Identity and Access

Access tells you a lot about the maturity of an environment.

I look at how engineers and administrators access the cloud account or tenant. In Azure, that means Entra ID, subscriptions, management groups, RBAC assignments, privileged roles, break-glass accounts, conditional access, and service principals or managed identities. In AWS, that means IAM users, roles, policies, organizations, permission sets, root account protections, and access patterns into individual accounts.

Then I look deeper.

How do people access VMs? Are they using Bastion, VPN, just-in-time access, public IPs, SSH keys, local admin accounts, domain accounts, or shared credentials? How do they access Kubernetes? Is cluster access tied to identity, certificates, kubeconfig files, local admin credentials, or a CI/CD pipeline?

For users, I want to know how login works end to end. Are users authenticating through SSO, local accounts, LDAP, SAML, OIDC, or another identity provider? Are there separate admin paths? Are service accounts documented? Are secrets stored in a vault, pipeline variable, Kubernetes secret, config file, or somewhere no one wants to admit?

The questions are simple:

- Who has access?
- How did they get it?
- Do they still need it?
- Can we trace what they did?
- Can we remove access without breaking the environment?

## Understand Network and User Paths

Network diagrams are useful, but I still want to validate the real paths.

How does traffic enter the environment? Is it public internet, private WAN, VPN, peering, transit gateway, hub-and-spoke, firewall, load balancer, ingress, or a mix? Are there multiple clouds involved? Is there on-prem connectivity? Are DNS zones split between internal and external views?

For load balancers, I want to know why they exist and what problem they solve. Are they providing high availability, TLS termination, path-based routing, private access, outbound control, WAF inspection, or simply sitting there because the original design needed one?

I also look for hidden dependencies:

- DNS records owned by the client
- Certificates owned by another team
- Firewall rules managed outside the cloud team
- Third-party allowlists
- On-prem systems required for authentication or data flow
- Legacy IP dependencies
- Hardcoded hostnames or endpoints

These items matter during migrations, DR planning, and infrastructure redesign. Terraform can deploy a second region, but it cannot automatically make the client approve DNS changes, update third-party allowlists, validate application behavior, or coordinate a business cutover.

## Decide Whether the Future Is Cloud, On-Prem, or Hybrid

Every client has a different path for cloud, on-prem, and hybrid work.

Some environments are fully cloud-native. Some still have on-prem systems that are not going away soon. Some are hybrid because identity, file services, main applications, or compliance requirements still depend on existing infrastructure. Some use more than one cloud because of acquisitions, team preferences, vendor requirements, or existing contracts.

Hybrid is a design constraint that needs to be understood clearly.

The important questions are:

- What must stay on-prem?
- What can move?
- What should not move yet?
- What data flows between environments?
- What is the latency requirement?
- Who owns each side of the connection?
- What is the long-term direction?

The answer shapes network design, identity, backup, monitoring, security controls, cost, and operational ownership.

## Review Security Posture

Security posture shows up in several parts of the environment at once.

I look at identity, network exposure, logging, vulnerability management, endpoint protection, firewall placement, WAF rules, private access, key management, secrets, patching, and how exceptions are handled.

I also want to understand which security tools are already in place and where they sit in the architecture. Are they deployed at the endpoint, cloud control plane, container layer, network layer, CI/CD pipeline, or application layer? Are they alerting? Is anyone reviewing the alerts? Are findings routed to the right team? Are there duplicate tools creating noise?

Baseline standards matter too. If the client needs CIS benchmarks, regulatory controls, internal hardening requirements, or industry-specific compliance, those requirements should be mapped into the environment review early. Waiting until the end usually creates rework.

Security questions I care about include:

- Are public endpoints intentional?
- Are management ports exposed?
- Are admin accounts protected with MFA and conditional access?
- Are least-privilege roles actually in place?
- Are secrets stored properly?
- Are disks, databases, and storage encrypted?
- Are workloads patched?
- Are unsupported operating systems still running?
- Are container images and dependencies scanned?
- Are logs available for investigation?
- Is there evidence for compliance, or only a verbal process?

## Look at Operations and Lifecycle

An environment is only as strong as the way it is operated.

I look at OS versions, patching cadence, maintenance windows, application release process, infrastructure-as-code usage, ticket flow, change management, incident response, and ownership.

I also look for operational overhead. Are engineers manually repeating tasks that should be automated? Are there fragile scripts only one person understands? Are deployments tied to a specific workstation? Are there old VMs nobody wants to touch? Are there resources with unclear owners?

Client churn matters here. If the client's internal team is changing often, documentation and repeatable process become even more important. If a managed services team is taking over, the environment needs enough clarity for someone new to support it without guessing.

## Validate Backups, Retention, and Recovery

Backups need evidence behind them.

I want to know whether backups exist, what they cover, how often they run, where they are stored, how long they are retained, who monitors failures, and when the last restore was tested.

For each critical workload, I want the client to understand the difference between backup and disaster recovery. A backup may restore data. DR should answer how the business continues operating when a region, site, service, or dependency fails.

The right DR design depends on appetite and budget.

Some clients need a warm second region with automated deployment and tested failover. Some need infrastructure-as-code that can deploy a second region when needed, with DNS and application cutover handled in a defined phase. Some only need backups and a documented restore process because the workload does not justify active DR.

The important part is making the decision intentionally.

Questions I ask:

- What is the recovery time objective?
- What is the recovery point objective?
- What systems must come back first?
- What can wait?
- Who owns DNS changes?
- Who owns application validation?
- Is failover tested?
- Is failback planned?
- Are backup policies aligned with business requirements?

## Review Monitoring and Logs

Monitoring should match what the client cares about.

I want to know what logs are collected, where they go, how long they are retained, what alerts exist, and whether the alerts are useful. More logs do not automatically mean better operations. Too few logs create blind spots. Too many logs create cost and noise.

The key questions are:

- What do we need to know during an incident?
- What does the security team need for investigation?
- What does compliance require?
- What does the application team actually query?
- Which logs are high volume but low value?
- Which signals should alert someone immediately?
- Which signals should go to a dashboard or report instead?

This is also where cost and monitoring meet. Log volume, retention, diagnostic settings, container logging, and workspace design all affect the bill. The client should understand why logs are being collected and what value they provide.

## Check Cost, Capacity, and Long-Term Commitments

Cost review starts with usage, commitments, waste, and the business direction.

I look at reservations, savings plans, committed use discounts, right-sizing, unattached disks, idle public IPs, old snapshots, unused load balancers, orphaned resources, backup storage growth, log ingestion, data transfer, and managed service tiers.

I also care about long-term items the client cares about:

- Are they trying to reduce monthly spend?
- Are they willing to commit to reservations?
- Are they expecting growth?
- Are there seasonal spikes?
- Are they paying for high availability they are not using?
- Are they avoiding HA to save cost without understanding the risk?

Cost decisions should be tied to business expectations and the way the environment is expected to grow.

## Run Scans and Look for Red Flags

Manual review is important, but scans help catch what people miss.

I like to run checks for unattached disks, stale snapshots, unused NICs, public exposure, unsupported SKUs, missing tags, policy drift, unencrypted resources, old operating systems, missing backups, and obvious security misconfigurations.

For compliance-oriented environments, I also look at CIS benchmarks and any required control framework. The useful part is separating critical risk, near-term cleanup, long-term improvement, and items that need a business decision.

Scanning should support the conversation and give engineering judgment better evidence.

## Build the Plan in Phases

After the review, the client needs more than a long list of findings.

I want phases.

A good plan usually separates immediate risk, quick hygiene wins, required compliance work, platform improvements, modernization, and long-term design changes. It should also identify what the client owns, what the engineering team owns, and what depends on another group.

For example, an infrastructure team may be able to deploy a second region with Terraform. The client may still own DNS changes, user testing, certificate approvals, vendor allowlists, and production cutover timing. Those responsibilities need to be visible before the plan becomes a project.

The final output should help everyone understand:

- What is wrong
- Why it matters
- What should happen first
- Who owns each action
- What risk remains if the work is deferred
- What the target state looks like

## Final Thought

This is the general shape of how I think through environments.

Start with the application and data. Understand access, network paths, security, operations, backup, DR, monitoring, cost, and ownership. Validate assumptions with evidence. Use scans to find red flags. Build the plan in phases the client can actually execute.

Good infrastructure work starts with knowing which questions to ask before deploying anything.
