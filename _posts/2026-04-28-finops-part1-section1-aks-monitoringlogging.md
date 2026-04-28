---
title: "Go Big or Go Broke? FinOps Engineering Series - AKS Monitoring"
date: 2026-04-28
---
Climbing higher… and higher… and higher.
How far?
No, not that tower climb you saw on Netflix. That was impressive.
This is our clients’ data ingestion.
And it’s not slowing down.

At first, it doesn’t make sense.
Nothing major changed in the clusters. Workloads are steady. No big deployments. No sudden spikes in traffic.
So we check compute.
Node pools look fine. Autoscaling is behaving. No obvious waste.
Nothing stands out.

So now the question becomes simple.
Why is this still going up?

This actually ties back to another change that had been happening.
As part of moving away from the legacy monitoring agent and standardizing on Azure Monitor Agent (covered in my other post: [you’ll add title here]), Azure Policy was used to deploy AMA across client environments.
That part made sense.
Then, alongside that, Container Insights was enabled across AKS clusters at scale through an internal platform push.
No real context around what that would mean from a data perspective.
Just enabled.

At a glance, everything looks good.
Policy is compliant. The agent is there. Monitoring is “enabled.”
So we move on.

Until we actually look at what’s being collected.

Container logs.
Kubernetes events.
Performance metrics.
Inventory data.
Across all namespaces.
Every minute.
Everything.

And that’s when it clicks.
Nothing is broken.
It’s working exactly as configured.
And that’s the problem.

In busy clusters, that volume adds up quickly when everything is collected by default. 
There was also no daily cap set on the workspace.
So ingestion just kept going.

This is a real world example, and was caught by cost anlomy alerts.. credit requests make the organization treat this as a priority.. $ talks..
So, what can we will look at to prevent more gray hairs form popping up..?

We can start with Container Insights.
By default, container logs are sent to Log Analytics using the Analytics tier. In containerlogsV2, there's an option to set the tier to Basic logs. If logs are mainly used for troubleshooting and not constant querying, this can reduce ingestion cost, with the tradeoff that queries become usage-based.

Container Insights has a few different collection presets that control how much data is collected and how often. Common options include Logs and Events, Standard, Cost-optimized, and Custom. The default can provide broad visibility, but it may also collect more than what is actually needed for the environment.

The Cost-optimized preset can be a good starting point because it increases the collection interval and excludes certain system namespaces. It does not remove monitoring completely. It just reduces how much data is collected by default. 

Next is namespaces.
Not every namespace needs to be logged.

In most clusters, there are namespaces that generate a large amount of logs but are rarely used during troubleshooting. This can include monitoring tools, operators, and system workloads.

Container Insights supports namespace filtering through its log collection configuration. Instead of collecting logs across everything, we can narrow it down by including only specific namespaces or excluding high-volume namespaces that do not provide much value day to day.

That directly reduces how much data gets sent to Log Analytics.

Then there’s collection frequency.
Container Insights collects data every 1 minute by default, but that interval can be increased up to 30 minutes.
That default is not always needed.

If the environment does not need near real-time visibility, increasing the interval can reduce ingestion while still providing enough operational data for most scenarios


Outside of Container Insights, we also look at the workspace itself.
Log Analytics supports a daily ingestion cap. Once the cap is reached, ingestion stops for the rest of the day. This can help protect against unexpected spikes, misconfigured logging, or sudden increases in data volume.

It should be treated as a safeguard, not the main cost optimization strategy. If the cap is hit, logs will not be collected again until the next day.


Log Analytics workspaces have configurable retention, and this is often left at the default. If data does not need to be retained that long, reducing retention can lower storage cost.

If long-term retention is required, exporting logs to Azure Storage may be more cost-effective than keeping everything in Log Analytics.

Then we step back and look across the environment.
Diagnostic Settings are often enabled across services early on. Over time, not all of those logs are actually used. Reviewing and trimming those down helps reduce unnecessary data collection while still meeting requirements. 

Over time, ingestion comes down.
Visibility is still there.
But now it actually reflects how the environment is being used.

That’s really the takeaway.
AKS monitoring isn’t just something we enable and forget.
It’s something we need to understand.


Because if we go all in by default, it will work.
But it will also cost you.

Go big or go broke?
