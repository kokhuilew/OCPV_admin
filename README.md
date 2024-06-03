# OpenShift Virtualization Day2 Administration
Quick Start Administrative Guide




# Table of Contents

1. VM Management
   - VM's Default CPU Model
   - Physical CPU to Virtual Ratio For VM
   - Dedicated resources for VM
   - Descheduler evictions for VM (DRS for VMs)
     
3. Node Management
   - Reserved Node Resources Automatically
   - Reserved Node Resources Manually
   - Node routing enabled
   - Gracefully Reboot Node
   - Gracefully Shut Down Node
   - Delete Node
   - Deleted Node Rejoin Cluster
     
5. Storage Management
   - Default StorageClass
   - ODF StorageSystem Degraded
   - HPE CSI Driver Diagnostics
   - HPE CSI Change Username/Password
     
7. Monitoring Management
8. Backup and Restore Management
9. Disaster Recovery Management
10. Support Management
    - OCP-V lodge case preparation
    - ODF lodge case preparation

# About this document
## Disclaimer

* This guide intends to provide non-kubernetes administrators with quick start administrative guide on OpenShift Virtualization.
* This guide will complement OpenShift documentation with quick access for particular administrative scenario.
* This guide isn’t used to replace any Red Hat knowledgebase or  OpenShift documentation.
* This guide doesn’t provide OpenShift day0 or features installation or configuration.
## Purpose of this document

# Day 2 operation content
what's day 2 operation?
what does admin should care in day 2 operation

# Basic concept and definition of Openshift virtualization

# OpenShift Virtualization Deployment Architecture
This quick start administrative guide is based on 3 compact nodes OpenShift architecture as shown below. The administrative guidance shall be practical to all OpenShift deployment architecture
<img width="833" alt="Screenshot 2024-06-03 at 2 34 37 PM" src="https://github.com/kokhuilew/OCPV_admin/assets/4863828/abddb47f-1473-4edb-a25b-8b457cbe317a">

# VM Management
Full lifecycle mangement from creation, running, upgading to deleteing 
## VM creation
### Import VM from existing 

### Create VM from template
How to standard enterprise VM

## VM monitoring
the content could put in level one section of Monitering
## Delete/Destroy VM

# Monitoring 
monitor the health of your cluster and virtual machines (VMs)

# Node Manangement
What will the VM be impacted when node enter maintaince (upgrade/reboot/shutdown)?
Use **Eviction strategies** control the VM migration

# Storage Management
How to monitor and resize the storage capactiy and performance 
- ODF
- HPE
- DELL
- the others 
# Network
what belong to day 2 ? Need identify the reqyuirement ?
# Live migration , Backup restore and DR related topic

# Openshift virtualization upgrade

# Trouble shooting
How to collection related information to analyze the root cause 