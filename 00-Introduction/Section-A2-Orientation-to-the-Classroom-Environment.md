
# Section A.2: Orientation to the Classroom Environment

Welcome to my comprehensive learning journey through the **Red Hat OpenStack Administration I: Core Operations for Domain** course. This documentation captures the course materials, lab exercises, quizzes, and guided exercises, serving as an enduring resource to revisit key concepts and practical skills in OpenStack administration.

---

## Table of Contents
- [Red Hat OpenStack Administration I: Core Operations for Domain](#red-hat-openstack-administration-i-core-operations-for-domain)
  - [Table of Contents](#table-of-contents)
  - [Introduction \& Orientation](#introduction--orientation)
  - [Classroom Environment Overview](#classroom-environment-overview)
    - [Environment Layout](#environment-layout)
    - [Classroom Diagram Reference](#classroom-diagram-reference)
  - [System Credentials \& Virtual Machines](#system-credentials--virtual-machines)
    - [Virtual Machines Overview](#virtual-machines-overview)
    - [Credentials Summary](#credentials-summary)
  - [Managing OpenStack Overclouds](#managing-openstack-overclouds)
    - [Key Considerations](#key-considerations)
  - [Controlling Your Systems](#controlling-your-systems)
    - [VM States \& Actions](#vm-states--actions)
      - [**VM States**](#vm-states)
      - [**Actions**](#actions)
  - [Resetting Your Classroom Environment](#resetting-your-classroom-environment)
    - [Reset Procedures](#reset-procedures)
  - [Practical Exercises \& Lab Walkthroughs 🚀](#practical-exercises--lab-walkthroughs-)
    - [Sample Lab Exercise: Launching an Instance](#sample-lab-exercise-launching-an-instance)
    - [Additional Exercises](#additional-exercises)
  - [Key Insights \& Best Practices 💡](#key-insights--best-practices-)
    - [Core Insights](#core-insights)
    - [Best Practices](#best-practices)
  - [References \& Additional Links](#references--additional-links)
    - [OpenStack Platform](#openstack-platform)
    - [Networking](#networking)
    - [Storage](#storage)
    - [Lab Exercises \& Additional Tutorials](#lab-exercises--additional-tutorials)

---

## Introduction & Orientation

![image](https://github.com/user-attachments/assets/1f3180c1-42a0-49bb-9278-c7a598d7e36c)

Welcome to the journey of mastering OpenStack administration!  
In this course, we delve into the domain of managing and operating OpenStack environments. The course begins with an **orientation to the classroom environment**, ensuring that you understand the roles of different virtual machines (VMs) and how to interact with them using both graphical and command-line interfaces.

- **Objective:**  
  Understand the operational landscape of a classroom-based OpenStack deployment and learn how to leverage the provided tools effectively.
- **Focus Areas:**  
  - **Domain Operations:** Managing overcloud and undercloud nodes.
  - **Lab Exercises:** Hands-on practice using the workstation as a central access point.
  - **Guided Exercises:** Step-by-step walkthroughs on launching instances, configuring networks, and managing storage.

[Back to TOC](#table-of-contents)

---

## Classroom Environment Overview

This section provides an overview of the classroom setup used throughout the course. The environment is meticulously designed for hands-on activities and real-world simulation.

### Environment Layout
- **Primary Workstation:**  
  The only VM with a graphical desktop, required for using a browser and accessing remote dashboards and GUI tools.
- **Network Configuration:**  
  - **External Network:** `172.25.250.0/24` with a gateway at `172.25.250.254`.
  - **Internal Networks:** Overcloud VMs use multiple VLANs with `172.24.X.0/24` addressing.
- **Additional VMs:**  
  Include nodes such as `controller0`, `compute0`, `compute1`, `computehci0`, and `ceph0`—each playing a distinct role within the overcloud domain.

### Classroom Diagram Reference
A network topology diagram (e.g., Figure 0.2 in course materials) provides a visual layout of the environment. It is recommended to print or save this diagram for ongoing reference.

[Back to TOC](#table-of-contents)

---

## System Credentials & Virtual Machines

Understanding the available VMs and their credentials is key to managing the OpenStack environment. Below are summarized details from the course materials.

### Virtual Machines Overview

| Machine Name                                   | IP Addresses                                    | Role                                                           |
|------------------------------------------------|-------------------------------------------------|----------------------------------------------------------------|
| `workstation.lab.example.com`                  | `172.25.250.9`                                  | **Graphical Student Workstation**                              |
| `director.lab.example.com`                     | `172.25.250.200`, `172.25.249.200`               | Undercloud node (director) – **Leave Powered Off**             |
| `power.lab.example.com`                        | Multiple addresses (e.g., `172.25.250.100`, etc.)| Manages overcloud nodes' IPMI power management                   |
| `utility.lab.example.com`                      | `172.25.250.220`, `172.24.250.220`               | Identity management and VLAN configurations                     |
| `controller0.overcloud.example.com`            | `172.25.250.1`, `172.25.249.P`, `172.24.X.1`     | Overcloud controller node                                      |
| `compute0.overcloud.example.com`               | `172.25.250.2`, `172.25.249.R`, `172.24.X.2`     | Overcloud compute node                                         |
| `compute1.overcloud.example.com`               | `172.25.250.12`, `172.25.249.S`, `172.24.X.12`   | Overcloud compute node                                         |
| `computehci0.overcloud.example.com`            | `172.25.250.6`, `172.25.249.T`, `172.24.X.6`     | Compute node with integrated storage                           |
| `ceph0.overcloud.example.com`                  | `172.25.250.3`, `172.25.249.U`, `172.24.X.3`     | Block and object storage server node                           |
| `classroom.example.com`                        | `172.25.254.254`                                | Classroom materials and content server                         |

### Credentials Summary

| Access Type                                      | Username    | Password       |
|--------------------------------------------------|-------------|----------------|
| Unprivileged Shell Login (student)             | student     | student        |
| Privileged Shell Login                           | root        | redhat         |
| Undercloud Node (Unprivileged)                   | stack       | redhat         |
| Undercloud Node (Privileged)                     | root        | redhat         |
| Overcloud Node (Unprivileged)                    | heat-admin  | *Passwordless SSH* |
| Overcloud Node (Privileged)                      | root        | Use `sudo -i`  |

> **Note:** Application credentials are provided separately for identity and platform administration.

[Back to TOC](#table-of-contents)

---

## Managing OpenStack Overclouds

![image](https://github.com/user-attachments/assets/dccd8df4-095e-491b-ada1-ae950b9d1f07)

Managing the overcloud in a classroom setting is a distinct process from production environments. This section covers the operational practices for maintaining overcloud clusters.

### Key Considerations
- **Cluster Dynamics:**  
  The overcloud consists of multiple nodes (controller, compute, storage) that must operate in unison. Resetting a single node might disrupt cluster communications.
- **Group Resets:**  
  For exercises involving the overcloud, always reset nodes as a group to ensure all dependencies are maintained.
- **Production vs. Classroom:**  
  Classroom environments are scaled-down versions with some shortcuts; always refer to the official documentation for production procedures.

> **Warning:** Never reset critical nodes like the power node in an online environment as it can lead to loss of management credentials.

[Back to TOC](#table-of-contents)

---

## Controlling Your Systems

![image](https://github.com/user-attachments/assets/1175337f-ce44-426f-9eae-a1df3a3ce2e3)

This section provides guidance on managing the state of your VMs through the web-based control panel provided in the classroom environment.

### VM States & Actions

#### **VM States**
- **Building:** The VM is in the process of being created.
- **Active:** The VM is running and available.
- **Stopped:** The VM is shut down but retains disk data.

#### **Actions**
- **CREATE / DELETE:** Provision or remove the classroom environment.
- **START / STOP:** Power on or shut down VMs.
- **OPEN CONSOLE:** Access the VM’s console in a new browser tab for direct login and command execution.
- **ACTION → Reset:** Reset a VM to its initial state. Use with caution as disk contents will be lost.

[Back to TOC](#table-of-contents)

---

## Resetting Your Classroom Environment

Resetting your classroom environment is a key part of ensuring a clean slate for repeated lab exercises.

### Reset Procedures
- **Individual Node Reset:**  
  Use when instructed to reset a specific VM (e.g., workstation, compute node).
- **Overcloud Reset:**  
  Requires resetting all nodes in the overcloud together to avoid disrupting the cluster state.
- **Full Environment Reset:**  
  The DELETE and CREATE functions allow you to completely rebuild the classroom environment.

> **Important:** The DELETE operation is irreversible. Ensure that any critical work is saved externally before proceeding.

[Back to TOC](#table-of-contents)

---

## Practical Exercises & Lab Walkthroughs 🚀

This section details the step-by-step lab exercises and guided activities that reinforce theoretical concepts with hands-on practice.

### Sample Lab Exercise: Launching an Instance
1. **Objective:**  
   Launch a new compute instance using the dashboard.
2. **Steps:**
   - **Login:** Start at the workstation VM.
   - **Access Dashboard:** Open your browser and navigate to the RHOSP dashboard.
   - **Create Instance:** Follow the guided steps, selecting the desired image and flavor.
   - **Verification:** Use SSH from the workstation to connect to the new instance.
3. **Code Snippet:**
   ```bash
   # Launch an instance using the CLI
   openstack server create --flavor m1.small --image cirros --network private-net instance1
   ```
4. **Inline Commentary:**  
   - The command above demonstrates a typical CLI-based instance launch.
   - Ensure that network configurations match your environment's setup.

### Additional Exercises
- **Network Configuration:** Set up and test internal and external network connectivity.
- **Storage Management:** Configure and test various storage options such as persistent and ephemeral storage.
- **Automation:** Explore automating deployments using tools like Cloud-init.

[Back to TOC](#table-of-contents)

---

## Key Insights & Best Practices 💡

### Core Insights
- **Understanding the Environment:**  
  A strong grasp of your classroom’s topology and node roles is crucial.
- **Group Operations:**  
  Always perform overcloud operations as a group to maintain consistency.
- **Troubleshooting:**  
  Familiarize yourself with common issues such as misconfigured network settings or reset errors, and refer back to your lab exercises for remediation steps.

### Best Practices
- **Consistent Monitoring:**  
  Keep an eye on VM states and network performance.
- **Documentation:**  
  Regularly update your notes and configuration samples for future reference.
- **Official Resources:**  
  Always cross-check with Red Hat’s official documentation when in doubt.

[Back to TOC](#table-of-contents)

---

## References & Additional Links

### OpenStack Platform
- [Red Hat OpenStack Documentation](https://access.redhat.com/documentation/en-us/red_hat_openstack_platform)

### Networking
- [OpenStack Networking Guide](https://docs.openstack.org/neutron/latest/)

### Storage
- [OpenStack Storage Solutions](https://docs.openstack.org/cinder/latest/)

### Lab Exercises & Additional Tutorials
- [Red Hat Customer Portal – OpenStack Labs](https://access.redhat.com/documentation/en-us/red_hat_openstack_platform/)
- [Community Tutorials & Forums](https://www.openstack.org/community/)

