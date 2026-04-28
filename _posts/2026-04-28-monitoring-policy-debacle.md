---
title: "Monitoring Should Be Working… Right?"
date: 2026-04-28
---
Monitoring is one of those things that everyone assumes is working… until it isn’t.

Your team automated the full deployment of Azure Monitor agent and all it's dependancies (more on this later) with Azure Policy..

Azure policy shows green. The Data Collection Rule is associated. Everything looks correct. So naturally, you move on.
Then a few weeks go by, no alerts. Nice!!! "Our production VMs are smooth babyyy.. yea!!" err, wait, oof, this VM is maxed out on cpu..

Why is there no alerts firing.. why am I able to sleep at night, full 8 hours.. (why am I complaining about sleep) and not getting called for this issue?!

Then you start checking.. wait, I don't see anything in Log Analytics.

Nothing.
No heartbeat. No data. Just silence.


At first, it doesn’t make sense. The extension is there. Policy is compliant. The DCR is attached. Isn’t that enough?

You double check the policy. Still green.
You check the DCR association. It’s there.
You refresh Log Analytics again. Still nothing.

 ..and, now I have two more grey hairs!!

Let’s back up for a second.

At a high level, Azure Monitor Agent is made up of a few moving parts working together. The agent itself runs as an extension on the VM.
A Data Collection Rule defines what data should be collected and where it should be sent. 
That rule is then linked to the VM through an association, which tells the agent which configuration to follow. 
From there, the data is sent to a destination, most commonly a Log Analytics workspace, where it can be queried and used for monitoring and alerting.

That’s the full flow from the VM to usable data.

Where Things Start Breaking Down
It’s easy to assume that once Azure Policy (or human, err.. AI in future) deploys the AMA extension and the DCR is associated, the job is done.
But that’s not really what’s happening.

Policy deploys the Monitor extension, confirms the configuration exists, and then moves on. It’s not sitting there watching the extension install, waiting to see if it actually succeeds, or checking whether the agent is actively sending data.

It deploys… and then it goes off to watch its favorite Japanese drama. (and I can't blame it, I have a lot of recommendations for series)

If the extension gets stuck in a transitioning state, fails silently, or never properly initializes, Policy doesn’t care. As far as it’s concerned, the resource is configured the way it was told to configure it.


The False Sense of “Everything Is Fine”
This is where engineers can get tripped up.
You end up relying on signals that don’t actually represent the full picture:

Policy compliance says “configured”

The extension exists on the VM

The DCR is associated

But none of those confirm that monitoring is actually functioning.

The real signal is simple:
Is the agent sending data? Did we verify and test if the agent is consuming the data? 
If there’s no heartbeat in Log Analytics, then monitoring is not working. It doesn’t matter how green everything looks elsewhere.

The Moment You Start Asking Better Questions

This is usually where the troubleshooting shifts from “what’s deployed” to “what’s actually happening.”
Why isn’t there a heartbeat?
Is the extension actually healthy, or just present?
Did the extension fully install, or is it stuck?
Is the VM able to reach the ingestion endpoint?
Is the DCR correct for what we expect to collect?
Is identity configured properly?
Suddenly, you’re not just checking boxes anymore..

---

As engineers, we should be defining what “working” looks like in your environment.
Not just at a component level, but the full shebang.
What signals prove that monitoring is healthy?
What signals indicate something is broken?
What should you check first when data stops flowing?
And most importantly, documenting that process so you’re not figuring it out again at 2 AM six months later.

Final Thought
We can’t assume something is working just because it’s deployed or shows green.
As engineers, we need to understand the full flow and have clear proof that it’s actually doing what we intended. Not just for monitoring, but for any solution we put in place.

“It should be working” isn’t enough. We need to know that it is.
Azure gives you all the building blocks. For monitoring: Policy, DCRs, AMA, Log Analytics, etc.
But it doesn’t guarantee that those pieces are working together.
That part is on us.
Because at the end of the day, “configured” and “working” are not the same thing.
And monitoring only matters when it’s actually working.
