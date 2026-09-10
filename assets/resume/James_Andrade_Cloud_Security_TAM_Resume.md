# James Andrade

**Lead Cloud / Platform Engineer | Customer Engineering | Cloud Security | AWS | Azure | GCP | Kubernetes | Terraform**

## Professional Summary

Lead cloud/platform engineer with customer-facing consulting experience designing, deploying, securing, migrating, and troubleshooting enterprise cloud environments across AWS, Azure, and Google Cloud.

Owns technical delivery and customer onboarding for managed-service clients, including discovery, architecture review, automation platform installation, implementation planning, production validation, documentation, and operational handoff.

Works across managed services and professional services, seeing the client timeline from discovery meetings and environment deployment through ongoing improvements, support, and lifecycle work.

Leads client environment reviews that turn scan data, operational findings, security posture, cost signals, and customer goals into prioritized remediation plans and longer-term platform work.

Hands-on background in Kubernetes, Terraform, private networking, IAM/RBAC, WAF architecture, KMS encryption, observability, vulnerability remediation, and secure platform operations.

## Core Technologies

**Cloud:** AWS, Azure, Google Cloud

**AWS:** ECS, EC2, VPC, IAM, ALB, NLB, Route 53, CloudFront, CloudWatch, KMS, EBS, S3, AWS WAF, Secrets Manager, RDS, Auto Scaling, Launch Templates

**Azure:** AKS, Azure Virtual Desktop, Azure AI Hub, Azure OpenAI infrastructure, Azure Monitor, Azure Monitor Agent, Data Collection Rules, Azure Policy, Defender for Storage, Event Grid, Azure Functions, Blob Storage, Private Endpoints, Azure Lighthouse, hub-and-spoke networking, Sentinel alerting, Azure DevOps

**Google Cloud:** GKE, Shared VPC, Cloud Load Balancer, Cloud NAT, Cloud Armor, Cloud DNS, Artifact Registry, Cloud Logging, Cloud Monitoring, Private Service Connect, IAM, Workload Identity, OS Policy Assignments

**Platform:** Kubernetes, Helm, Terraform, Atlantis, GitHub Actions, Azure DevOps, CI/CD, GitOps, ArgoCD

**Delivery:** customer onboarding, automation platform setup, Git repository linkage, Terraform standards, backup validation, policy baselines, environment scans

**Security / Ops:** Zero Trust, CIS Benchmarks, private networking, TLS, certificates, VPN, Linux, Windows, Prometheus, Grafana, Datadog, monitoring agents, security agents, FinOps agents

## Professional Experience

### RapidScale

**Lead Cloud Engineer | 2024 - Present**
**Senior Cloud Engineer, Cloud SME | 2023 - 2024**
**Team Lead, Senior Cloud Support | 2022 - 2023**
**Cloud Support Associate | Early 2022**

- Lead technical engineer across 25+ managed cloud client accounts, including dedicated architecture and delivery ownership for five strategic enterprise customers.
- Own managed services and professional services delivery across AWS, Azure, and Google Cloud, from discovery meetings and architecture review through environment deployment, automation platform installation, validation, ongoing improvement, support, documentation, and handoff.
- Delivered dozens of client projects covering platform redesign, Terraform refactoring, Kubernetes operations, cloud security controls, monitoring and security agent rollout, FinOps agent rollout, backup and policy baselines, infrastructure deployment, and cost optimization.
- Validated client platform onboarding by confirming backups, policies, Git repository linkage, Terraform code layout, agent health, scan findings, and Zero Trust control placement before operational handoff.
- Led an internal client environment review initiative covering security posture, reliability, service lifecycle, orphaned resources, unattached disks, platform hygiene, and FinOps; automated the findings workflow and reduced engineering report effort from roughly 40 hours to 2 hours.
- Used review findings with clients to define first-priority work, balancing immediate risk, operational overhead, cost cleanup, backup gaps, logging gaps, and longer-term modernization goals.
- Created measurable client and internal value through automation, advisory work, lifecycle management, and platform cleanup, including $42K in direct client savings beyond standard support scope.
- Serve as escalation point for complex cloud, Kubernetes, networking, monitoring, and security issues, working directly with customer engineering teams during production incidents and planned maintenance.

### Rackspace

**Linux Cloud Systems Administrator | 2017 - 2021**

- Supported Linux-based cloud environments for production customer workloads, including incident response, OS troubleshooting, access management, patching, and operational support.

## Selected Technical Projects

### Client Platform Onboarding and Automation Enablement

- Onboarded customer environments into automation and managed-service platforms by enabling backup coverage, establishing policy baselines, creating and linking Git repositories, standardizing Terraform folders and files, and deploying the environment foundation.
- Automated monitoring, security tooling, and FinOps agent installation, then validated agent health, data flow, backup and policy coverage, scan findings, and platform functionality before handoff.
- Used environment scans to surface key findings such as orphaned resources, unattached disks, access gaps, logging blind spots, and Zero Trust control placement.
- Supported multi-customer Azure operations through Azure Lighthouse delegated resource management, validating RBAC boundaries, subscription context, managed-service visibility, and cross-tenant administration paths.

### Client Environment Review Automation and Advisory Program

- Led an internal initiative to standardize recurring client environment reviews across security posture, reliability, backup coverage, access paths, logging, service lifecycle, orphaned resources, unattached disks, platform hygiene, and FinOps.
- Implemented automation that collected and organized environment findings so engineers could produce a client-ready review package in roughly 2 hours instead of about 40 hours of manual effort.
- Used the review with clients to align remediation priorities with their goals, separating urgent risk, operational cleanup, cost actions, and longer-term architecture work.

### Azure Defender for Storage Malware Quarantine Automation

- Built an event-driven malware response workflow using Microsoft Defender for Storage, Event Grid, Azure Functions, C# isolated worker, .NET, Blob Storage, Azure Monitor, and Application Insights.
- Processed Defender scan results for uploaded objects, filtered source-container events, ignored clean verdicts, copied malicious blobs to a quarantine container, and preserved the original object for investigation and audit requirements.

### Kubernetes (AKS) Lifecycle and Upgrade Process

- Led staged Kubernetes lifecycle and upgrade work across sandbox, development, stage, production, and shared environments, coordinating control plane, managed add-on, node pool, image, and template changes through customer-approved maintenance windows.
- Moved application platforms through sequential Kubernetes versions with rolling node replacement, health checks, load balancer validation, workload verification, and rollback planning.
- Folded security remediation into lifecycle work by updating worker images and Kubernetes add-ons while reducing production risk through environment sequencing and post-upgrade validation.

### Kubernetes Ingress and Security Policy Controls

- Supported Kubernetes ingress designs around routing, authentication, WAF placement, TLS, network reachability, and application validation without tying the design to one controller pattern.
- Built the AKS equivalent for ingress and security policy work, including private access, RBAC, Azure Policy, network controls, workload identity, monitoring signal flow, and policy validation.

### Kubernetes Platform Troubleshooting and Add-On Operations

- Troubleshot managed Kubernetes platform issues across add-ons, worker image updates, autoscaling behavior, ingress behavior, node pressure, pod placement, resource requests, and monitoring signal gaps.
- Used Datadog, Kubernetes events, node utilization, workload density, provisioning behavior, and add-on state to separate platform problems from application resource pressure.

### AWS Application Platform and Private Networking

- Designed and supported AWS application platforms using Route 53, CloudFront, ALB/NLB, ECS services, managed Kubernetes workloads, EC2 Auto Scaling, private and public subnets, Security Groups, IAM roles, ECR, KMS, EBS, S3, CloudWatch, TLS certificates, and Terraform.
- Planned customer platform changes around DNS ownership, private networking, VPN/firewall paths, WAF placement, service segmentation, high availability, and production cutover responsibilities.

### Azure and Google Cloud Platform Security

- Completed Azure Monitor Agent migrations across thousands of VMs, coordinating Data Collection Rules, workspace mappings, phased rollout plans, and validation for monitoring and security signal continuity.
- Built Azure Policy initiatives aligned with CIS benchmarks and internal security standards, enforcing private endpoint usage, auditing lifecycle risk and drift, and supporting cross-tenant Azure Lighthouse operations for managed-service clients.
- Advised on secure Azure AI and Azure Virtual Desktop platform designs involving private endpoints, DNS, Entra ID, RBAC, WAF placement, session-host identity, and production migration constraints.
- Designed and supported GKE environments using private clusters, regional node pools, Shared VPC, Cloud NAT, Cloud Armor, Cloud DNS, Cloud Logging, Cloud Monitoring, IAM, Workload Identity, and Terraform-based delivery.

### Infrastructure as Code and Managed-Service Standards

- Operated Atlantis-based Terraform workflows with pull-request review gates, controlled infrastructure changes, reduced direct production access, and clear audit trails for customer environments.
- Established Azure managed services standards and Google Cloud support practices covering customer onboarding, governance, post-sales transition, platform review, documentation, and operational handoff.

## Certifications

- AWS Certified: Security - Specialty
- AWS Certified: DevOps Engineer - Professional
- Google Cloud Professional Cloud Architect
- Google Cloud Professional Cloud Security Engineer
- Microsoft Certified: Azure Solutions Architect Expert
- Microsoft Certified: Azure Network Engineer Associate
- Microsoft Certified: Azure AI Engineer Associate
- Microsoft Certified: Azure Virtual Desktop Specialty
- Tanium Deployment Specialist
- Gremlin Certified Chaos Engineering Professional
