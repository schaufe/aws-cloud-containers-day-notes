# AWS Container Day

`2022-11-03`

---

- [Intro / Industry Talk](#intro--industry-talk)
  - [AWS Fargate - serverless](#aws-fargate---serverless)
  - [misc](#misc)
- [Platform Engineering](#platform-engineering)
  - [misc](#misc-1)
- [Data on Kubernetes](#data-on-kubernetes)
  - [mics](#mics)
- [Container Security](#container-security)
  - [Shared Responsibility Model](#shared-responsibility-model)
  - [Defenence in Depth (Model)](#defenence-in-depth-model)
- [Operational Excellence](#operational-excellence)
  - [Infrastructure management](#infrastructure-management)
- [Observability](#observability)
- [Reliability](#reliability)
- [Accelerators / Guardrails](#accelerators--guardrails)
  - [AWS Roles](#aws-roles)
  - [Deploying changes](#deploying-changes)
- [Workshop](#workshop)
  - [Handy commands](#handy-commands)

---

## Intro / Industry Talk

https://www.forrester.com/bold/planning-guide-2023/

https://go.forrester.com/wp-content/uploads/2022/08/Forrester-2023-Planning-Isnt-Business-As-Usual.pdf

rationalisation
optimisation

decrease/avoid legacy; favor cloud-native

app modernisation!

### AWS Fargate - serverless

cost optimisation -> aws customers are "spoiled" for choice (burdened maybe?)

### misc

Growingly popular deployment model:
ECS controlplane + Fargate dataplane

fault-tolerant workloads

arm64

Graviton3 => "sustainability?" -> more energy efficient

AWS dominates 80% of the "container world" ?

## Platform Engineering

next 3-5 years looking like:
helping software engineers own their code in prod
phasing out dedicated ops teams
aggressively outsourcing infrastructure

- right level of abstraction
- reduce cognitive load
- self-service for developers

ux research

### misc

CNCF backstage
  - orig built by spotify

humanitech

blueprints (eks)

Apache Spark (Data Analytics)

Kubeflow (Machine Learning)

Kafka (Streaming Data)

## Data on Kubernetes

Data on EKS (DoEKS)
accelerator for building data platforms on cloud

EKS Distro (of Kube)

AWS Batch w/ EKS
batch jobs!
- don't need to provide dedicated clusters

EKS anywhere

KubeCost (on EKS)

Karpenter
- node auto scaler, for Kubernetes clusters
- open source
- coming up in the workshop...

### mics

you build it, you run it

## Container Security

https://aws.github.io/aws-eks-best-practices/security/docs/

### Shared Responsibility Model

- non-static line bet. Customer Responsibilities and AWS responsibilities
  - varies by aws product
  - blurred in some places

### Defenence in Depth (Model)

<onion visual>

Host, Container, ... , Source Code, ... Config

Big "Five"

#### Least Privilege 

- tightly scoped permissions
- short-lived credentials

#### Build security

remove unnecessary binaries and shells
- bringing a "debug" container in your pod
- those debug utilties and commands need not be available all the time

lint your dockerfiles

"de-fang" your containers
- setting a non-root user for running your app/binaries

use immutable tags

Amazon Inspector
- continual scanning
- automate workflows

#### Host security

treat worker nodes as immutable
don't run your updates on your worker nodes (better to destroy and recreate w/ the new stuff like patches)

`Bottlerocker`
- an OS (built from the ground up (by amazon??) for... the plutonic cloud...)
- everything done via api (no running commands directly on the terminal)
- atomic update model
- idea here is you get some host security for free?

#### Runtime security

- apply resource limits
- don't run containers as root
- policy as code


#### Infrastructure security

- security groups for pods
  - (some limitations on # per host)

- network policies
  - segmentation, yay!

#### Secrets Management

https://aws.github.io/aws-eks-best-practices/security/docs/data/

AWS (or EKS?) provides a set of tools to aid in acheiving secrets management
(e.g. AWS KMS envelope)


#### Clusters and Nodes

- Controlplane logs

---

## Operational Excellence

operations driven design

feedback loops (design <-> implementation <-> operations <-> design)

a problem: cost of change in production is high

### Infrastructure management

- infra as code
- testing
- upgrade plan
  - do you even... patch your controlplane?
- automation

litmus tests...
- can you recreate everything from scratch?


## Observability

"If you can't measure it, you can't manage it"

observability on:
- compute
- control plane

> relevant for aws customers, because, shared responsibility model

know the metrics that are limiting factors relevant to your application
- e.g. latency is probably more relevant to applications than CPU (which is not to say CPU shouldn't be observed)

"unlock elasticity"
- do you know the limiting factors for all layers of the system?
- the scability thresholds? (not sure how this one is diff from the first point)
- do you... monitor
- ...
- do you have a method of cost monitoring and allocation?
- do you take cost into consideration at design stage?

cost of managed services vs. running services yourselves

## Reliability

- rto/rpo
- dr plan
- backup
- testing your DR plan and/or backup recovery
- ...

## Accelerators / Guardrails

EKS Blueprints

"Cloud Operating Models"
applications / platform / operations / security 
- team makeup varies
- devops: cross cutting, or sometimes a thing of its own
- some teams are the "wild west"

"sustain optimise grow"

Customer "A" Journey - standing up clusters for safe use

stage 1 - putting up clusters (think about security concerns, meaningful boundaries)
stage 2 - add ons
stage 3 - team/tenant management (to inform the right isolation points, eg. namespaces)
stage 4 - workload management

### AWS Roles

blueprints addresses some gap & applies roles to Kubernetes (should come up in the workshops)

### Deploying changes

some deployment options:

via aws owner
- aws apis

via gitops
- e.g. via argocd

via helm + terraform
- a bit slow
- full cluster deploy even for just one small change

## Workshop

https://dashboard.eventengine.run/login

fa2a-10dfd35464-64

### Handy commands

``` bash
kubectl auth can-i create pods --namespace kube-system
```


