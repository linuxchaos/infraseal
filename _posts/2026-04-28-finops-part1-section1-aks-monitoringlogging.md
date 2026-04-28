---
title: "FinOps Engineering Series - Hidden Cost Levers in AKS and Azure Monitoring"
date: 2026-04-28
---
Most cost optimization discussions around AKS focus on compute. Node sizing, autoscaling, Spot instances, and reservations are all valid areas to review.

Alongside that, there are other areas that can contribute to cost over time, especially around monitoring and logging.

Services like Container Insights, Log Analytics, and Diagnostic Settings are often enabled early and left as-is. Over time, they can collect more data than is actually needed.

This post walks through a few configurations engineers can review and adjust based on their environment, goals, and use case.

1. Container Insights v2 - Basic Logs tier

Container Insights sends AKS logs to Log Analytics, with container logs stored in the ContainerLogV2 table. This table supports the Basic Logs tier.

Basic Logs reduces ingestion cost compared to the Analytics tier, which can make a noticeable difference in environments with a lot of container log volume.

This tends to make the most sense when logs are mainly used for troubleshooting or occasional investigation, not constant querying.

There is a tradeoff. Basic Logs charges for queries based on the amount of data scanned. So ingestion cost is lower, but query cost becomes usage-based.

For environments where logs are not queried frequently, this can still result in lower overall cost.

2. Container Insights presets

Container Insights includes presets that control what data is collected and how often it is collected.

Common options include Logs and Events (default), Standard, Cost-optimized, and Custom.

The cost-optimized preset increases the collection interval and excludes certain system namespaces. It doesn’t remove monitoring, it just adjusts how much data is collected by default.

This can be a good starting point depending on how active the cluster is and what visibility is actually needed.

3. Namespace filtering

Not every namespace needs to be logged.

In most clusters, there are namespaces that generate a large amount of logs but are rarely used during troubleshooting. This often includes monitoring tools, operators, and system workloads.

Container Insights supports namespace filtering through its log collection configuration (the DCR/config-based setup in Azure documentation).

Instead of collecting logs across everything, you can narrow it down. That might mean including only specific namespaces, or excluding the ones that are known to be high-volume and not useful.

This directly reduces the amount of data being sent to Log Analytics.

4. Collection interval

Container Insights collects data every 1 minute by default, but this can be adjusted up to 30 minutes.

That default is not always needed.

If 1-minute granularity isn’t required, increasing the interval reduces ingestion volume while still providing enough visibility for most scenarios.

This is a simple setting that can be adjusted without impacting most operational workflows.

5. Log Analytics daily cap

Log Analytics supports a daily ingestion cap. Once the cap is reached, ingestion stops for the rest of the day.

This helps protect against unexpected spikes, misconfigured logging, or sudden increases in data volume.

It should be treated as a safeguard. It’s often overlooked, but it can prevent unexpected cost spikes when something changes in the environment.

Just keep in mind that once the cap is hit, logs won’t be collected again until the next day.

6. Diagnostic Settings review

Azure resources use Diagnostic Settings to send logs to destinations such as Log Analytics, Storage, or Event Hub.

A common pattern is enabling all available log categories during setup. Over time, only some of those logs are actually used, while the rest continue to be ingested.

Reviewing Diagnostic Settings across services can help identify where data is being collected but not used. This isn’t just for AKS, but across other Azure services as well.

Adjust or disable what isn’t needed, while still making sure compliance and security requirements are met.

7. Long-term log storage

For long-term retention, Azure Storage can be more cost-effective than keeping logs in Log Analytics.

Storage tiers include Hot, Cool, Cold, and Archive. Lifecycle rules can move data between tiers over time.

Each tier has minimum retention periods. Cool is around 30 days, Cold around 90 days, and Archive around 180 days.

If data is moved or deleted earlier than those timeframes, charges still apply based on the minimum duration.

Tiering works best when retention requirements actually align with those durations. I will have a separate blog post just on storage tiers.

8. AKS configuration decisions

These settings are often defined during deployment and not revisited.

Cluster management tiers

AKS offers Free, Standard, and Premium.

The difference is control plane SLA.

If SLA is not required, the Free tier can reduce cost. Note: I have seen production clusters within client environments that are on the Free tier. I’ve never seen them go down.

If the risks are evaluated, and the workloads running on the clusters are capable of withstanding downtime for whatever reason, the Free tier without the SLA may be a worthy tradeoff.

Azure Advisor will scream at you, but the real yelling that matters is from the customers and users.

Stop/start clusters

AKS clusters can be stopped and started.

When stopped, nodes are deallocated, compute cost is reduced, and the cluster state is preserved.

This can be useful for clusters that do not need to run continuously.

One consideration is that in capacity-constrained regions, restarting may be delayed if capacity is not available.

This should be evaluated before using it in critical environments.

Additional: Log Analytics data retention

Log Analytics workspaces allow data retention to be configured, and this is often left at the default.

Default retention is usually around 30 days, but it can be adjusted based on actual needs.

If data does not need to be retained that long, reducing retention lowers storage cost.

If long-term retention is required, exporting logs to Storage may be more cost-effective than keeping everything in Log Analytics.

This is one of those settings that’s easy to overlook.

Real-world example: Container Insights via Azure Policy

One situation that stood out was when Container Insights was enabled across AKS clusters through an Azure Policy deployed by an internal platform team.

There wasn’t much context provided ahead of time around what that change would mean from a data or cost perspective.

After it was applied, a handful of clients started seeing their Log Analytics ingestion increase, and in some cases it was a pretty noticeable spike.

Looking into it, the behavior made sense. Container Insights was now collecting container logs, Kubernetes events, performance data, and inventory across active clusters. In busy clusters, that volume adds up quickly when everything is collected by default.

There also wasn’t a Log Analytics daily cap set at the time, so ingestion just kept going.

Our cost anomaly detection picked it up, and both internal teams and the client were alerted.

From there, we worked through stabilizing things. Container Insights configuration was adjusted, namespace filtering was implemented, and collection settings were tuned to better match how the clusters were actually being used. A daily cap was also added to the workspace as a safeguard going forward.

A credit request was submitted due to the unexpected cost increase, and it was approved.

After tuning, the required visibility was still there, but ingestion volume was reduced. It also led to better internal processes around communicating changes like this, especially when deploying through policy or automation across multiple environments.

In closing,

Cost optimization in AKS environments includes more than compute.

Monitoring and logging configurations should also be reviewed periodically, including log tiers, collection intervals, namespace filtering, diagnostic settings, and retention policies.

These settings can be adjusted based on the environment and how it’s actually being used, helping reduce unnecessary data collection while maintaining the visibility needed to operate effectively.
