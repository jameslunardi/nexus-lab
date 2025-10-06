# nexus-lab

Hybrid cloud test infrastructure combining VMware on-premises and Azure environments.

## Overview

This project provides a fully automated hybrid cloud test environment for learning and development purposes. Infrastructure is deployed using Terraform and connected via site-to-site VPN.

## Components

**On-Premises (VMware Workstation Pro)**
- Active Directory Domain Controller (source domain)
- VPN gateway/router
- Management workstation

**Azure (Terraform)**
- Multiple Virtual Networks
- Active Directory Domain Controllers (source and target domains)
- VPN Gateway
- Network Security Groups
- Site-to-site VPN connection

## Documentation

- Requirements Document *(coming soon)*
- High-Level Design (HLD) *(coming soon)*
- Deployment Guide *(coming soon)*

## Part of Project Nexus

This is part of [Project Nexus](https://github.com/jameslunardi/nexus) - a portfolio initiative for enterprise cloud infrastructure and automation.

## Technology Stack

- Terraform
- Microsoft Azure
- VMware Workstation Pro
- PowerShell
- Active Directory

## License

MIT License
