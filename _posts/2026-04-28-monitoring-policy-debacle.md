---
title: "Monitoring Should Be Working, Right?"
date: 2026-04-28
---

Monitoring is one of those areas where teams often assume everything is fine because the deployment looks fine.

The policy is compliant. The agent extension exists. The Data Collection Rule is associated. The dashboard looks quiet, so it is easy to move on.

Then a VM starts running hot. CPU is high, disk space is low, and no alert fired. You open Log Analytics and realize the real issue: there is no data. No heartbeat. No useful signal. Just a monitoring setup that looked configured but was not actually working.

## How the Flow Is Supposed to Work

At a high level, Azure Monitor Agent depends on a few pieces working together.

The agent runs as an extension on the VM. A Data Collection Rule defines what data should be collected and where it should be sent. That rule is linked to the VM through a Data Collection Rule Association. From there, the data is sent to a destination, usually a Log Analytics workspace, where it can be queried and used for alerts, dashboards, and investigation.

That is the full path from the VM to usable monitoring data.

If any part of that path is present but not healthy, monitoring can fail quietly.

## Where Things Break Down

Azure Policy is useful for deploying and enforcing configuration at scale. Runtime health still needs separate validation.

Policy can confirm that the extension exists. It can confirm that an association exists. It can report that the resource matches the desired configuration. The remaining work is to prove that the agent installed cleanly, initialized correctly, can reach the ingestion endpoint, and is actively sending data.

If the extension is stuck, failed, blocked by network rules, missing identity permissions, or misconfigured in a way that stops data flow, the environment can still look compliant from the policy view.

That same lesson applies outside Azure Policy. An engineer can deploy the right components manually and still miss the operational proof that those components are working together.

## The Signal That Matters

The better question is "what proves monitoring is working?"

For Azure Monitor Agent, that proof usually starts with the heartbeat and expected data in Log Analytics. If the VM is not sending data, the rest of the configuration is only part of the story.

The checks should include questions like:

- Is the agent extension healthy, or only present?
- Did the extension fully install?
- Is the VM able to reach the required ingestion endpoints?
- Is the Data Collection Rule collecting what we expect?
- Is the association linked to the right resource?
- Is identity configured correctly?
- Are expected logs and metrics visible in the workspace?
- Are alerts built on data that is actually arriving?

Those checks move the conversation from deployment status to operational confidence.

## Define What Working Means

As engineers, we need to define what working means for the environment.

That should include the signals that prove monitoring is healthy, the signals that show something is broken, and the first places to check when data stops flowing. It should also be documented so the team is not rebuilding the same troubleshooting path during an outage.

The same idea applies to any platform component we deploy at scale.

Configured and working are not the same thing. Monitoring only matters when it is actually collecting the data needed to act.
