---
layout: page
title: Resume
permalink: /resume/
---

<section class="resume-hero">
  <p class="resume-label">Cloud security, customer engineering, and platform delivery</p>
  <h1>James Andrade</h1>
  <p class="resume-lede">Lead cloud/platform engineer focused on customer-facing cloud security, Kubernetes, Terraform, managed services, and professional services across AWS, Azure, and Google Cloud.</p>
  <div class="resume-actions" aria-label="Resume downloads">
    <a class="resume-button resume-button-primary" href="{{ '/assets/resume/James_Andrade_Cloud_Security_TAM_Resume.pdf' | relative_url }}">Download PDF</a>
    <a class="resume-button" href="{{ '/assets/resume/James_Andrade_Cloud_Security_TAM_Resume.docx' | relative_url }}">Word</a>
    <a class="resume-button" href="{{ '/assets/resume/James_Andrade_Cloud_Security_TAM_Resume.md' | relative_url }}">Markdown</a>
  </div>
</section>

<section class="resume-stats" aria-label="Experience summary">
  <div class="resume-stat">
    <strong>25+</strong>
    <span>managed cloud client accounts</span>
  </div>
  <div class="resume-stat">
    <strong>Dozens</strong>
    <span>client projects across AWS, Azure, GCP, and Kubernetes</span>
  </div>
  <div class="resume-stat">
    <strong>40h to 2h</strong>
    <span>engineering effort reduced for client environment findings</span>
  </div>
  <div class="resume-stat">
    <strong>Client value</strong>
    <span>automation, advisory work, lifecycle management, and platform cleanup</span>
  </div>
</section>

<section class="resume-section">
  <h2>Profile</h2>
  <p>I design, deploy, secure, migrate, and troubleshoot cloud environments for clients that need their infrastructure to be understandable, supportable, and ready for production work. My day-to-day work spans managed services and professional services, from discovery meetings and architecture review through Terraform implementation, environment deployment, Kubernetes operations, security controls, monitoring, production incidents, validation, ongoing improvement, support, documentation, and handoff.</p>
  <p>I try to start with the environment before I start with a recommendation. That means understanding where the data is stored, how the application works, how users and engineers access it, what is already monitored, what is backed up, where security tools sit, what costs are trending, and what the client actually wants to solve first.</p>
</section>

<section class="resume-section">
  <h2>How I Work With Clients</h2>
  <div class="resume-service-grid">
    <article class="resume-card">
      <h3>Environment Review</h3>
      <p>Map application flow, data stores, access paths, network entry points, dependencies, backup coverage, logging, security controls, cost drivers, and operational ownership.</p>
    </article>
    <article class="resume-card">
      <h3>Priority Planning</h3>
      <p>Work with the client to separate immediate risk, cleanup work, cost actions, required compliance items, and longer-term design changes.</p>
    </article>
    <article class="resume-card">
      <h3>Platform Onboarding</h3>
      <p>Set up automation platforms and managed services by enabling backups, establishing policies, linking Git repositories, standardizing Terraform folders and files, and validating deployments.</p>
    </article>
    <article class="resume-card">
      <h3>Operational Validation</h3>
      <p>Confirm that monitoring, security, and FinOps agents are installed correctly, data is flowing, scan findings are visible, and Zero Trust controls are placed where they need to be.</p>
    </article>
  </div>
</section>

<section class="resume-section">
  <h2>Review and Onboarding Flow</h2>
  <div class="resume-diagram" aria-label="Client environment review and onboarding flow">
    <div class="diagram-step">Discovery</div>
    <div class="diagram-step">Scan Findings</div>
    <div class="diagram-step">Priority Plan</div>
    <div class="diagram-step">Git and Terraform Setup</div>
    <div class="diagram-step">Deploy and Install Agents</div>
    <div class="diagram-step">Validate and Handoff</div>
  </div>
</section>

<section class="resume-section">
  <h2>Selected Work</h2>
  <div class="resume-project-list">
    <article class="resume-project">
      <h3>Client Platform Onboarding and Automation Enablement</h3>
      <p>Onboarded client environments into automation and managed-service platforms, including backup coverage, policy baselines, Git repository linkage, Terraform code standards, environment deployments, and post-deployment validation.</p>
      <ul>
        <li>Automated monitoring, security tooling, and FinOps agent installation across client environments.</li>
        <li>Validated agent health, scan findings, platform data flow, backup and policy coverage, and Zero Trust control placement before operational handoff.</li>
      </ul>
      <p class="resume-tags">Terraform, Git, backups, policy, monitoring agents, security agents, FinOps agents, Zero Trust</p>
    </article>

    <article class="resume-project">
      <h3>Client Environment Review Automation and Advisory Program</h3>
      <p>Led an internal initiative to standardize recurring client environment reviews across security posture, reliability, backup coverage, access paths, logging, service lifecycle, orphaned resources, unattached disks, platform hygiene, and FinOps.</p>
      <ul>
        <li>Built automation that collected and organized findings so engineers could produce a client-ready review package in roughly 2 hours instead of about 40 hours of manual effort.</li>
        <li>Used the reports with clients to align remediation priorities with their goals and decide which items needed attention first.</li>
      </ul>
      <p class="resume-tags">Automation, advisory delivery, security posture, FinOps, reporting, environment scans</p>
    </article>

    <article class="resume-project">
      <h3>Azure Defender for Storage Malware Quarantine Automation</h3>
      <p>Built an event-driven response workflow using Microsoft Defender for Storage, Event Grid, Azure Functions, C# isolated worker, .NET, Blob Storage, Azure Monitor, and Application Insights.</p>
      <ul>
        <li>Processed Defender scan results for uploaded objects and ignored clean verdicts.</li>
        <li>Copied malicious blobs to a quarantine container while preserving the original object for investigation and audit requirements.</li>
      </ul>
      <p class="resume-tags">Azure Defender for Storage, Event Grid, Azure Functions, Blob Storage, C#</p>
    </article>

    <article class="resume-project">
      <h3>Kubernetes (AKS) Lifecycle and Upgrade Process</h3>
      <p>Led staged Kubernetes lifecycle and upgrade work across sandbox, development, stage, production, and shared environments, coordinating control plane, managed add-ons, node pools, images, and template changes through approved maintenance windows.</p>
      <ul>
        <li>Moved application platforms through sequential Kubernetes versions with rolling node replacement, health checks, load balancer validation, workload verification, and rollback planning.</li>
        <li>Folded security remediation into lifecycle work by updating worker images and Kubernetes add-ons while reducing production risk through environment sequencing.</li>
      </ul>
      <p class="resume-tags">Kubernetes, AKS, node pools, images, add-ons, change windows</p>
    </article>

    <article class="resume-project">
      <h3>Kubernetes Ingress and Security Policy Controls</h3>
      <p>Supported Kubernetes ingress designs around routing, authentication, WAF placement, TLS, network reachability, and application validation without tying the design to one controller pattern.</p>
      <ul>
        <li>Built the AKS equivalent for ingress and security policy work, including private access, RBAC, Azure Policy, network controls, workload identity, monitoring signal flow, and policy validation.</li>
      </ul>
      <p class="resume-tags">Kubernetes, AKS, ingress, Azure Policy, RBAC, WAF, workload identity</p>
    </article>

    <article class="resume-project">
      <h3>Kubernetes Platform Troubleshooting and Add-On Operations</h3>
      <p>Troubleshot managed Kubernetes platform issues across add-ons, worker image updates, autoscaling behavior, ingress behavior, node pressure, pod placement, resource requests, and monitoring signal gaps.</p>
      <ul>
        <li>Used Datadog, Kubernetes events, node utilization, workload density, provisioning behavior, and add-on state to separate platform problems from application resource pressure.</li>
      </ul>
      <p class="resume-tags">Kubernetes, add-ons, autoscaling, node pressure, Datadog, workload troubleshooting</p>
    </article>

    <article class="resume-project">
      <h3>AWS Application Platform and Private Networking</h3>
      <p>Designed and supported AWS application platforms using Route 53, CloudFront, ALB/NLB, ECS, managed Kubernetes workloads, EC2 Auto Scaling, private and public subnets, Security Groups, IAM, ECR, KMS, EBS, S3, CloudWatch, TLS certificates, and Terraform.</p>
      <ul>
        <li>Planned platform changes around DNS ownership, private networking, VPN and firewall paths, WAF placement, service segmentation, high availability, and production cutover ownership.</li>
      </ul>
      <p class="resume-tags">AWS, Terraform, private networking, DNS, WAF, ALB, NLB, ECS, Kubernetes</p>
    </article>

    <article class="resume-project">
      <h3>Azure and Google Cloud Platform Security</h3>
      <p>Delivered Azure and GCP platform work covering monitoring agent migration, Azure Policy, CIS-aligned controls, private endpoint usage, Azure Lighthouse operations, secure Azure AI and AVD design, and GKE support.</p>
      <ul>
        <li>Coordinated Data Collection Rules, workspace mappings, phased rollout plans, and validation for monitoring and security signal continuity.</li>
        <li>Supported GKE environments using private clusters, regional node pools, Shared VPC, Cloud NAT, Cloud Armor, Cloud DNS, Cloud Logging, Cloud Monitoring, IAM, Workload Identity, and Terraform.</li>
      </ul>
      <p class="resume-tags">Azure Monitor, Azure Policy, Azure Lighthouse, GKE, Shared VPC, Cloud Armor, Terraform</p>
    </article>
  </div>
</section>

<section class="resume-section">
  <h2>Experience</h2>
  <div class="resume-timeline">
    <article>
      <h3>RapidScale</h3>
      <p><strong>Lead Cloud Engineer</strong> | 2024 - Present</p>
      <p><strong>Senior Cloud Engineer, Cloud SME</strong> | 2023 - 2024</p>
      <p><strong>Team Lead, Senior Cloud Support</strong> | 2022 - 2023</p>
      <p><strong>Cloud Support Associate</strong> | Early 2022</p>
    </article>
    <article>
      <h3>Rackspace</h3>
      <p><strong>Linux Cloud Systems Administrator</strong> | 2017 - 2021</p>
    </article>
  </div>
</section>

<section class="resume-section">
  <h2>Certifications</h2>
  <ul class="resume-cert-list">
    <li>AWS Certified: Security - Specialty</li>
    <li>AWS Certified: DevOps Engineer - Professional</li>
    <li>Google Cloud Professional Cloud Architect</li>
    <li>Google Cloud Professional Cloud Security Engineer</li>
    <li>Microsoft Certified: Azure Solutions Architect Expert</li>
    <li>Microsoft Certified: Azure Network Engineer Associate</li>
    <li>Microsoft Certified: Azure AI Engineer Associate</li>
    <li>Microsoft Certified: Azure Virtual Desktop Specialty</li>
    <li>Tanium Deployment Specialist</li>
    <li>Gremlin Certified Chaos Engineering Professional</li>
  </ul>
</section>

<section class="resume-section">
  <h2>Related Writing</h2>
  <ul>
    <li><a href="{{ '/2026/07/09/client-environment-review-infrastructure-design.html' | relative_url }}">What I Look For Before Designing or Taking Over an Environment</a></li>
    <li><a href="{{ '/2026/07/09/ai-workload-governance-checklists-auditing-scanning.html' | relative_url }}">Why AI Workloads Need Checklists, Governance, Auditing, and Scanning</a></li>
    <li><a href="{{ '/docs/' | relative_url }}">Docs and topic index</a></li>
  </ul>
</section>
