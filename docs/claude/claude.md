# Project Context for Claude

## Project Overview
This is a homelab infrastructure project for testing, learning, and development. The infrastructure is managed using Terraform with libvirt/KVM/QEMU virtualisation on Arch Linux. The project is designed to be a portfolio piece demonstrating infrastructure as code (IaC) practices and Active Directory synchronisation capabilities.

## Project Methodology
This project is run as a **professional software/infrastructure project** with:
- Formal project planning and tracking
- Clear roles and responsibilities
- Defined phases, stages, and deliverables
- Acceptance criteria for each stage
- Quality standards and best practices
- Regular status updates and reviews
- Comprehensive documentation

## Current Status
**Phase:** 1 - Foundation Infrastructure Setup  
**Stage:** 1.1 - Environment Preparation  
**Status:** Not Started  
**Date Started:** 14 October 2025  
**Location:** `/mnt/Dev/`  
**Blockers:** None

### Progress Summary
- [x] Folder structure created (14 Oct 2025)
- [x] Project plan created (14 Oct 2025)
- [x] Documentation templates established (14 Oct 2025)
- [ ] Stage 1.1: Environment Preparation
- [ ] Stage 1.2: Network Module Development
- [ ] Stage 1.3: Domain Controller Module Development
- [ ] Stage 1.4: Infrastructure Project Development
- [ ] Stage 1.5: Documentation and Git Workflow

## Project Structure

### Repository Organisation
The project uses **separate Git repositories**:

1. **homelab-infrastructure** - Infrastructure and Terraform code (this repository)
2. **adsync-tool** - PowerShell-based Active Directory synchronisation service (separate repository)

### Folder Structure
```
/mnt/Dev/
├── homelab-infrastructure/          # Git repo for infrastructure
│   ├── docs/                        # Documentation
│   │   ├── architecture/            # Architecture diagrams, design decisions
│   │   ├── setup/                   # Setup guides, prerequisites
│   │   ├── runbooks/                # Operational procedures
│   │   ├── project-plans/           # Project planning documents
│   │   └── claude/                  # Claude context files
│   ├── terraform/                   # Terraform code
│   │   ├── modules/                 # Reusable Terraform modules
│   │   │   ├── domain-controller/   # Domain Controller module
│   │   │   ├── member-server/       # Member server module
│   │   │   └── network/             # Network configuration module
│   │   └── projects/                # Specific project deployments
│   │       ├── infrastructure/      # Foundation: DCs, networking
│   │       └── adsync/              # ADSync tool server deployment
│   ├── scripts/                     # Helper scripts (bash, etc.)
│   ├── images/                      # VM base images, ISOs
│   └── README.md
│
├── adsync-tool/                     # Git repo for ADSync application
│   ├── src/                         # PowerShell source code
│   ├── tests/                       # Tests
│   ├── docs/                        # Application documentation
│   └── README.md
│
└── vm-storage/                      # Not in Git - actual VM disk storage
    ├── domain-controllers/          # Domain Controllers
    │   ├── dc01.qcow2               # Domain Controller 1
    │   └── dc02.qcow2               # Domain Controller 2
    └── member-servers/              # Member servers
        └── adsync01.qcow2           # AD Sync Server
```

## Technical Stack

### Virtualisation
- **Platform:** Arch Linux
- **Hypervisor:** KVM (Kernel-based Virtual Machine)
- **Emulator:** QEMU
- **Management:** libvirt
- **GUI Tool:** virt-manager (for visual management and console access)
- **IaC Tool:** Terraform with libvirt provider

### Infrastructure Design Philosophy
- **Terraform Modules:** Organised by server role (domain-controller, member-server, network)
- **Terraform Projects:** Organised by deployment unit (infrastructure, adsync)
- **VM Storage:** Organised by server role (domain-controllers/, member-servers/)

This hybrid approach allows:
- Reusable modules across different projects
- Clear deployment dependencies (infrastructure → adsync)
- Logical organisation of VM disk files

## Project Phases Overview

### Phase 1: Foundation Infrastructure Setup (Current)
**Duration Estimate:** 2-3 weeks  
**Primary Goal:** Establish the core infrastructure and deploy two domain controllers

**Stages:**
1. Environment Preparation - Install tools, obtain ISOs, initialise Git
2. Network Module Development - Create reusable network module
3. Domain Controller Module Development - Create reusable DC module
4. Infrastructure Project Development - Deploy complete infrastructure
5. Documentation and Git Workflow - Complete Phase 1 documentation

**Key Deliverables:**
- Working libvirt/KVM environment
- Network and Domain Controller Terraform modules
- Two operational domain controllers in separate domains
- Complete documentation and runbooks
- Clean Git repository with proper commits

### Phase 2: ADSync Tool Infrastructure
**Duration Estimate:** 1-2 weeks  
**Primary Goal:** Deploy member server and prepare for application development

**Key Deliverables:**
- Member Server Terraform module
- Operational member server joined to domain
- Complete infrastructure ready for tool development

### Phase 3: ADSync Tool Development
**Duration Estimate:** 3-4 weeks  
**Primary Goal:** Develop and test the PowerShell synchronisation service
**Note:** Will have separate project plan in adsync-tool repository

## Initial Project: ADSync Tool

### Purpose
Develop a PowerShell-based service to synchronise user accounts between two separate Active Directory domains.

### Infrastructure Requirements
**Phase 1 - Infrastructure Project:**
- 2x Windows Server 2022 VMs configured as Domain Controllers
- Each DC in a separate domain
- Network configuration to allow communication

**Phase 2 - ADSync Project:**
- 1x Windows Server 2022 VM as a member server
- Runs the ADSync PowerShell service
- Member of one domain, with trust/connectivity to both

### VM Specifications
- **dc01.qcow2** - Domain Controller for Domain 1
- **dc02.qcow2** - Domain Controller for Domain 2  
- **adsync01.qcow2** - Tool server running the synchronisation service

## Portfolio Goals
Both the infrastructure code and the ADSync tool are intended as portfolio pieces to demonstrate:
- Infrastructure as Code practices
- Documentation quality
- Active Directory expertise
- PowerShell development
- Terraform module design
- Network architecture
- Professional project management

## Roles and Responsibilities

### lunardi (Project Lead/Architect/Developer)
**Responsibilities:**
- Make all architectural decisions
- Write all code (Terraform and PowerShell)
- Implement infrastructure and applications
- Execute deployment and configuration tasks
- Review and approve all work
- Manage timelines and priorities
- Final decision maker on all technical matters

### Claude (Project Manager/Guide/Secretary)
**Responsibilities:**
- Maintain project plan and track progress
- Provide guidance and suggestions when requested
- Answer technical questions
- Create documentation templates
- Review documentation for completeness
- Keep project organised
- Update status and tracking documents
- Provide best practice recommendations
- Act as a sounding board for ideas
- **NEVER** make architectural decisions
- **NEVER** write production code without explicit request

## Quality Standards

### Code Quality
- All Terraform code must pass `terraform fmt` and `terraform validate`
- Modules must have complete README documentation
- Variables should have descriptions and sensible defaults
- Use meaningful naming conventions
- Follow Terraform best practices

### Documentation Quality
- All procedures must be tested and verified
- Use clear, professional UK English
- Include diagrams where helpful
- Assume reader has basic Linux/Windows knowledge
- Each document should have a clear purpose

### Git Practices
- Meaningful commit messages
- Commit working code only
- Use tags for phase completions
- Keep .gitignore updated
- Regular commits (don't accumulate large changes)

## Key Design Decisions

### Why Separate Repositories?
The infrastructure and application code serve different purposes:
- Infrastructure can be reused for other projects
- ADSync tool is a standalone application
- Each can be version-controlled independently
- Clearer portfolio presentation

### Why Hybrid Terraform Organisation?
- **Modules by role:** Promotes reusability and follows infrastructure patterns
- **Projects by deployment:** Matches actual workflow and dependencies
- This provides both flexibility and clarity

### Why vm-storage Outside Git?
- VM disk files are large binary files
- They change frequently during operation
- Not suitable for version control
- Can be recreated from Terraform code

## Tools and Technologies to Use

### Required Packages (Arch Linux)
```bash
# Virtualisation stack
qemu-full libvirt virt-manager

# Terraform
terraform

# Libvirt provider for Terraform
# Provider: dmacvicar/libvirt
```

### Key Commands
```bash
# Libvirt
virsh list --all                    # List all VMs
virsh net-list --all                # List all networks
virsh console <vm-name>             # Connect to VM console

# Terraform
terraform init                      # Initialise Terraform
terraform fmt                       # Format code
terraform validate                  # Validate configuration
terraform plan                      # Preview changes
terraform apply                     # Apply changes
terraform destroy                   # Destroy infrastructure
terraform state list                # List resources in state

# Git
git status                          # Check status
git add .                           # Stage all changes
git commit -m "message"             # Commit changes
git tag v1.0-phase1                 # Create tag
git log --oneline                   # View commit history
```

## Risk Management

### Key Risks Identified
1. **Windows Server licensing** - Mitigated by using evaluation edition
2. **KVM/libvirt compatibility** - Mitigated by early testing
3. **Insufficient hardware resources** - Monitor and adjust VM specs
4. **Time constraints** - Prioritise core functionality
5. **Terraform state corruption** - Regular backups, version control
6. **AD configuration complexity** - Thorough research and documentation

## Communication and Updates

### When lunardi Reports Progress
Claude should:
1. Acknowledge the completed work
2. Check acceptance criteria are met
3. Suggest next steps from the project plan
4. Remind about documentation if needed
5. Update internal progress tracking

### Weekly Reviews (Suggested)
- Review completed tasks
- Update project plan with actual progress
- Identify blockers or issues
- Plan next week's priorities
- Update claude.md with current status

### Stage Completions
- Verify all acceptance criteria met
- Update project plan
- Create summary of work completed
- Document lessons learned
- Plan next stage activities

## Important Notes
- All file paths in this documentation use `/mnt/Dev/` as the base
- The user's development directory is mounted at `/mnt/Dev/`
- Always use UK English for documentation
- Focus on clean, professional documentation for portfolio purposes
- Test thoroughly before committing code
- This is run as a real professional project with proper planning and tracking

## Key Documents

### Essential Reading
- **This file** - Overall project context and current status
- **/mnt/Dev/homelab-infrastructure/docs/project-plans/plan.md** - Detailed project plan with all stages and tasks
- **/mnt/Dev/homelab-infrastructure/README.md** - Repository overview

### To Be Created During Project
- Architecture documentation (docs/architecture/)
- Setup guides (docs/setup/)
- Runbooks (docs/runbooks/)
- Module READMEs (terraform/modules/*/README.md)

## Reference Information
- **Terraform Provider:** [dmacvicar/libvirt](https://registry.terraform.io/providers/dmacvicar/libvirt/latest/docs)
- **User:** lunardi
- **Project Start Date:** 14 October 2025
- **Current Date:** 14 October 2025

## Quick Reference

### Directory Paths
```
Infrastructure Code:     /mnt/Dev/homelab-infrastructure/
Network Module:          terraform/modules/network/
DC Module:               terraform/modules/domain-controller/
Member Server Module:    terraform/modules/member-server/
Infrastructure Project:  terraform/projects/infrastructure/
ADSync Project:          terraform/projects/adsync/
Documentation:           docs/
Project Plans:           docs/project-plans/
VM Storage:              /mnt/Dev/vm-storage/
```

### Next Immediate Steps
1. Start Stage 1.1: Environment Preparation
2. Install qemu-full, libvirt, virt-manager
3. Install Terraform
4. Enable libvirtd service
5. Download Windows Server 2022 ISO
6. Initialise Git repository

---

**Document Version:** 2.0  
**Last Updated:** 14 October 2025  
**Next Review:** On completion of Stage 1.1
