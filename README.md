# Nexus Lab - Infrastructure as Code

**Terraform-managed virtualised lab environment for Project Nexus**

---

## Overview

This repository contains the Infrastructure as Code (IaC) for the Nexus Lab project. It provides Terraform modules and projects for deploying a complete virtualised Active Directory environment using KVM/libvirt on Arch Linux.

**Part of:** [Project Nexus](https://github.com/jameslunardi/nexus) - A portfolio project demonstrating infrastructure automation and PowerShell development

---

## Repository Contents

### Terraform Modules
Reusable infrastructure components:

- **network** - Virtual network configuration
- **domain-controller** - Windows Server domain controller deployment
- **member-server** - Windows Server member server deployment

### Terraform Projects
Complete infrastructure deployments:

- **infrastructure** - Foundation infrastructure (networks and domain controllers)
- **adsync** - ADSync tool deployment (member server)

### Documentation
Infrastructure-specific documentation:

- **setup/** - Installation guides and prerequisites
- **runbooks/** - Operational procedures and troubleshooting

### Additional Resources

- **images/** - ISO files for VM installation
- **scripts/** - Helper scripts for infrastructure management

---

## Project Structure

```
nexus-lab/
├── terraform/
│   ├── modules/
│   │   ├── network/              # Virtual network module
│   │   ├── domain-controller/    # Domain controller module
│   │   └── member-server/        # Member server module
│   └── projects/
│       ├── infrastructure/       # Foundation infrastructure
│       └── adsync/              # ADSync server deployment
├── docs/
│   ├── setup/                   # Setup and installation guides
│   │   └── iso-details.md       # Windows Server ISO information
│   └── runbooks/                # Operational procedures
├── scripts/                     # Helper scripts
├── images/                      # ISO files (not in Git)
└── README.md                    # This file
```

---

## Infrastructure Overview

### Virtual Machines

**Domain Controllers:**
- **D1DC01** - corp.lab domain controller (10.0.100.10)
- **D2DC01** - prod.lab domain controller (10.0.101.10)

**Member Servers:**
- **ADSYNC01** - ADSync tool host in prod.lab (10.0.101.20)

### Network Architecture

**Domain 1 Network (corp.lab):**
- Subnet: 10.0.100.0/24
- Gateway: 10.0.100.1
- DNS: D1DC01 (10.0.100.10)

**Domain 2 Network (prod.lab):**
- Subnet: 10.0.101.0/24
- Gateway: 10.0.101.1
- DNS: D2DC01 (10.0.101.10)

**Routing:** Libvirt-managed routing between networks

### Active Directory

**Domains:**
- **corp.lab** (CORP) - Source domain on D1DC01
- **prod.lab** (PROD) - Target domain on D2DC01

**Relationship:** Separate forests with no trust (true cross-forest sync scenario)

---

## Prerequisites

### Required Software

```bash
# Arch Linux packages
sudo pacman -S qemu-full libvirt virt-manager terraform

# Enable and start libvirtd
sudo systemctl enable --now libvirtd
sudo usermod -a -G libvirt $USER
```

### Windows Server ISO

- Windows Server 2025 Evaluation Edition
- Location: `/mnt/Dev/nexus-lab/images/`
- See [docs/setup/iso-details.md](docs/setup/iso-details.md) for download information

### Terraform Provider

- Provider: `dmacvicar/libvirt`
- Documentation: https://registry.terraform.io/providers/dmacvicar/libvirt/latest/docs

---

## Quick Start

### 1. Clone the Repository

```bash
git clone git@github.com:jameslunardi/nexus-lab.git
cd nexus-lab
```

### 2. Download Windows Server ISO

See [docs/setup/iso-details.md](docs/setup/iso-details.md) for instructions.

### 3. Deploy Infrastructure

```bash
# Navigate to infrastructure project
cd terraform/projects/infrastructure

# Initialise Terraform
terraform init

# Review the plan
terraform plan

# Deploy infrastructure
terraform apply
```

### 4. Configure Domain Controllers

Follow the runbooks in `docs/runbooks/` to configure the domain controllers after deployment.

---

## Terraform Modules

### Network Module

**Purpose:** Create libvirt virtual networks with optional DHCP and DNS

**Location:** `terraform/modules/network/`

**Usage:**
```hcl
module "domain1_network" {
  source = "../../modules/network"
  
  network_name = "domain1"
  subnet_cidr  = "10.0.100.0/24"
  dhcp_enabled = false
}
```

### Domain Controller Module

**Purpose:** Deploy Windows Server VMs configured as domain controllers

**Location:** `terraform/modules/domain-controller/`

**Usage:**
```hcl
module "dc01" {
  source = "../../modules/domain-controller"
  
  hostname    = "D1DC01"
  domain_name = "corp.lab"
  ip_address  = "10.0.100.10"
  network_id  = module.domain1_network.network_id
}
```

### Member Server Module

**Purpose:** Deploy Windows Server VMs as domain members

**Location:** `terraform/modules/member-server/`

**Usage:**
```hcl
module "adsync01" {
  source = "../../modules/member-server"
  
  hostname   = "ADSYNC01"
  domain     = "prod.lab"
  ip_address = "10.0.101.20"
  network_id = module.domain2_network.network_id
}
```

---

## Documentation

### Infrastructure Documentation (This Repository)

- [ISO Details](docs/setup/iso-details.md) - Windows Server ISO information
- Module READMEs (within each module directory)
- Runbooks (in `docs/runbooks/` - created during implementation)

### Project Documentation

For project-level documentation, see the main [Project Nexus repository](https://github.com/jameslunardi/nexus):

- Project Vision and Goals
- Master Project Plan
- Architecture Overview
- Network Design Specifications
- Git Workflow Guide

---

## Development Workflow

### Making Changes

1. Create or modify Terraform code
2. Format code: `terraform fmt`
3. Validate configuration: `terraform validate`
4. Test deployment in a safe environment
5. Update module READMEs if needed
6. Commit with meaningful message
7. Push to GitHub

### Code Quality Standards

- All code must pass `terraform fmt` and `terraform validate`
- Modules must have complete README files
- Variables must have descriptions
- Use meaningful resource names
- Follow Terraform best practices

### Git Workflow

See the [Git Workflow Guide](https://github.com/jameslunardi/nexus/blob/main/git-workflow.md) in the main nexus repository for detailed Git conventions.

---

## Useful Commands

### Libvirt Management

```bash
# List VMs
virsh list --all

# List networks
virsh net-list --all

# Connect to VM console
virsh console <vm-name>

# VM network info
virsh domifaddr <vm-name>
```

### Terraform Operations

```bash
# Initialise
terraform init

# Format code
terraform fmt

# Validate configuration
terraform validate

# Plan changes
terraform plan

# Apply changes
terraform apply

# Destroy infrastructure
terraform destroy

# List resources
terraform state list
```

---

## Project Phases

### Phase 1: Foundation Infrastructure (Current)
- ✅ Environment preparation
- 🔄 Network module development
- ⏳ Domain controller module development
- ⏳ Infrastructure deployment

### Phase 2: ADSync Infrastructure
- ⏳ Member server module development
- ⏳ ADSync server deployment

### Phase 3: ADSync Application
See [nexus-adsync repository](https://github.com/jameslunardi/nexus-adsync)

---

## Related Repositories

- **[nexus](https://github.com/jameslunardi/nexus)** - Project documentation and architecture
- **[nexus-adsync](https://github.com/jameslunardi/nexus-adsync)** - PowerShell AD synchronisation tool

---

## Support and Documentation

For questions, issues, or additional documentation:

1. Check the [main project documentation](https://github.com/jameslunardi/nexus)
2. Review module READMEs in `terraform/modules/`
3. See operational runbooks in `docs/runbooks/`
4. Consult the [Terraform libvirt provider documentation](https://registry.terraform.io/providers/dmacvicar/libvirt/latest/docs)

---

## Licence

This project is created as a portfolio piece. See the main [nexus repository](https://github.com/jameslunardi/nexus) for licence information.

---

## Author

**James Lunardi**  
GitHub: [@jameslunardi](https://github.com/jameslunardi)

---

**Last Updated:** 17 October 2025  
**Status:** Active Development - Phase 1
