# Nexus Lab

**Infrastructure as Code for a KVM/libvirt-based homelab environment on Arch Linux.**

---

## Overview

Nexus Lab provides a fully automated virtualised test environment managed through Terraform. This project demonstrates Infrastructure as Code (IaC) practices and serves as the foundation for Active Directory testing and development projects.

The infrastructure is deliberately simplified from the original hybrid cloud vision to focus on practical local development whilst maintaining portfolio-quality code and documentation.

## Current Architecture

**Virtualisation Stack:**
- **Host OS**: Arch Linux
- **Hypervisor**: KVM (Kernel-based Virtual Machine)
- **Management**: libvirt
- **IaC Tool**: Terraform with libvirt provider
- **GUI Tool**: virt-manager (optional)

**Phase 1 Infrastructure:**
- Virtual network with appropriate routing and DNS
- Two Windows Server 2025 Domain Controllers in separate Active Directory domains
- Foundation for inter-domain synchronisation testing

**Future Phases:**
- Member server for running the nexus-adsync tool
- Additional infrastructure as needed for testing and development

## Project Structure

```
.
├── docs/                    # Documentation
│   ├── architecture/        # Architecture diagrams and design decisions
│   ├── setup/              # Setup and installation guides
│   ├── runbooks/           # Operational procedures
│   ├── project-plans/      # Project planning documents
│   └── claude/             # AI assistant context files
├── terraform/
│   ├── modules/            # Reusable Terraform modules
│   │   ├── domain-controller/
│   │   ├── member-server/
│   │   └── network/
│   └── projects/           # Project-specific deployments
│       ├── infrastructure/ # Foundation infrastructure
│       └── adsync/        # ADSync tool infrastructure
├── scripts/                # Helper scripts
└── images/                 # VM base images and ISOs
```

## Technology Stack

- **Hypervisor**: KVM (Kernel-based Virtual Machine)
- **Virtualisation Management**: libvirt
- **Infrastructure as Code**: Terraform with libvirt provider
- **Host Operating System**: Arch Linux
- **Guest Operating System**: Windows Server 2025
- **Version Control**: Git/GitHub

## Getting Started

### Prerequisites
- Arch Linux with KVM support enabled
- Minimum 16GB RAM recommended
- Sufficient disk space for VM images (50GB+)

### Installation
```bash
# Install required packages
sudo pacman -S qemu-full libvirt virt-manager terraform

# Enable and start libvirt service
sudo systemctl enable --now libvirtd

# Add your user to the libvirt group
sudo usermod -a -G libvirt $USER

# Log out and back in for group changes to take effect
```

### Deployment
Detailed deployment instructions are available in the `docs/setup/` directory. The infrastructure is deployed in phases:

1. **Phase 1**: Foundation infrastructure (network + 2 domain controllers)
2. **Phase 2**: Member server for nexus-adsync tool
3. **Phase 3**: Additional infrastructure as needed

## Documentation

Comprehensive documentation is maintained in the `docs/` directory:
- **Setup Guides**: Installation and configuration procedures
- **Architecture Documentation**: Design decisions and infrastructure diagrams
- **Runbooks**: Operational procedures for common tasks
- **Project Plans**: Detailed project planning and tracking

## Part of Project Nexus

This is part of [Project Nexus](https://github.com/jameslunardi/nexus) - a portfolio initiative demonstrating enterprise-level cloud infrastructure and automation capabilities.

### Related Repositories
- **[nexus-adsync](https://github.com/jameslunardi/nexus-adsync)** - PowerShell AD user synchronisation tool

## Architecture Evolution

**Current Implementation**: KVM/libvirt local virtualisation
- Simplified deployment and testing
- Lower resource requirements
- Faster iteration and development
- Full infrastructure control

**Original Vision**: Hybrid cloud (VMware + Azure)
- May be implemented in a future phase
- Current architecture provides solid foundation
- Demonstrates IaC principles effectively

## Portfolio Goals

This project demonstrates:
- Infrastructure as Code practices with Terraform
- Modular, reusable infrastructure design
- Professional documentation and project management
- Active Directory domain architecture
- Network design and configuration
- Version control and Git workflow

## Current Status

**Phase**: 1 - Foundation Infrastructure Setup  
**Stage**: 1.1 - Environment Preparation  
**Status**: In Progress

See `docs/project-plans/plan.md` for detailed project tracking.

## License

MIT License - See LICENSE file for details

---

**Note:** This is a learning and portfolio project. While it demonstrates enterprise patterns and best practices, it is designed for development and testing purposes.

---

**Last Updated**: 17 October 2025
