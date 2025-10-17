# Project Context for Claude

## Project Overview
This is the Nexus Lab infrastructure project for testing, learning, and development. The infrastructure is managed using Terraform with libvirt/KVM/QEMU virtualisation on Arch Linux. The project is designed to be a portfolio piece demonstrating infrastructure as code (IaC) practices and Active Directory synchronisation capabilities.

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
**Stage:** 1.2.2 - Create Network Module  
**Status:** In Progress  
**Date Started:** 17 October 2025  
**Location:** `/mnt/Dev/nexus-lab/`  
**Blockers:** None

### Progress Summary
- [x] Project structure created and consolidated into Nexus repos (17 Oct 2025)
- [x] Stage 1.1: Environment Preparation (17 Oct 2025)
  - [x] Installed qemu-full, libvirt, virt-manager, Terraform
  - [x] Enabled and started libvirtd service
  - [x] Verified KVM support and permissions
  - [x] Downloaded Windows Server 2025 ISO
  - [x] Verified ISO checksum
  - [x] Initialised Git repositories (nexus, nexus-lab, nexus-adsync)
  - [x] Configured SSH authentication with GitHub
  - [x] Documented Git workflow
- [x] Stage 1.2.1: Design Network Architecture (17 Oct 2025)
  - [x] Defined dual-subnet architecture (10.0.100.0/24 and 10.0.101.0/24)
  - [x] Specified domain names (corp.lab and prod.lab)
  - [x] Allocated static IPs for all VMs
  - [x] Documented network topology and routing strategy
  - [x] Created architecture diagram and overview
- [ ] Stage 1.2.2: Create Network Module
- [ ] Stage 1.2.3: Test Network Module
- [ ] Stage 1.3: Domain Controller Module Development
- [ ] Stage 1.4: Infrastructure Project Development
- [ ] Stage 1.5: Documentation and Git Workflow

## Project Structure

### Repository Organisation
The project uses **separate Git repositories** under the Nexus project umbrella:

1. **nexus** - Overall project documentation, vision, and architecture (main repository)
2. **nexus-lab** - Infrastructure code and Terraform modules (this repository)
3. **nexus-adsync** - PowerShell-based Active Directory synchronisation service

**GitHub Organisation:** https://github.com/jameslunardi

### Folder Structure
```
/mnt/Dev/
├── nexus/                           # Git repo for project documentation
│   ├── nexus-vision.md              # Project vision and goals
│   ├── architecture.drawio          # Complete architecture diagram
│   ├── architecture-overview.md     # Architecture documentation
│   └── README.md
│
├── nexus-lab/                       # Git repo for infrastructure
│   ├── docs/                        # Documentation
│   │   ├── architecture/            # Architecture diagrams, design decisions
│   │   │   └── network-design.md    # Network architecture specification
│   │   ├── setup/                   # Setup guides, prerequisites
│   │   │   ├── iso-details.md       # ISO documentation
│   │   │   └── git-workflow.md      # Git workflow guide
│   │   ├── runbooks/                # Operational procedures
│   │   ├── project-plans/           # Project planning documents
│   │   │   └── plan.md              # Master project plan
│   │   └── claude/                  # Claude context files
│   │       └── claude.md            # This file
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
├── nexus-adsync/                    # Git repo for ADSync application
│   ├── src/                         # PowerShell source code
│   ├── tests/                       # Pester tests
│   ├── docs/                        # Application documentation
│   └── README.md
│
└── vm-storage/                      # Not in Git - actual VM disk storage
    ├── domain-controllers/          # Domain Controllers
    │   ├── D1DC01.qcow2             # Domain Controller 1 (corp.lab)
    │   └── D2DC01.qcow2             # Domain Controller 2 (prod.lab)
    └── member-servers/              # Member servers
        └── ADSYNC01.qcow2           # AD Sync Server (prod.lab)
```

## Technical Stack

### Virtualisation
- **Platform:** Arch Linux with Gnome 49
- **Hypervisor:** KVM (Kernel-based Virtual Machine)
- **Emulator:** QEMU
- **Management:** libvirt
- **GUI Tool:** virt-manager (for visual management and console access)
- **IaC Tool:** Terraform with libvirt provider

### Network Architecture
- **Domain 1 (corp.lab):** 10.0.100.0/24
  - D1DC01: 10.0.100.10 (Domain Controller)
- **Domain 2 (prod.lab):** 10.0.101.0/24
  - D2DC01: 10.0.101.10 (Domain Controller)
  - ADSYNC01: 10.0.101.20 (Member Server)
- **Routing:** Libvirt built-in routing between networks
- **DHCP:** Disabled (all VMs use static IPs)

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
1. ✅ Environment Preparation - Install tools, obtain ISOs, initialise Git
2. 🔄 Network Module Development - Create reusable network module
3. ⏳ Domain Controller Module Development - Create reusable DC module
4. ⏳ Infrastructure Project Development - Deploy complete infrastructure
5. ⏳ Documentation and Git Workflow - Complete Phase 1 documentation

**Key Deliverables:**
- ✅ Working libvirt/KVM environment
- 🔄 Network and Domain Controller Terraform modules
- ⏳ Two operational domain controllers in separate domains
- ✅ Complete documentation and runbooks
- ✅ Clean Git repository with proper commits

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
**Note:** Will have separate project plan in nexus-adsync repository

## Infrastructure Specifications

### VM Specifications
- **D1DC01** (Domain Controller 1)
  - Domain: corp.lab
  - IP: 10.0.100.10
  - OS: Windows Server 2025
  - Role: Domain Controller, DNS Server
  
- **D2DC01** (Domain Controller 2)
  - Domain: prod.lab
  - IP: 10.0.101.10
  - OS: Windows Server 2025
  - Role: Domain Controller, DNS Server

- **ADSYNC01** (Member Server - Phase 2)
  - Domain: prod.lab (member)
  - IP: 10.0.101.20
  - OS: Windows Server 2025
  - Role: AD Sync Tool host

### Active Directory Domains
- **corp.lab** (Domain 1)
  - NetBIOS: CORP
  - Forest: Separate forest
  - DC: D1DC01

- **prod.lab** (Domain 2)
  - NetBIOS: PROD
  - Forest: Separate forest
  - DC: D2DC01

**Domain Relationship:**
- Completely separate forests (no trust)
- Cross-domain connectivity via routing
- ADSync tool synchronises users from corp.lab to prod.lab

## Portfolio Goals
The Nexus project (infrastructure + ADSync tool) is intended as a portfolio piece to demonstrate:
- Infrastructure as Code practices with Terraform
- Documentation quality and project management
- Active Directory expertise
- PowerShell development
- Terraform module design
- Network architecture
- Professional project management
- AI-assisted development workflow

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
The infrastructure, application, and project documentation serve different purposes:
- Infrastructure can be reused for other projects
- ADSync tool is a standalone application
- Each can be version-controlled independently
- Clearer portfolio presentation
- Main nexus repo provides project-level documentation

### Why Simplified Architecture (Local KVM vs Hybrid Cloud)?
Original vision included VMware + Azure hybrid cloud:
- Simplified to focus on core skills (IaC, PowerShell, AD)
- Faster development and testing cycles
- Lower complexity and cost
- Local environment sufficient for portfolio demonstration
- Can demonstrate cloud principles without cloud costs
- Hybrid cloud can be added in future phase if desired

### Why Separate Subnets?
- More realistic enterprise scenario
- Demonstrates multi-network management
- Shows routing capabilities
- Better simulates real AD sync scenarios

### Why No DHCP Initially?
- Only three VMs, all infrastructure servers
- Static IPs are standard for servers
- Simpler configuration and troubleshooting
- Can be added later if needed

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
git push origin main                # Push to GitHub
```

## Risk Management

### Key Risks Identified
1. **Windows Server licensing** - Mitigated by using evaluation edition (180 days)
2. **KVM/libvirt compatibility** - Mitigated by early testing
3. **Insufficient hardware resources** - Monitor and adjust VM specs
4. **Time constraints** - Prioritise core functionality
5. **Terraform state corruption** - Version control, regular backups
6. **AD configuration complexity** - Thorough research and documentation

## Communication and Updates

### When lunardi Reports Progress
Claude should:
1. Acknowledge the completed work
2. Check acceptance criteria are met
3. Suggest next steps from the project plan
4. Remind about documentation if needed
5. Update internal progress tracking

### Stage Completions
- Verify all acceptance criteria met
- Update project plan
- Create summary of work completed
- Document lessons learned
- Plan next stage activities

## Important Notes
- All file paths use `/mnt/Dev/` as the base
- Always use UK English for documentation
- Focus on clean, professional documentation for portfolio purposes
- Test thoroughly before committing code
- This is run as a real professional project with proper planning and tracking
- Git authentication uses SSH with GitHub
- User email for Git: 217091044+jameslunardi@users.noreply.github.com

## Key Documents

### Essential Reading
- **This file** - Overall project context and current status
- **/mnt/Dev/nexus-lab/docs/project-plans/plan.md** - Detailed project plan with all stages and tasks
- **/mnt/Dev/nexus/nexus-vision.md** - Project vision and goals
- **/mnt/Dev/nexus-lab/docs/architecture/network-design.md** - Network architecture specification
- **/mnt/Dev/nexus/architecture-overview.md** - Complete architecture documentation

### Documentation Created
- ISO details (docs/setup/iso-details.md)
- Git workflow guide (docs/setup/git-workflow.md)
- Network architecture design (docs/architecture/network-design.md)
- Architecture overview (in nexus repo)

### To Be Created During Project
- Network module README (terraform/modules/network/README.md)
- Domain Controller module README (terraform/modules/domain-controller/README.md)
- Member Server module README (terraform/modules/member-server/README.md)
- Runbooks (docs/runbooks/)

## Reference Information
- **Terraform Provider:** [dmacvicar/libvirt](https://registry.terraform.io/providers/dmacvicar/libvirt/latest/docs)
- **User:** lunardi
- **Project Start Date:** 17 October 2025
- **GitHub:** https://github.com/jameslunardi

## Quick Reference

### Directory Paths
```
Main Documentation:      /mnt/Dev/nexus/
Infrastructure Code:     /mnt/Dev/nexus-lab/
ADSync Code:             /mnt/Dev/nexus-adsync/
Network Module:          /mnt/Dev/nexus-lab/terraform/modules/network/
DC Module:               /mnt/Dev/nexus-lab/terraform/modules/domain-controller/
Member Server Module:    /mnt/Dev/nexus-lab/terraform/modules/member-server/
Infrastructure Project:  /mnt/Dev/nexus-lab/terraform/projects/infrastructure/
ADSync Project:          /mnt/Dev/nexus-lab/terraform/projects/adsync/
Documentation:           /mnt/Dev/nexus-lab/docs/
Project Plans:           /mnt/Dev/nexus-lab/docs/project-plans/
VM Storage:              /mnt/Dev/vm-storage/
```

### Next Immediate Steps
1. Create network module structure
2. Write network module Terraform code
3. Test network module
4. Document network module

---

**Document Version:** 3.0  
**Last Updated:** 17 October 2025  
**Next Review:** On completion of Stage 1.2
