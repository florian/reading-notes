# Part I – Introduction

## 1. Introduction

- Popular SWE saying:
> Hope is not a strategy
- Development vs operation
    - Historically, development and operations were two distinct teams: the ones of software engineers and sysadmins
    - These groups try to find a trade-off between their extreme goals of “We want to launch anything, any time, without hindrance" and "We won’t want to ever change anything in the system once it works"
    - Google has a somewhat different setup, consisting of *site reliability engineers* (*SREs*)
    - > SRE is what happens when you ask a software engineer to design an operations team
    - > 50–60% are Google Software Engineers, or more precisely, people who have been hired via the standard procedure for Google Software Engineers. The other 40–50% are candidates who were very close to the Google Software Engineering qualifications (i.e., 85–99% of the skill set required), and who ***in addition*** had a set of technical skills that is useful to SRE but is rare for most software engineers. By far, UNIX system internals and networking (Layer 1 to Layer 3) expertise are the two most common types of alternate technical skills we seek
    - Google computed statistics on the career progression of these groups and found no significant difference between them
    - SREs are meant to automate their tasks, with the goal of not having costs scale linearly with service size. This is achieved by capping the amount of op work (on-call, responding to tickets, etc.) SREs should do at 50% of their time. The remaining time should be spend on automating things
- Outside of Google, the term “DevOps” has become popular for this kind of role
- The goal of an SRE is to keep services running at high availability and low latency
- *SLO*: Service-level objective states a service's concrete serving objectives
- Tenets of SRE work
    - 50% engineering: SREs try to automate things in this time
    - Enabling high development velocity without hurting availability/latency of a service: Products define an *error budget*, i.e. what kind of downtime is okay for the service. While 100% is required for pacemakers, it rarely is for web services as users cannot tell the difference between 100% and 99.99%. While the error budget is not yet overdrawn, product teams may launch new things
    - Monitoring: This should be as automated as possible, only pulling in people when really necessary. When people are pulled in, it should be clearly described how sensitive the issue is, i.e. does it require immediate attention or not
    - Emergency management: When services do go down, they should be up as quickly as possible again
    - Change management: Make rolling out changes as efficient and safe as possible
    - Capacity planning: Ensure services have enough resources for their predicted load
