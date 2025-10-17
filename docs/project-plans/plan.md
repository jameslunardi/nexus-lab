# Homelab Infrastructure Project Plan

**Project Manager:** Claude  
**Project Lead/Architect/Developer:** lunardi  
**Project Start Date:** 14 October 2025  
**Project Type:** Infrastructure Development & Portfolio Piece

---

## Executive Summary

This project will deliver a virtualised homelab infrastructure managed by Terraform, supporting the development and deployment of an Active Directory synchronisation tool. The project is divided into two main phases, with clear deliverables and success criteria for portfolio presentation.

---

## Project Objectives

### Primary Objectives
1. Build a reproducible, code-managed infrastructure using Terraform and libvirt
2. Create reusable Terraform modules following infrastructure best practices
3. Deploy a working Active Directory environment across two domains
4. Demonstrate professional-grade documentation and project management
5. Develop a portfolio-ready codebase suitable for professional presentation

### Success Criteria
- Infrastructure can be deployed and destroyed repeatably via Terraform
- All code is version-controlled and well-documented
- Both domain controllers are operational and properly configured
- Documentation is clear enough for another engineer to replicate the setup
- Code follows Terraform best practices and is modular

---

## Project Phases

### Phase 1: Foundation Infrastructure Setup
**Duration Estimate:** 2-3 weeks  
**Status:** Not Started  
**Primary Goal:** Establish the core infrastructure and deploy two domain controllers

### Phase 2: ADSync Tool Deployment
**Duration Estimate:** 1-2 weeks  
**Status:** Not Started  
**Primary Goal:** Deploy member server and prepare for application development

### Phase 3: ADSync Tool Development
**Duration Estimate:** 3-4 weeks  
**Status:** Not Started  
**Primary Goal:** Develop and test the PowerShell synchronisation service

---

## Phase 1: Foundation Infrastructure Setup

### Stage 1.1: Environment Preparation
**Objective:** Set up the development environment and tooling

#### Tasks
1. **Install Base Requirements**
   - [ ] Install qemu-full, libvirt, virt-manager packages
   - [ ] Install Terraform
   - [ ] Enable and start libvirtd service
   - [ ] Add user to libvirt group
   - [ ] Verify KVM support and permissions

2. **Obtain Required Images**
   - [ ] Download Windows Server 2022 Evaluation ISO
   - [ ] Verify ISO checksum
   - [ ] Store ISO in `/mnt/Dev/homelab-infrastructure/images/`
   - [ ] Document ISO source and version

3. **Initialise Git Repository**
   - [ ] Initialise git in homelab-infrastructure directory
   - [ ] Create .gitignore file (exclude ISOs, VM disks, .terraform/)
   - [ ] Create initial commit with folder structure
   - [ ] Document Git workflow in docs/setup/

**Deliverables:**
- Working libvirt/KVM environment
- Windows Server 2022 ISO ready for use
- Git repository initialised
- Installation documentation

**Acceptance Criteria:**
- `virsh list` executes without errors
- `terraform version` displays installed version
- virt-manager can connect to QEMU/KVM
- Git repository has initial commit

---

### Stage 1.2: Network Module Development
**Objective:** Create reusable network module for VM connectivity

#### Tasks
1. **Design Network Architecture**
   - [ ] Define network topology (subnet, DHCP range, gateway)
   - [ ] Document network design in docs/architecture/
   - [ ] Determine DNS configuration strategy
   - [ ] Plan IP address allocation scheme

2. **Create Network Module**
   - [ ] Create module directory structure
   - [ ] Write main.tf with libvirt_network resource
   - [ ] Create variables.tf for configurable parameters
   - [ ] Create outputs.tf for network ID and details
   - [ ] Write README.md documenting module usage

3. **Test Network Module**
   - [ ] Create test configuration in modules/network/examples/
   - [ ] Deploy test network
   - [ ] Verify network creation in virt-manager
   - [ ] Validate DNS and DHCP functionality
   - [ ] Clean up test deployment

**Deliverables:**
- Network module (terraform/modules/network/)
- Network architecture documentation
- Module README with usage examples
- Tested and validated module

**Acceptance Criteria:**
- Module can be deployed without errors
- Network visible in `virsh net-list`
- Module is parameterised and reusable
- Documentation is complete and accurate

---

### Stage 1.3: Domain Controller Module Development
**Objective:** Create reusable module for deploying Windows Server domain controllers

#### Tasks
1. **Design Domain Controller Module**
   - [ ] Define module interface (required/optional variables)
   - [ ] Plan disk configuration (size, format, storage pool)
   - [ ] Determine memory and CPU requirements
   - [ ] Design cloud-init or answer file approach
   - [ ] Document module design decisions

2. **Create Base VM Configuration**
   - [ ] Write libvirt_volume resource for disk creation
   - [ ] Configure libvirt_domain resource for VM
   - [ ] Set up network interface configuration
   - [ ] Configure boot order and installation media
   - [ ] Add cloud-init or answer file support (if applicable)

3. **Create Domain Controller Module**
   - [ ] Create module directory structure
   - [ ] Write main.tf with VM resources
   - [ ] Create variables.tf (hostname, domain, IPs, resources)
   - [ ] Create outputs.tf (VM ID, IP address, connection details)
   - [ ] Write README.md with module documentation

4. **Test Domain Controller Module**
   - [ ] Create test configuration
   - [ ] Deploy single test VM
   - [ ] Verify VM boots and installs correctly
   - [ ] Connect via virt-manager console
   - [ ] Document any manual installation steps required
   - [ ] Clean up test deployment

**Deliverables:**
- Domain Controller module (terraform/modules/domain-controller/)
- Module documentation
- Installation procedure documentation
- Tested module

**Acceptance Criteria:**
- VM deploys successfully via Terraform
- Windows Server installs correctly
- VM is accessible via console
- All manual steps are documented
- Module follows Terraform best practices

---

### Stage 1.4: Infrastructure Project Development
**Objective:** Deploy complete foundation infrastructure with two domain controllers

#### Tasks
1. **Design Infrastructure Architecture**
   - [ ] Define two separate AD domains (names, configuration)
   - [ ] Plan IP addressing for both DCs
   - [ ] Design DNS configuration strategy
   - [ ] Document complete architecture with diagrams
   - [ ] Review design for any issues or improvements

2. **Create Infrastructure Project**
   - [ ] Create project directory (terraform/projects/infrastructure/)
   - [ ] Write main.tf calling network and DC modules
   - [ ] Create variables.tf for project-level configuration
   - [ ] Create outputs.tf for important connection details
   - [ ] Write terraform.tfvars with actual configuration
   - [ ] Create project-specific README.md

3. **Deploy Infrastructure**
   - [ ] Run `terraform init` in infrastructure project
   - [ ] Run `terraform plan` and review
   - [ ] Run `terraform apply`
   - [ ] Verify both VMs are created and running
   - [ ] Document deployment process

4. **Configure Domain Controllers**
   - [ ] Connect to dc01 via console
   - [ ] Complete Windows Server installation
   - [ ] Configure networking (static IP, DNS)
   - [ ] Install Active Directory Domain Services role
   - [ ] Promote to domain controller (Domain 1)
   - [ ] Verify domain is operational
   - [ ] Repeat process for dc02 (Domain 2)
   - [ ] Document all configuration steps

5. **Verify Infrastructure**
   - [ ] Test DNS resolution on both DCs
   - [ ] Verify AD replication (within each domain)
   - [ ] Test network connectivity between DCs
   - [ ] Create test user accounts in each domain
   - [ ] Document verification procedures

6. **Create Runbooks**
   - [ ] Write runbook for DC deployment
   - [ ] Write runbook for DC configuration
   - [ ] Write runbook for common troubleshooting
   - [ ] Write runbook for destroying and recreating infrastructure

**Deliverables:**
- Infrastructure project code (terraform/projects/infrastructure/)
- Complete architecture documentation with diagrams
- Two operational domain controllers
- Runbooks for deployment and operations
- Verification test results

**Acceptance Criteria:**
- Both domain controllers are operational
- Each DC serves its own AD domain
- Network connectivity between domains is working
- All configuration steps are documented
- Infrastructure can be destroyed and recreated
- Runbooks are complete and tested

---

### Stage 1.5: Documentation and Git Workflow
**Objective:** Complete Phase 1 documentation and establish Git workflow

#### Tasks
1. **Complete Documentation**
   - [ ] Write comprehensive setup guide (docs/setup/)
   - [ ] Create architecture diagrams (docs/architecture/)
   - [ ] Finalise all runbooks (docs/runbooks/)
   - [ ] Update README.md with Phase 1 results
   - [ ] Review all documentation for accuracy

2. **Code Review and Cleanup**
   - [ ] Review all Terraform code for best practices
   - [ ] Ensure consistent formatting (`terraform fmt`)
   - [ ] Validate all configurations (`terraform validate`)
   - [ ] Remove any test or temporary files
   - [ ] Update all module READMEs

3. **Git Repository Management**
   - [ ] Commit all Phase 1 code and documentation
   - [ ] Tag Phase 1 completion (v1.0-phase1)
   - [ ] Write detailed commit messages
   - [ ] Review .gitignore for completeness
   - [ ] Push to remote repository (if applicable)

4. **Phase 1 Review**
   - [ ] Test complete deployment from scratch
   - [ ] Verify all documentation is accurate
   - [ ] Ensure all acceptance criteria are met
   - [ ] Document lessons learned
   - [ ] Update project plan with actual timelines

**Deliverables:**
- Complete Phase 1 documentation
- Clean, committed Git repository
- Lessons learned document
- Phase 1 completion report

**Acceptance Criteria:**
- All documentation is complete and accurate
- Code is properly formatted and validated
- Git history is clean and well-organised
- Phase 1 can be deployed following documentation alone
- All stage 1.1-1.4 acceptance criteria remain met

---

## Phase 2: ADSync Tool Infrastructure

### Stage 2.1: Member Server Module Development
**Objective:** Create reusable module for Windows member servers

#### Tasks
1. **Design Member Server Module**
   - [ ] Define module interface and variables
   - [ ] Determine differences from DC module
   - [ ] Plan domain join configuration
   - [ ] Document module design

2. **Create Member Server Module**
   - [ ] Create module structure
   - [ ] Write Terraform configuration
   - [ ] Create module documentation
   - [ ] Test module independently

**Deliverables:**
- Member server module (terraform/modules/member-server/)
- Module documentation
- Test results

**Acceptance Criteria:**
- Module deploys Windows Server successfully
- Module is parameterised and reusable
- Documentation is complete

---

### Stage 2.2: ADSync Project Development
**Objective:** Deploy member server for ADSync tool

#### Tasks
1. **Create ADSync Project**
   - [ ] Create project directory
   - [ ] Write Terraform configuration
   - [ ] Document project architecture
   - [ ] Deploy via Terraform

2. **Configure ADSync Server**
   - [ ] Install Windows Server
   - [ ] Join to Domain 1
   - [ ] Configure network connectivity to Domain 2
   - [ ] Install prerequisites for PowerShell service
   - [ ] Document configuration steps

3. **Create Runbooks**
   - [ ] Deployment runbook
   - [ ] Configuration runbook
   - [ ] Troubleshooting guide

**Deliverables:**
- ADSync project code (terraform/projects/adsync/)
- Configured member server
- Complete documentation
- Runbooks

**Acceptance Criteria:**
- Member server is operational
- Server has connectivity to both domains
- All configuration is documented
- Infrastructure can be recreated from documentation

---

### Stage 2.3: Phase 2 Documentation and Completion
**Objective:** Complete Phase 2 documentation and prepare for tool development

#### Tasks
1. **Complete Documentation**
   - [ ] Update architecture documentation
   - [ ] Complete all runbooks
   - [ ] Update README.md
   - [ ] Document Phase 2 completion

2. **Git Repository Management**
   - [ ] Commit Phase 2 code
   - [ ] Tag Phase 2 completion (v1.0-phase2)
   - [ ] Review and cleanup

**Deliverables:**
- Complete Phase 2 documentation
- Git tag for Phase 2
- Infrastructure ready for tool development

**Acceptance Criteria:**
- All three VMs operational
- Complete documentation available
- Infrastructure meets all Phase 2 objectives

---

## Phase 3: ADSync Tool Development

**Note:** This phase will be detailed in a separate project plan in the adsync-tool repository. The focus will shift from infrastructure to application development.

### High-Level Overview
1. Design ADSync tool architecture
2. Develop PowerShell synchronisation service
3. Implement testing strategy
4. Create deployment automation
5. Write comprehensive documentation

---

## Project Roles and Responsibilities

### lunardi (Project Lead/Architect/Developer)
**Responsibilities:**
- Make all architectural decisions
- Write all code (Terraform and PowerShell)
- Implement infrastructure and applications
- Execute deployment and configuration tasks
- Review and approve all work
- Manage timelines and priorities

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

---

## Risk Management

### Identified Risks

| Risk | Impact | Likelihood | Mitigation |
|------|--------|-----------|------------|
| Windows Server licensing restrictions | High | Medium | Use evaluation edition (180 days) |
| KVM/libvirt compatibility issues | High | Low | Test environment early, use stable versions |
| Insufficient hardware resources | Medium | Medium | Monitor resource usage, adjust VM specs |
| Time constraints | Medium | Medium | Prioritise core functionality, document MVPs |
| Terraform state corruption | High | Low | Regular backups, use version control |
| AD configuration complexity | Medium | Medium | Research thoroughly, document extensively |
| Module design requiring refactoring | Medium | Medium | Start simple, iterate, plan for refactoring time |

---

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

---

## Communication and Status Updates

### Weekly Reviews
At the end of each week:
- Review completed tasks
- Update project plan with actual progress
- Identify blockers or issues
- Plan next week's priorities
- Update claude.md with current status

### Stage Completions
At the end of each stage:
- Verify all acceptance criteria are met
- Update project plan
- Create summary of work completed
- Document lessons learned
- Plan next stage activities

---

## Project Tracking

### Current Status
**Phase:** 1 - Foundation Infrastructure Setup  
**Stage:** 1.1 - Environment Preparation  
**Status:** Not Started  
**Blockers:** None

### Completed Milestones
- [x] Project planning and folder structure creation (14 Oct 2025)

### Upcoming Milestones
- [ ] Environment preparation complete
- [ ] Network module complete
- [ ] Domain controller module complete
- [ ] Infrastructure project complete
- [ ] Phase 1 documentation complete

---

## Notes for lunardi

### Getting Started
1. Begin with Stage 1.1: Environment Preparation
2. Work through tasks in order
3. Update this document as you complete tasks (check boxes)
4. Ask Claude for guidance when needed
5. Don't hesitate to adjust the plan as you learn

### Working with Claude
- Claude will track your progress as you report completed tasks
- Ask Claude to update this plan when changes are needed
- Request documentation templates or examples when useful
- Use Claude to review your architecture decisions
- Claude can help troubleshoot issues

### Important Reminders
- This is a portfolio piece - quality matters
- Document as you go (don't leave it to the end)
- Test everything thoroughly
- Commit code regularly
- Take breaks and don't rush

---

## Appendix

### Useful Commands Reference
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

### Directory Quick Reference
```
Infrastructure Code:     /mnt/Dev/homelab-infrastructure/
Network Module:          terraform/modules/network/
DC Module:               terraform/modules/domain-controller/
Member Server Module:    terraform/modules/member-server/
Infrastructure Project:  terraform/projects/infrastructure/
ADSync Project:          terraform/projects/adsync/
Documentation:           docs/
VM Storage:              /mnt/Dev/vm-storage/
```

---

**Document Version:** 1.0  
**Last Updated:** 14 October 2025  
**Next Review:** On completion of Stage 1.1
