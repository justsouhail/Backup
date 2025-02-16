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


## Hosts inventory 
 

### **📂** Playbook Location: `playbooks/hosts`**  

```ini

[routers]
R1 ansible_host=192.168.11.25
R2 ansible_host=192.168.11.26
[routers:vars]
ansible_user=souhail
ansible_password=123
ansible_connection=network_cli
ansible_network_os=ios


[switches]
SW2  ansible_host=192.168.11.28
[switches:vars]
ansible_user=souhail
ansible_password=123
ansible_connection=network_cli
ansible_network_os=ios





[fortigates]
fgt ansible_host=192.168.11.66 ansible_user=admin ansible_password=ertdfgcvb

[fortigates:vars]
ansible_network_os=fortinet.fortios.fortios
[all:vars]

ansible_connection=httpapi

ansible_httpapi_validate_certs=no

ansible_httpapi_use_ssl=no





[dell]
DEll1 ansible_host=192.168.11.152 ansible_net_os_name=dellos10

[dell:vars]
ansible_connection= ansible.netcommon.network_cli
ansible_network_os= dellemc.os10.os101
ansible_user= admin
ansible_password= ertdfgcvb
ansible_become= true
ansible_become_method= enable
ansible_become_password= !vault...

[paloAltos]
palo ansible_host=192.168.11.160

[paloAltos:vars]
ansible_connection=local
os=panos


```

##  Cisco Routers / switches
### Router model
![Project Logo](assets/routers.png)
### switch model
![Project Logo](assets/switches.png)



### **📂** Playbook Location: `playbooks/cisco.yml`**  
```yaml
---
- name: General Config for Routers
  hosts: routers
  gather_facts: true  
  vars:
    hostname: "{{ inventory_hostname }}"
  tasks:
    # - name: Add Banner
    #   cisco.ios.ios_banner:
    #     banner: login
    #     text: |
    #       Wasssssssssssuuuuuuuuuup
    #     state: present

    - name: Get timestamp
      command: date +%Y-%m-%d
      register: timestamp

    - name: Configurable backup path
      cisco.ios.ios_config:
        src: ios_template.j2
        backup: true
        backup_options:
          filename: "{{ inventory_hostname }}_{{ timestamp.stdout }}.cfg"
          dir_path: "/etc/ansible/cisco_folder/RT"

- name: General Config for Switches
  hosts: switches
  gather_facts: yes 
  vars:
    hostname: "{{ inventory_hostname }}"
  tasks:
    # - name: Add Banner
    #   cisco.ios.ios_banner:
    #     banner: login
    #     text: |
    #       Wasssssssssssuuuuuuuuuup
    #     state: present
    
    - name: Get timestamp
      command: date +%Y-%m-%d
      register: timestamp

    - name: Configurable backup path
      cisco.ios.ios_config:
        src: ios_template.j2
        backup: true
        backup_options:
          filename: "{{ inventory_hostname }}_{{ timestamp.stdout }}.cfg"
          dir_path: "/etc/ansible/cisco_folder/SW"

```

##  Dell

### dell model
![Project Logo](assets/dell.png)





### **📂** Playbook Location: `playbooks/dell.yml`**  
```yaml
---
- name: Backup Dell OS10 Switch Configuration
  hosts: dell  
  gather_facts: yes 

  tasks:
    - name: Backup current switch config (dellos10)
      dellemc.os10.os10_config:
        backup: yes
        backup_options:
          dir_path: "/etc/ansible/dell_folder"
          filename: "{{ inventory_hostname }}_{{ ansible_date_time.date }}.cfg"
      register: backup_dellos10_location
      when: ansible_network_os == 'dellemc.os10.os10'

    - name: Display backup location
      debug:
        msg: "Backup stored at {{ backup_dellos10_location.backup_path }}"
      when: backup_dellos10_location.backup_path is defined

```


