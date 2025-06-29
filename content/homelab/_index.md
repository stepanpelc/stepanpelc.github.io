+++
title = "HomeLab"
tags = []
date = "2019-04-02"
toc = true
+++


### **HomeLab & Self-Hosting Environment**

## Core Infrastructure

- Dual-stack (IPv4/IPv6) network built on UniFi hardware
- Domain configured with valid FQDN and internal **Step-CA** certificate authority supporting ACME
- Hosted on **Proxmox VE**, primarily using **LXC containers** with some VMs for Docker workloads
- High-availability DNS filtering via **Pi-hole**

## Container & Service Management

- **Docker** environment orchestrated with **Portainer**
- **Traefik** used as a dynamic reverse proxy; **NGINX** handles public-facing services
- Single Sign-On (SSO) managed through **Authentik**

## Email & Notification

- Self-hosted **mail server** for technical/system accounts (e.g., alerts, automation)
- **ntfy** used as a push-notification system for **Uptime Kuma** alerts and **DroneCI build status updates**

## Monitoring & Metrics

- **LibreNMS** for SNMP-based network interface monitoring
- **Grafana** for dashboards and infrastructure visualizations
- **InfluxDB** for time-series metric storage and performance data analysis

## Automation & CI/CD

- **Gitea** for self-hosted Git repositories
- **DroneCI** and **Ansible Semaphore** for build pipelines and infrastructure automation

## Applications & Data Management

- **NextCloud** for document storage and file sync
- **BookStack** for internal knowledge base and documentation
- **NetBox** for IP address and network infrastructure management (IPAM)
- **Vaultwarden** for secure, self-hosted password management
- **MinIO** as S3-compatible object storage, primarily used for backups
- **Matomo** for self-hosted web analytics
- **PrivateBin** for encrypted, ephemeral paste sharing
- **Syncthing** for continuous, peer-to-peer file synchronization

## Home Automation

- **Home Assistant** integrated with **Hue** ecosystem and custom dashboards for smart home control
