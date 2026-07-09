---
title: "FinOps Engineering Series - AKS Monitoring Costs"
date: 2026-04-28
---

Ingestion costs can climb even when the application is stable.

That is what makes monitoring cost issues easy to miss at first. Workloads may look normal. Node pools may be sized correctly. Autoscaling may be behaving. There may be no obvious spike in traffic, no major deployment, and no immediate application incident.

Then the cost anomaly shows up.

## What Changed

In this case, the environment was being standardized on Azure Monitor Agent as part of a broader move away from legacy monitoring components. Azure Policy was used to deploy the agent across client environments. At the same time, Container Insights was enabled across AKS clusters to improve observability.

From a deployment perspective, things looked good.

Policy was compliant. The agent existed. Monitoring was enabled. The platform was doing what it had been asked to do.

The issue was the amount of data being collected.

Container logs, Kubernetes events, performance metrics, inventory data, and namespace-level data were being collected broadly and frequently. In busy clusters, that adds up quickly. There was also no daily cap set on the Log Analytics workspace, so ingestion kept growing until the cost signal made it impossible to ignore.

Nothing was broken. It was working exactly as configured.

That was the problem.

## Start With What You Actually Need

Start by defining what the team actually needs to see, how quickly they need to see it, and how long they need to keep it.

Some data is useful for alerting. Some is useful during active troubleshooting. Some is useful for audits. Some is collected because it was enabled once and never reviewed again.

Those requirements need different collection, storage, and retention decisions.

## Container Insights Collection Settings

Container Insights has collection presets that control how much data is collected and how often. Common options include broader default collection, cost-optimized collection, and custom configurations.

The default settings can be helpful when a team is first establishing visibility, but they may collect more than the environment needs day to day.

A cost-optimized preset can be a good starting point because it reduces collection volume without removing monitoring entirely. Custom configuration is usually better once the team understands which namespaces, logs, and metrics matter most.

## Namespace Filtering

Each namespace should have a reason for the logs it sends.

Some namespaces generate a high volume of logs but are rarely useful during normal troubleshooting. This can include system workloads, operators, internal monitoring components, and other noisy services.

Container Insights supports namespace filtering through its log collection configuration. Instead of collecting everything across every namespace, teams can include the namespaces that matter and exclude the ones that create volume without much operational value.

That change can reduce ingestion directly while keeping the logs that engineers actually use.

## Collection Frequency

Frequency matters too.

Collecting every minute may be appropriate for some workloads, but it is not always necessary across every cluster and namespace. If the environment does not need near real-time visibility for a specific signal, increasing the interval can reduce ingestion while still leaving enough data for normal operations.

The decision should be tied to how the data is used. Alerting, incident response, capacity planning, and compliance may each need different levels of detail.

## Workspace Guardrails

Log Analytics workspaces support daily ingestion caps. A cap can help protect against unexpected spikes, misconfigured logging, or sudden increases in volume.

I treat the cap as a safeguard. If the cap is reached, ingestion stops for the rest of the day, which means logs may be missing when the team needs them.

Retention settings should also be reviewed. If data does not need to stay in Log Analytics for the default period, reducing retention can lower storage costs. If long-term retention is required, exporting logs to storage may be a better fit than keeping everything in the workspace.

## Review Diagnostic Settings

Outside AKS, diagnostic settings are often enabled across services and then left alone.

Over time, those settings can continue sending logs that no one queries, alerts on, or reviews. A periodic diagnostic settings review helps reduce unnecessary collection while still meeting operational and compliance requirements.

This is where FinOps and engineering need to work together. The work is to keep the right visibility in place and make sure it matches the environment and the business need.

## The Takeaway

AKS monitoring needs periodic review.

The collection scope, namespace filters, frequency, retention, and workspace guardrails should be reviewed like any other production design decision. Good monitoring gives teams the data they need to operate the environment. Unreviewed monitoring can collect everything by default and turn into a cost problem.

Know what you are collecting, why you are collecting it, and what value it provides.
