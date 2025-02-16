# Ansible-Backup
![Project Logo](assets/red-hat-ansible-vector-logo.png)



## Table of Contents
- [Introduction](#introduction)
- [Topology](#topology)
- [Steps](#steps)
- [Usage](#usage)
- [Configuration](#configuration)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

## Introduction
I'm developing an Ansible-based backup automation project for firewalls (Fortinet, Palo Alto, F5) and network devices (Cisco switches and routers). The project includes a web interface to manage and monitor the automation process. Initially, I'm using PNetLab for simulation, with plans to transition to real-world scenarios for deployment and testing.


## Topology 
![Project Logo](assets/pnetlogo.png)


## Steps

### 🔹 **Phase 1: Environment Setup**  
- [ ] Set up **PNetLab** with firewall and network device images  
- [ ] Install **Ansible** and required dependencies  
- [ ] Configure SSH & API access for devices  
- [ ] Define Ansible inventory for network devices  

### 🔹 **Phase 2: Backup Automation**  
- [ ] Create Ansible playbooks for:  
  - [ ] **Fortinet** backup  
  - [ ] **Palo Alto** backup  
  - [ ] **Cisco switches/routers** backup
  - [ ] **F5** backup  
- [ ] Implement a  FTP  solution  
- [ ] Schedule automated backups

### 🔹 **Phase 3: Web Interface Development**  
- [ ] Design a **dashboard** for backup management  

### 🔹 **Phase 4: Testing & Real-World Deployment**  
- [ ] Validate automation in **PNetLab**  
- [ ] Deploy to real devices in a **test environment**


## Device IP Address Table  

| Device Type   | Vendor     | Hostname     | IP Address     |
|--------------|-----------|-------------|--------------|
| Firewall     | Fortinet   | FW1  | 192.168.11.66  |
| Firewall     | Palo Alto  | PA-VM-1      | 192.168.11.160  |
| Firewall     | F5         | F5-LTM-1     |   |
| Switch       | Cisco      | SW-Core-1    | 192.168.11.28 |
| Router       | Cisco      | R1           | 192.168.11.25 |
| Router       | Cisco      | R2           | 192.168.11.26 |
| Server      | Ubuntu      | Control node           | 192.168.11.100 |
| Server      | Ubuntu      |  SFTP         |  |






