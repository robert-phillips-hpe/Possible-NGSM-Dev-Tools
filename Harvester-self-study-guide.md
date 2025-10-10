# Harvester Technical Course Curriculum

## Course Overview
Harvester is a modern, open-source hyperconverged infrastructure (HCI) platform built on Kubernetes. This course is designed to provide a comprehensive understanding of Harvester, including installation, configuration, and usage, as well as troubleshooting and advanced topics for deploying and managing virtual machines and storage in a Kubernetes ecosystem.

This self-study course is designed for IT professionals, DevOps engineers, and system administrators who want to use Harvester effectively in their infrastructure.

---

## Course Outline

### Module 1: Introduction to Harvester
- What is Harvester?
- Key features and benefits of Harvester
- Use cases for Harvester

### Module 2: Prerequisites and Environment Setup
- System requirements for Harvester
- Installing Harvester on bare-metal or virtualized environments

### Module 3: Harvester Fundamentals
- Understanding Harvester architecture
- Kubernetes integration
- Core components (VMs, storage, networks)

### Module 4: Managing Virtual Machines
- Creating and configuring virtual machines
- Attaching storage to VMs
- Networking and IP management

### Module 5: Storage Management
- Overview of Longhorn (Harvester's storage solution)
- Storage provisioning and management
- Backups and disaster recovery

### Module 6: Monitoring and Troubleshooting
- Using Harvester's integrated monitoring tools
- Debugging and resolving common issues

### Module 7: Advanced Topics
- Customizing Harvester configurations
- Cluster scaling and upgrades
- Integration with third-party tools

---

## Prerequisites and Required Skills

To succeed in this course, you should have the following prerequisites and skills:

1. **Basic Knowledge of Kubernetes**
   - Understanding Kubernetes concepts such as nodes, pods, services, and deployments.
   - Sources:
     - [Kubernetes Official Documentation](https://kubernetes.io/docs/)
     - [Learn Kubernetes Basics](https://kubernetes.io/docs/tutorials/kubernetes-basics/)
  
2. **Familiarity with Linux Operating Systems**
   - Command-line experience with Linux distributions such as Ubuntu or CentOS.
   - Sources:
     - [Linux Command Line Basics on Codecademy](https://www.codecademy.com/learn/learn-the-command-line)
     - [Linux Command Line Tutorial for Beginners](https://linuxjourney.com/)

3. **Understanding of Virtualization Concepts**
   - Knowledge of virtual machines, hypervisors, and virtualization tools.
   - Sources:
     - [Introduction to Virtualization by VMWare](https://www.vmware.com/topics/glossary/content/virtualization.html)
     - [Red Hat Virtualization Basics](https://www.redhat.com/en/topics/virtualization)

4. **Networking Basics**
   - Familiarity with IP addressing, routing, and DNS.
   - Sources:
     - [Linux Networking Tutorials](https://www.baeldung.com/linux/networking-series)
     - [Network Training on Percipio](https://hpe.percipio.com/search?q=linux%20networking) _HPE Login Required_

---

## Detailed Module Breakdown with Exercises

### Module 1: Introduction to Harvester
**Timeframe:** 2 hours  
**Topics:**
- Definition and purpose of Harvester
- Comparison with other HCI solutions
- Key features and benefits

**Exercises:**
1. Research and compare Harvester with VMware vSphere or Nutanix.
2. Write a short essay on how Harvester fits into modern infrastructure.

**Questions to Test Understanding:**
- What are the primary use cases for Harvester?
- How does Harvester differ from traditional HCI platforms?

**Sources:**
- [Harvester Official Website](https://docs.harvesterhci.io/v1.6/)
- [What is hyperconverged infrastructure?](https://www.ibm.com/think/topics/hyperconverged-infrastructure)
- [Harvester video introduction to dashboard](https://youtu.be/Ngsk7m6NYf4?si=52SzRH9tFxqBZXeI)

---

### Module 2: Prerequisites and Environment Setup
**Timeframe:** 3 hours  
**Topics:**
- Hardware and software requirements
- Installing Harvester on bare-metal
- Setting up a virtualized environment for Harvester

**Exercises:**
1. Install Harvester on a test system following the documentation.
2. Explore Harvester's web UI and CLI.

**Questions to Test Understanding:**
- What are the system requirements for Harvester installation?
- What are the differences between installing Harvester on bare-metal vs. virtualized environments?

**Sources:**
- [Harvester ISO Installation Guide](https://docs.harvesterhci.io/v1.6/install/index)
- [USB Installation Overview](https://docs.harvesterhci.io/v1.1/install/usb-install)

---

### Module 3: Harvester Fundamentals
**Timeframe:** 4 hours  
**Topics:**
- Harvester architecture overview
- Kubernetes integration
- Core Harvester components

**Exercises:**
1. Create a diagram of Harvester's architecture.
2. Identify how Harvester integrates with Kubernetes.

**Questions to Test Understanding:**
- What are the core components of Harvester?
- How does Harvester utilize Kubernetes?

**Sources:**
- [Overview of Harvester](https://systemadminspro.com/overview-of-harvester-a-hyper-converged-open-source-solution-powered-by-kubernetes/)
- [But What is Harvester?](https://jmcglock.substack.com/p/but-what-is-harvester)

---

### Module 4: Managing Virtual Machines
**Timeframe:** 5 hours  
**Topics:**
- Creating and configuring VMs
- VM storage and networking

**Exercises:**
1. Create a VM on Harvester and attach storage.
2. Configure networking for the VM and assign an IP.

**Questions to Test Understanding:**
- How do you create and configure a VM in Harvester?
- What are the options for VM storage?

**Sources:**
- [Virtual Machines](https://docs.harvesterhci.io/v1.6/vm/virtual-machines)
- [Fundamentals of Virtual Networking](https://www.geeksforgeeks.org/computer-networks/fundamentals-of-virtual-networking/)

---

### Module 5: Storage Management
**Timeframe:** 4 hours  
**Topics:**
- Longhorn storage overview
- Storage provisioning
- Backup strategies

**Exercises:**
1. Create storage volumes in Harvester using Longhorn.
2. Perform a backup and restore operation.

**Questions to Test Understanding:**
- What makes Longhorn an effective storage solution for Harvester?
- How do you perform backups in Harvester?

**Sources:**
- [Longhorn Documentation](https://longhorn.io/docs/)
- [Harvester StorageClass](https://docs.harvesterhci.io/v1.6/advanced/storageclass/)

---

### Module 6: Monitoring and Troubleshooting
**Timeframe:** 3 hours  
**Topics:**
- Integrated monitoring tools
- Debugging techniques

**Exercises:**
1. Monitor system performance using Harvester's tools.
2. Troubleshoot a simulated issue in Harvester.

**Questions to Test Understanding:**
- What monitoring tools are available in Harvester?
- How do you troubleshoot common issues?

**Sources:**
- [Monitoring Guide for Kubernetes](https://prometheus.io/docs/introduction/overview/)
- [Debugging Kubernetes Applications](https://kubernetes.io/docs/tasks/debug/)

---

### Module 7: Advanced Topics
**Timeframe:** 4 hours  
**Topics:**
- Customizing Harvester
- Scaling and upgrading clusters
- Integrating third-party tools

**Exercises:**
1. Customize Harvester configurations for your environment.
2. Scale up a Harvester cluster and perform an upgrade.

**Questions to Test Understanding:**
- How do you scale and upgrade Harvester clusters?
- What third-party tools can you integrate with Harvester?

**Sources:**
- [Deep Dive into Kubernetes (K8s), Rancher, and Harvester](https://medium.com/@bayounm95.eng/deep-dive-into-kubernetes-k8s-rancher-and-harvester-03b82e786c1a)
- [Third-party Integrations for Kubernetes](https://helm.sh/docs/)
- [Upgrading Harvester](https://docs.harvesterhci.io/v1.6/upgrade/index)

---

## Total Timeframe
**Estimated Completion Time:** 25 hours  
- 1 week at 5 hours per day (self-study pace)

---

## Final Assessment
- Case study: Deploy Harvester in a simulated environment and document the process.
- Multiple-choice quiz covering all modules.
- Troubleshoot a pre-designed issue in a Harvester setup.

---

## Additional Resources
1. [Harvester GitHub Repository](https://github.com/harvester/harvester)
2. [Percipio Kubernetes Training](https://hpe.percipio.com/search?q=kubernetes) _Requires HPE Login_
   

### List of URLs used in this Study Guide

1. [Kubernetes Official Documentation](https://kubernetes.io/docs/)
2. [Learn Kubernetes Basics](https://kubernetes.io/docs/tutorials/kubernetes-basics/)
3. [Linux Command Line Basics on Codecademy](https://www.codecademy.com/learn/learn-the-command-line)
4. [Linux Command Line Tutorial for Beginners](https://linuxjourney.com/)
5. [Introduction to Virtualization by VMWare](https://www.vmware.com/topics/glossary/content/virtualization.html)
6. [Red Hat Virtualization Basics](https://www.redhat.com/en/topics/virtualization)
7. [Linux Networking Tutorials](https://www.baeldung.com/linux/networking-series)
8. [Network Training on Percipio](https://hpe.percipio.com/search?q=linux%20networking) _HPE Login Required_
9. [Harvester Official Website](https://docs.harvesterhci.io/v1.6/)
10. [What is hyperconverged infrastructure?](https://www.ibm.com/think/topics/hyperconverged-infrastructure)
11. [Harvester video introduction to dashboard](https://youtu.be/Ngsk7m6NYf4?si=52SzRH9tFxqBZXeI)
12. [Harvester ISO Installation Guide](https://docs.harvesterhci.io/v1.6/install/index)
13. [USB Installation Overview](https://docs.harvesterhci.io/v1.1/install/usb-install)
14. [Overview of Harvester](https://systemadminspro.com/overview-of-harvester-a-hyper-converged-open-source-solution-powered-by-kubernetes/)
15. [But What is Harvester?](https://jmcglock.substack.com/p/but-what-is-harvester)
16. [Virtual Machines](https://docs.harvesterhci.io/v1.6/vm/virtual-machines)
17. [Fundamentals of Virtual Networking](https://www.geeksforgeeks.org/computer-networks/fundamentals-of-virtual-networking/)
18. [Longhorn Documentation](https://longhorn.io/docs/)
19. [Harvester StorageClass](https://docs.harvesterhci.io/v1.6/advanced/storageclass/)
20. [Monitoring Guide for Kubernetes](https://prometheus.io/docs/introduction/overview/)
21. [Debugging Kubernetes Applications](https://kubernetes.io/docs/tasks/debug/)
22. [Deep Dive into Kubernetes (K8s), Rancher, and Harvester](https://medium.com/@bayounm95.eng/deep-dive-into-kubernetes-k8s-rancher-and-harvester-03b82e786c1a)
23. [Third-party Integrations for Kubernetes](https://helm.sh/docs/)
24. [Upgrading Harvester](https://docs.harvesterhci.io/v1.6/upgrade/index)
25. [Harvester GitHub Repository](https://github.com/harvester/harvester)
26. [Percipio Kubernetes Training](https://hpe.percipio.com/search?q=kubernetes) _Requires HPE Login_
   
---
