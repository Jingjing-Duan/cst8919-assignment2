# CST8919 Assignment 2 – Cloud Service Alternatives Report

## Introduction

Throughout CST8919, most of the security and compliance work was completed using Microsoft Azure. We used Azure services for identity management, logging, security monitoring, governance, and threat detection.

For this assignment, I looked at how the same security requirements could be handled if an organization was using AWS or Google Cloud instead of Azure.

Rather than treating every service as a direct one-to-one replacement, I found that the three cloud providers sometimes organize the same capabilities differently. Azure often combines several functions into one service, while AWS may use several smaller services together. Google Cloud is somewhere in between.

The five areas compared in this report are:

* Identity and access management
* Monitoring and logging
* Cloud governance
* Cloud security posture and threat protection
* SIEM and security automation

---

# Quick Service Mapping

| Security Area       | Azure                         | AWS                                                | Google Cloud                     |
| ------------------- | ----------------------------- | -------------------------------------------------- | -------------------------------- |
| Identity and SSO    | Microsoft Entra ID            | AWS IAM + IAM Identity Center                      | Cloud Identity + IAM             |
| Monitoring and Logs | Azure Monitor + Log Analytics | Amazon CloudWatch                                  | Cloud Monitoring + Cloud Logging |
| Governance          | Azure Policy                  | AWS Config + Service Control Policies              | Organization Policy              |
| Cloud Security      | Microsoft Defender for Cloud  | Security Hub + GuardDuty + Inspector               | Security Command Center          |
| SIEM / SOAR         | Microsoft Sentinel            | Security Lake + Security Hub + automation services | Google Security Operations       |

One thing I noticed during this comparison is that the word **equivalent** does not always mean that there is one exact replacement. In AWS especially, multiple services may be required to provide the same overall capability as one Azure product.

---

# 1. Identity and Access Management

## What we used in Azure

Microsoft Entra ID is the main identity platform in Azure. Earlier versions of the service were called Azure Active Directory.

In CST8919, identity was important because we studied topics such as:

* Single Sign-On
* Federated identity
* Authentication
* Authorization
* Role-based access
* Multi-Factor Authentication

Entra ID can manage users, groups, applications, and authentication. It also supports SSO so that one identity can be used to access multiple applications.

A major security feature is Conditional Access. This allows access decisions to consider information such as the user's identity, device, location, or risk level.

---

## AWS Alternative

AWS does not use one single service for exactly the same purpose.

The two closest services are:

**AWS IAM** and **AWS IAM Identity Center**.

AWS IAM is mainly used to control access to AWS resources. Administrators can create users, roles, and policies that define exactly what actions are allowed.

IAM Identity Center provides centralized workforce access and SSO across multiple AWS accounts and applications.

For example, instead of giving a developer permanent administrator credentials, an organization can assign a role with only the permissions required for that developer's work.

---

## Google Cloud Alternative

Google Cloud also divides identity responsibilities between two services.

**Cloud Identity** manages users and organizational identities, while **Google Cloud IAM** manages permissions to cloud resources.

IAM uses roles and permissions to decide who can perform an action on a resource.

Google also supports service accounts and workload identity federation, which are useful for applications and automated systems.

---

## My Comparison

The three platforms all support the same basic security idea: users and applications should receive only the permissions they actually need.

This is the **principle of least privilege**.

Azure Entra ID feels more closely connected to employee identity because it is also widely integrated with Microsoft 365.

AWS IAM is very strong for controlling permissions between cloud resources.

Google Cloud IAM has a similar role-based model and integrates closely with Google Cloud projects and organizations.

For DevSecOps, all three can avoid storing permanent credentials in deployment pipelines by using roles, service identities, or federation.

---

# 2. Monitoring and Logging

## What we used in Azure

Azure Monitor and Log Analytics were especially relevant to the practical work in this course.

In my earlier assignment, I used logging to record application activity and then used **Kusto Query Language (KQL)** to search the logs.

For example, security logs can be used to detect:

* Repeated access to a protected route
* Failed authentication
* Unexpected application behavior
* Infrastructure failures

Azure Monitor collects metrics and logs from cloud resources.

Log Analytics allows administrators to query those logs using KQL.

Alerts can then be created based on a query or monitoring condition.

The basic process is:

`Application / Resource → Logs → Log Analytics → KQL → Alert`

---

## AWS Alternative

The closest AWS service is **Amazon CloudWatch**.

CloudWatch collects logs and metrics from AWS infrastructure and applications.

CloudWatch can be used to:

* Store application logs
* Monitor CPU and memory-related metrics
* Create dashboards
* Search log data
* Create alarms

AWS CloudTrail is also commonly used together with CloudWatch because CloudTrail records AWS API activity.

For example, if someone modifies IAM permissions or changes an AWS security configuration, CloudTrail can record the action and CloudWatch can generate an alarm.

---

## Google Cloud Alternative

Google Cloud separates these functions into:

* Cloud Monitoring
* Cloud Logging

Cloud Monitoring handles metrics, dashboards, and alerts.

Cloud Logging collects application and infrastructure logs.

Like Azure Log Analytics, administrators can search collected information to troubleshoot problems or investigate suspicious activity.

---

## My Comparison

This area has the clearest similarity between the three platforms.

The general workflow is almost the same:

`Collect → Store → Query → Alert`

Azure uses Azure Monitor and Log Analytics.

AWS mainly uses CloudWatch.

Google Cloud uses Cloud Monitoring and Cloud Logging.

The biggest difference for me is the query experience. In Azure, we worked directly with KQL, which is very useful for security investigations.

From a DevSecOps perspective, these services are important because a successful deployment does not mean the system will continue working correctly. Monitoring provides visibility after deployment.

---

# 3. Cloud Governance and Policy

## What we used in Azure

Azure Policy was one of the most practical governance tools we used in CST8919.

In our Azure Policy lab, policies could enforce rules such as:

* Only allow resources in an approved region
* Require a specific tag
* Prevent creation of a public IP

This showed the difference between simply writing a security rule in documentation and actually enforcing the rule technically.

For example, instead of telling developers:

> "Please only deploy resources to Canada Central."

Azure Policy can automatically deny a deployment to another region.

Azure Policy supports several effects, including:

* Audit
* Deny
* Modify
* DeployIfNotExists

Policies can also be grouped into an **initiative**.

This is useful when an organization needs many governance controls at the same time.

---

## AWS Alternative

AWS does not have one exact service that works exactly like Azure Policy.

The closest combination is:

* AWS Config
* AWS Organizations Service Control Policies

AWS Config checks whether resource configurations follow required rules.

For example, it can detect whether encryption is enabled or whether a resource follows a required configuration.

Service Control Policies, usually called SCPs, provide organization-level permission boundaries.

An important difference is that AWS Config is mainly used to **detect** compliance problems, while SCPs can **prevent** certain actions.

Because of this, the two services together are closer to the overall capability of Azure Policy.

---

## Google Cloud Alternative

The closest Google Cloud service is **Organization Policy Service**.

Organization Policy allows administrators to define restrictions across:

* Organizations
* Folders
* Projects

For example, an organization could restrict where resources are deployed or prevent certain insecure configurations.

Policies are inherited through the Google Cloud resource hierarchy.

---

## My Comparison

I think Azure Policy is one of the easier services to understand because policy evaluation and enforcement are provided through one governance system.

AWS uses a more distributed model.

AWS Config answers the question:

**"Is this resource compliant?"**

SCPs answer another question:

**"Should this action be allowed at all?"**

Google Organization Policy is closer to Azure Policy because both can enforce centralized constraints across a resource hierarchy.

For DevSecOps, all three approaches can be managed as code using Terraform, CLI tools, APIs, or CI/CD pipelines.

This is important because governance rules should be version controlled just like application code.

---

# 4. Cloud Security Posture and Threat Protection

## What we used in Azure

Microsoft Defender for Cloud gives security teams a centralized view of the security posture of cloud resources.

It can identify weaknesses such as:

* Missing security controls
* Vulnerable workloads
* Exposed resources
* Weak configurations
* Suspicious activity

One feature I find useful is the idea of a security score or security recommendations.

Instead of only reporting that something is wrong, the platform can recommend actions to improve the environment.

Defender for Cloud can protect different types of workloads such as virtual machines, containers, databases, and storage.

---

## AWS Alternative

AWS provides similar functionality through several services.

The main ones are:

### AWS Security Hub

Security Hub collects security findings and provides a centralized security view.

### Amazon GuardDuty

GuardDuty focuses on threat detection.

It analyzes activity and looks for suspicious behavior.

### Amazon Inspector

Inspector focuses more on vulnerability management and scanning supported workloads.

Together, these services cover much of the same area as Microsoft Defender for Cloud.

---

## Google Cloud Alternative

The closest Google Cloud service is **Security Command Center**.

Security Command Center provides a centralized view of security findings across Google Cloud resources.

It can help identify:

* Misconfigurations
* Vulnerabilities
* Threats
* Exposed resources
* Security risks

It also provides security posture and threat detection capabilities.

---

## My Comparison

This was another area where I found AWS architecture different.

Azure packages many security capabilities under Defender for Cloud.

Google does something similar with Security Command Center.

AWS separates those responsibilities between Security Hub, GuardDuty, Inspector, and other security services.

There is no technical problem with either design, but the AWS model may require more integration between services.

For a DevSecOps team, these tools are useful because security problems can be discovered continuously rather than waiting for a manual security review.

---

# 5. SIEM and Security Automation

## What we used in Azure

Microsoft Sentinel is a cloud-native SIEM and SOAR platform.

**SIEM** means Security Information and Event Management.

Its main purpose is to collect security data from many systems and analyze the information together.

Sentinel can receive data from:

* Microsoft Entra ID
* Azure resources
* Microsoft Defender
* Firewalls
* Applications
* Other cloud platforms
* On-premises systems

It can then use analytics rules to detect suspicious patterns.

Sentinel also supports incident investigation, threat hunting, dashboards, and automated response.

The SOAR part refers to:

**Security Orchestration, Automation and Response.**

For example, a Sentinel incident can trigger a Logic App playbook that automatically performs a response action.

---

## AWS Alternative

AWS does not currently have one direct service that matches every Microsoft Sentinel feature.

A comparable AWS solution could use several services together.

For example:

* Amazon Security Lake
* AWS Security Hub
* Amazon OpenSearch Service
* EventBridge
* Lambda

Security Lake can centralize security data.

Security Hub collects findings.

OpenSearch can be used for searching and analyzing information.

EventBridge and Lambda can automate response actions.

A simplified workflow could be:

`Security Finding → EventBridge → Lambda → Automated Response`

---

## Google Cloud Alternative

Google's closest direct alternative is **Google Security Operations**.

It provides both SIEM and SOAR capabilities.

Security teams can use it to:

* Collect security telemetry
* Search security events
* Detect threats
* Investigate incidents
* Manage cases
* Run automated playbooks

This makes Google Security Operations much closer to Microsoft Sentinel than the AWS combination.

---

## My Comparison

Microsoft Sentinel and Google Security Operations are the most direct competitors in this area because both are designed as complete security operations platforms.

AWS provides many of the same building blocks, but organizations may need to design their own combination of services.

One advantage of Sentinel is its integration with the Microsoft ecosystem.

For example:

`Entra ID → Azure Monitor → Defender → Sentinel → Logic Apps`

This creates a connected workflow from identity and monitoring to detection and automated response.

---

# Pricing Comparison

Instead of comparing exact dollar values, I focused on the pricing model because cloud prices depend heavily on usage.

| Area           | Azure                                                                                                         | AWS                                                                                             | Google Cloud                                                           |
| -------------- | ------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| Identity       | Basic identity features available with Microsoft services; advanced features may require additional licensing | IAM is generally available without separate charge; users pay for AWS resources                 | IAM core capabilities generally have no separate charge                |
| Monitoring     | Mainly based on log ingestion, retention, and monitoring usage                                                | Based on logs, metrics, alarms, dashboards, and queries                                         | Mainly based on monitoring/logging data volume and retention           |
| Policy         | Core Azure Policy does not normally have a separate basic enforcement charge                                  | Config charges based on recorded configurations and evaluations; SCPs are part of Organizations | Organization Policy normally has no major separate basic charge        |
| Cloud Security | Advanced Defender plans are charged depending on protected resources                                          | Security Hub, GuardDuty, and Inspector use separate usage-based pricing                         | Security Command Center has different service tiers                    |
| SIEM           | Strongly affected by security data ingestion and retention                                                    | Depends on the combination of security, storage, and analytics services                         | Based on the Security Operations package and security telemetry volume |

A common pattern appears across all three cloud providers:

**The more logs and security data an organization collects, the higher the monitoring and SIEM cost can become.**

For this reason, log retention and data ingestion should also be considered part of cloud security architecture.

---

# Security and Compliance

All three cloud providers support major security and compliance programs.

Examples include:

* ISO 27001
* SOC reports
* PCI DSS
* FedRAMP
* Privacy and data protection requirements

However, I think an important point from a security and compliance perspective is that choosing a compliant cloud provider does not automatically make an application compliant.

The cloud provider secures the cloud platform, but customers still need to correctly configure:

* IAM
* Network security
* Encryption
* Logging
* Policies
* Applications
* Data access

This is part of the shared responsibility model.

---

# DevSecOps Integration

The services compared in this report are also relevant to DevSecOps because security should not only happen after an application has been deployed.

The three cloud platforms all support automation through tools such as:

* Terraform
* Command-line tools
* APIs
* GitHub Actions
* Native CI/CD platforms

For example, a DevSecOps workflow could include:

1. A developer pushes infrastructure code.
2. A CI/CD pipeline validates the code.
3. IAM controls which identity can deploy it.
4. Cloud policies prevent insecure resources.
5. Monitoring collects logs after deployment.
6. Security services identify vulnerabilities or threats.
7. SIEM tools correlate security events.
8. Automation responds to high-risk incidents.

This shows that identity, governance, monitoring, and security operations are not separate topics. They work together throughout the cloud development lifecycle.

---

# Final Comparison

After comparing the three platforms, I do not think there is one provider that is always better in every security area.

**Azure** provides strong integration between its security products. Since the services connect closely with Entra ID, Azure Monitor, Defender, and Sentinel, it can be easier to build a unified Microsoft security environment.

**AWS** provides a large number of specialized services. This gives architects flexibility, but equivalent functionality may require several services working together.

**Google Cloud** provides a relatively simple security service structure. Security Command Center and Google Security Operations provide centralized security capabilities similar to Defender for Cloud and Sentinel.

The most important lesson from this comparison is that the product names are different, but the main security goals remain the same:

* Verify identities
* Apply least privilege
* Enforce governance rules
* Collect logs
* Detect threats
* Investigate incidents
* Automate security response

Understanding these concepts makes it easier to move from one cloud provider to another because the security principles remain similar even when the implementation changes.

---

# References

This report was based mainly on official cloud provider documentation:

* Microsoft Learn – Microsoft Entra ID
* Microsoft Learn – Azure Monitor
* Microsoft Learn – Azure Policy
* Microsoft Learn – Microsoft Defender for Cloud
* Microsoft Learn – Microsoft Sentinel
* AWS Documentation – AWS IAM and IAM Identity Center
* AWS Documentation – Amazon CloudWatch
* AWS Documentation – AWS Config
* AWS Documentation – AWS Organizations
* AWS Documentation – AWS Security Hub
* AWS Documentation – Amazon GuardDuty
* AWS Documentation – Amazon Inspector
* AWS Documentation – Amazon Security Lake
* Google Cloud Documentation – Cloud IAM
* Google Cloud Documentation – Cloud Monitoring and Logging
* Google Cloud Documentation – Organization Policy
* Google Cloud Documentation – Security Command Center
* Google Cloud Documentation – Google Security Operations

---

**Course:** CST8919 – DevOps Security and Compliance
**Assignment:** Assignment 2 – Cloud Service Alternatives Report
