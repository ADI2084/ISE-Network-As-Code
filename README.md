# Terraform & Ansible Deep Dive: Cisco ISE + pxGrid + SGT + Catalyst Center

This repository is a **deep research + lab project** focused on using **Terraform** and **Ansible** to automate:

- Cisco ISE deployment & configuration (with pxGrid)
- Security Group Tags (SGTs) & policies
- Integration of ISE with **Cisco Catalyst Center** (formerly DNA Center)
- End-to-end workflow where Segmentation & Context (SGTs) are consistently applied across ISE and the network fabric

> ⚠️ **Note:** This repo is for lab / PoC / learning. Do **not** use it directly in production without proper validation, code review, and security hardening.

---

## 1. Objectives

1. **Infrastructure as Code (IaC)**  
   Use **Terraform** to define and version ISE, Catalyst Center, and segmentation-related objects (where supported by APIs/providers).

2. **Configuration as Code (CaC)**  
   Use **Ansible** to:
   - Enable and configure pxGrid on ISE
   - Create SGTs / SGACLs on ISE
   - Sync or map SGTs and policies with Catalyst Center
   - Provision network devices (NADs) to ISE via Catalyst Center APIs

3. **End-to-End Segmentation Story**
   - ISE as the **Policy & Identity** engine
   - Catalyst Center as the **Network Controller**
   - SGTs as the **consistent security abstraction** from user/device to the fabric

4. **Deep Research Angle**
   - Capture design decisions
   - Compare different approaches (Terraform vs Raw API vs Ansible)
   - Document limitations, gotchas, and best practices

---

## 2. Repository Layout

- `docs/`  
  Architecture, deep dives, and design notes.

- `lab-topology/`  
  Lab design, IP plan, and topology.

- `terraform/`  
  Terraform configuration and modules for ISE, SGTs, and Catalyst Center.

- `ansible/`  
  Ansible inventory, roles, and playbooks for pxGrid, SGT, and Catalyst Center integration.

- `examples/`  
  Sample variable files and runbooks for executing end-to-end workflows.

---

## 3. High-Level Architecture

**Components:**

- **Cisco ISE**
  - Policy Service Node (PSN)
  - Policy Administration Node (PAN)
  - pxGrid node(s)
- **Cisco Catalyst Center**
  - Manages fabric switches / wireless controllers
  - Consumes ISE pxGrid context & SGTs
- **Network Devices**
  - Catalyst switches (access / distribution / core)
  - Wireless LAN controllers (optional)
- **Automation Stack**
  - Terraform (infrastructure objects, some policies)
  - Ansible (API-driven config, orchestration)

See `docs/01-architecture-overview.md` and `docs/diagrams/`.

---

## 4. Prerequisites

### 4.1 Tools

- Terraform >= 1.5.x
- Ansible >= 2.15.x
- Python 3.x with:
  - `requests`
  - `ansible` + Cisco collections (see below)
- Git

### 4.2 Ansible Collections (recommended)

```bash
ansible-galaxy collection install cisco.ise
ansible-galaxy collection install cisco.dnac
ansible-galaxy collection install community.general
