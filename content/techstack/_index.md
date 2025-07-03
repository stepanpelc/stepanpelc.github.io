+++
title = "Tech Stack"
tags = []
date = "2019-04-02"
toc = true
+++

This document outlines the technologies and tools I have worked with across various domains such as Kubernetes orchestration, CI/CD, networking, observability, and infrastructure automation. My focus has been primarily on **private cloud environments**, including not only operating within them but also **designing and building** air-gapped and self-hosted infrastructures, rather than public cloud platforms. The **bolded items** represent tools and platforms I have hands-on experience with, often in production environments or in-depth projects. The remaining tools, while included, reflect technologies I’ve explored to a limited extent—used in small-scale setups or evaluated during research.

---

## Operation Systems

**Linux** ⦁ **Mac OS** ⦁ Windows ⦁ UNIX - AIX

### Linux

- Using on daily basis on servers (Ubuntu, SuSE) and workstations (Fedora)
- Experimenting with new generation Rust based tools

### Mac OS

- Used before migrating to Linux environment
- Integrated Apple ecosystem with open-source tools

---

## Programming Languages

**bash** ⦁ **Python** ⦁ Rego ⦁ PHP

---

### bash

- Writing bash scripts for local-scale automation
- Created multiple onle-liners for daily work

### Python

- Wrote multiple scripts for data manipulations
- Created applications in Python (mostly Flask-based)

---

## Application Definition & Image Build

**Helm** ⦁ **Operator Framework** ⦁ **Podman** ⦁ Packer ⦁ Backstage

---

### Helm

- Working with both versions (v2, v3)
- Writing deployment templates and helpers
- Working with multi-layered Charts

### Operator Framework

- Deploying Helm Charts with Ansible automation and GitOps-based configuration and ansible script (concept & realization)

### Podman

- Migration from Docker to Pobman based builds (PoC & implementation)
- Deployed as a docker-less builds for work and self-hosted environments

---

## Continuous Integration & Delivery

**ArgoCD ⦁ k6 ⦁ JenkinsX ⦁ GitHub Actions ⦁ DroneCI** ⦁ flux ⦁ Jenkins

---

### ArgoCD

- Created GitOps structure design (Applications/ApplicationSets/Apps of Apps)
- Defined and implemented of security models for users and applications
- Implemented LDAP integration via dex for a regulated customer

### k6

- Created testing scripts for PoC/demo of Kubernetes rolling deployments (early usage)
- Prepared xk6- kafka benchmarking of Kafka instances (PoC & tunning scripts)

### JenkinsX

- Worked with main customer pipeline for customization and Kubernetes cluster integration ont air-gaped environment
- Created Gradle scripting for CI/CD pipelines

### GitHub Actions

- Wrote auto-build for internal projects (Golang Hugo) with self-hosted runners

### DroneCI

- Created and deployed pipelines for self-hosted auto build and deploy (js, Hugo, ssh deployment)

---

## Database

**Stolon** ⦁ **PostgreSQL** ⦁ MongoDB ⦁ Redis ⦁  MySQL ⦁ Oracle DB

---

### Stolon

- Updated Helm charts & deployment tuning

### PostgreSQL

- Wrote and deployed database based applications

## Streaming & Messaging

**⦁ Strimzi** ⦁ **Kafka** ⦁ RabbitMQ

---

### Strimzi

- Supported as L2/L3 on daily basis for insurance customer environment including migration from legacy system
- Shared knowledge and trained junior team members
- Planed and executed upgrades of Operator and related Kafka clusters and ZooKeeper to KRaft on-line migration

### Kafka

- Worked with kafka-cli
- Did cluster data re-arrangments and configuration tunning

## Scheduling & Orchestration

**⦁ Kubernetes** ⦁ **RKE/RKE2** ⦁ **Open Sovereign Cloud** ⦁ **Rancher** ⦁ **Proxmox** ⦁ **kind** ⦁ k3s ⦁ k0s

---

### Kubernetes

- Have every day hands-on experience since 1.7 version
- Learned the Kubernetes *The Hard Way*
- Supported for platforms based on K8S
- Acted as L3 support for approx. 800+ clusters
- Designed a security model and lifecycle management
- Supported for applications lift & shift to Kubernetes environment
- Had experience with architecture planning, PoCs and consultations

### SuSE RKE / RKE2

- Prepared PoC for gov organization
- Upgraded of Terraform scripts deploying RKE
- Supported integration RKE2 environment to Cisco ACI (Kuberntes/RKE2 support for Cisco specialist)

### Rancher

- Made PoC for gov organization
- Designed of air-gaped deployments / applications store

### Open Sovereign Cloud

- Held deployment and integration support for Lab environment
- Provided daily support for applications on OSC platform for 1.5 year

### Proxmox

- Used in self-hosted environment and lab for running LXC and VMs workload

### kind

- Used as a workspace for PoC and portable presentations of concepts

## Service Mesh

**Istio** ⦁ **Consul**

### Istio

- Managed (managment of/ managed) of Istio based ingresses
- Debugged istio networking errors
- Wrote custom Envoy scripts (header removal, adding additional parameters)

### Consul

- Prepared Consul-as-DNS for Kubernetes internal traffic PoC

---

## Service Proxy

**Envoy** ⦁ **Nginx** ⦁ **Caddy** ⦁ **HAProxy** ⦁ **MetalLB**

### Envoy

- Created custom filters for Istio mesh

### Nginx

- Worked with nginx-based ingress controller
- Used as primary reverse proxy for homelab

### Caddy

- Used for static content in internal services

### HAProxy

- Used for load balancing in PoCs

### MetalLB

- Used as a load balancer for static created environments

---

## Coordination & Service Discovery

**etcd** ⦁ **CoreDNS** ⦁ **ZooKeeper**

---

### etcd

- Solved HA/DR scenarios for air-gapped environment (two datacenters)
- Debugged performance issues related to storage and filesystem

### CoreDNS

- Prepared custom configuration for Consul PoC

### ZooKeeper

- Managed and optimized settings for Kafka clusters

---

## Cloud Native Storage

**Longhorn** ⦁ **Ceph** ⦁ **MINIO**

---

### Longhorn

- Used as storage layer for multiple Rancher/RKE based PoCs

### Ceph

- Worked with RadosGW and CephFS interfaces for Kubernetes storage

---

## Container Runtime

**Containerd** ⦁ **CRI-O** ⦁ **LXC** ⦁ **Podman**

---

### CRI-O

- Migrated form dockerd to cri-o runtime in air-gapped environment
- Created PoC for user namespaces based on runc

### LXC

- Using lxc on daily homelab/self-hosting environment

---

## Cloud Native Network

**Cilium** ⦁ **Calico** ⦁ **Flannel**

---

### Cilium

- Worked with Cilium policy definitions
- Debugged network issues on top of Cilium CNI

### Calico

- Migrated form Flannel to Calico
- Debugged Calico errors alongside with other used technologies (Istio, Envoy, OpenStack networking)

### Flannel

- Created first Flannel based installations on the bare-metal in air-gapped environment

---

## Security & Compliance

**cert-manager** ⦁ **Open Policy Agent** ⦁ **Polaris** ⦁ **Dex** ⦁ **Authentik** ⦁ **step-ca** ⦁ kube-bench ⦁ Keycloak

### cert-manager

- PoC of Automatic certificate provisioning inside and outside of Kubernetes clusters with integration to external CA provider

### Open Policy Agent

- Created complete solution for workload validating (Admissions Webhooks) and authorization of user roles (multiple constraints, negative definitions, generic to specific approach). Written in Rego

### Keycloak

- Helped on Kubernetes integration of project with its own Keycloak instance

### Polaris

- Created validation polices for Kubernetes workload, later replaced by OPA

### Dex

- Implemented LDAP/OAuth integration for ArgoCD service

### Authentik

- Created SSO service for self-hosted environment applications

### step-ca

- Implemented CA

---

## Automation & Configuration

**Ansible** ⦁ **Ansible Semaphore**

### Ansible

- worked on first air-gapped Kubernetes setup automation (pre-kubespray)
- created idempotent servers provisioning
- created ansible based operator for the deploying of system related components

### Ansible Semaphore

- deployed and actively using at home-lab environment

---

## Key Management

**CyberArk Conjur** ⦁ Vault

---

### CyberArk Conjur

- Designed deployment and internal key structure mapping
- Supported product team during implementation phase

---

## Observability

**fluent-bit** ⦁ **Prometheus** ⦁ **Hubble** ⦁ **Grafana** ⦁ **Netdata** ⦁ **OpenSearch**

---

### fluent-bit

- Prepared fluent-bit configuration for bare-metal services

### Prometheus

- Worked with metrics probes and alerts

### Hubble

- Prepared Cilium deployment for specific OSC-based environment

### Grafana

- Created custom dashboards and alerts based on Prometheus metrics

### ElasticSearch

- Prepared integration into the environment, solved certificates rotation

### OpenSearch

- Supported migration into Kubernetes environment
- Helped with ReadonlyREST integration

---

## Continuous Optimization

**Kubecost** ⦁ kube-bench

### Kubecost

- PoC of tool usefulness in air-gapped environment