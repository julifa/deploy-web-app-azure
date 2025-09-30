# Write-up Template

### Analyze, choose, and justify the appropriate resource option for deploying the app.

*For **both** a VM or App Service solution for the CMS app:*
- *Analyze costs, scalability, availability, and workflow*
- *Choose the appropriate solution (VM or App Service) for deploying the app*
- *Justify your choice*

#### Option A: Azure App Service (PaaS)

Costs: Must pay for each plan or tier (shared to premium). There is no cost in time or people for OS patching. Built-ins like autoscale, slots, backups, and TLS cut down on third-party costs and operational costs.

Scalability: Vertical scaling happens in seconds, and horizontal scaling happens automatically based on CPU, RAM, requests, and queue length. There is no need to make VM Scale Sets or load balancers.

Availability: The platform is backed by a SLA, health probes, and deployment slots that allow for zero downtime. Traffic Manager/Front Door makes it easy to set up regional redundancy.

Workflow: Streamlined DevEx with GitHub Actions/Azure DevOps, az webapp up, container support, Managed Identity for Key Vault/Storage/Service Bus, App Insights, and built-in logging. Very little infrastructure to keep up.

#### Option B: Virtual Machine (IaaS)

Costs: VM, disk, LB, NSG, and (optionally) VMSS. At small scales, the lower price of raw compute is often canceled out by the time it takes to do operations (patching, hardening, backups, and observability).

It is needed to be designed (VMSS, autoscale rules, custom images) so that it can grow. Deployments need careful planning to avoid downtime.

Availability: Must manage Nginx/Gunicorn, systemd, OS updates, failover and health checks, and blue/green rollouts.

Workflow: The user has full control over the workflow (root, custom packages/drivers), but there is needed to do CI/CD, monitoring, certificates, and secret management yourself or through extra services connected to.

#### Azure App Service is the option.

Justification: It makes operations easier by providing first-rate integrations (Managed Identity, Key Vault, Service Bus, Storage), autoscaling, deployment slots, and observability right out of the box. App Service gives you a faster time-to-prod, easier scaling, and higher reliability with fewer moving parts for a Flask CMS that works with a standard Linux runtime or container and doesn't need OS-level customization.


### Assess app changes that would change your decision.

*Detail how the app and any other needs would have to change for you to change your decision in the last section.* 

If one or more of the following happened, I would rather have a VM than an App Service:

OS/Kernel/Driver needs: Native libraries, custom FFmpeg codecs, system packages, or GPU/CUDA needs that go beyond what App Service can handle.

Networking and security requirements: full host administration, deep control of iptables, custom routing, host-based proxies/agents, or compliance policies.

Nonstandard protocols: Services that need custom listeners other than standard HTTP/WebSockets or strict connection lifetimes/timeouts can't work with a managed front door.

If the app went in those directions, a VM would be a better fit because they give you more control. For today's CMS needs, though, App Service is still the best choice.


# IMPORTANT NOTE:

There are project’s packages that cannot be installed due to library errors, therefore I cannot work with the code.

![Error Image 1](required_images/Error%20installing%20dependencies%20I.png)
![Error Image 2](required_images/Error%20Installing%20dependecies%20II.png)