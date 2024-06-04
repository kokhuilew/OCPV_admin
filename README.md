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
* This document is based on Openshift 4.15
## Purpose of this document
This document serves as a comprehensive guide for IT administrators on the Day 2 operations of OpenShift virtualization. It aims to provide detailed insights, instructions, and best practices for managing OpenShift virtualized environments post-deployment.

The primary purpose of this document is to empower IT administrators with the knowledge and tools necessary to effectively operate and maintain OpenShift virtualization infrastructure. By understanding the Day 2 operations, administrators can ensure the smooth functioning, performance optimization, and scalability of their OpenShift virtualized environments.
# Day 2 operation content
Comparing day 0 planning, day 1 deploying, yes, day-2 is the stuff that happens after deployment. The fun stuff is over. Celebrations over a 'successful delivery' are behind you. It's time to embrace reality for a while, at least until you start all over again.

**Day-2 operations tasks include:**
- Manage the VM lifecycle
- Monitoring VM status to prevent and respond to problems
- Monitoring the computing, memory, network and storage performance and capacity 
- Performing routine maintenance like backups and restores
- Restoring service if infrastructure fails, through spinning up new infrastructure or doing rollbacks
- Patch and upgrade the VM
- Patch and upgrade the infrastructure node, storage and etc.
You can sum up day-2 operations as 'business as usual' operations tasks. It's the phase that keeps your product ticking over so your end user can enjoy it when they need it.

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
To monitor virtual machines (VMs) in OpenShift Virtualization, you can use various tools and techniques to gather metrics, logs, and other performance indicators. Here's a general guide on how to monitor VMs in OpenShift Virtualization:

- OpenShift Monitoring Stack: OpenShift provides a built-in monitoring stack based on Prometheus, Grafana, and Kubernetes-native monitoring components. You can use this stack to monitor various metrics related to your VMs, such as CPU usage, memory usage, disk I/O, and network traffic. You can deploy the monitoring stack using the OperatorHub in the OpenShift console.

- Prometheus: Prometheus is a powerful monitoring tool commonly used in Kubernetes environments. It can scrape metrics from various endpoints exposed by your VMs and store them for querying and visualization. You can configure Prometheus to scrape metrics from your VMs using service discovery mechanisms or static configuration.

- Grafana: Grafana is a visualization tool that works well with Prometheus. You can use Grafana to create dashboards that display metrics collected by Prometheus. Grafana provides various visualization options, such as graphs, gauges, and tables, allowing you to monitor the performance of your VMs effectively.

- Node Exporter: Node Exporter is a Prometheus exporter that collects system-level metrics from Linux hosts. You can deploy Node Exporter on each node in your OpenShift cluster to collect metrics such as CPU, memory, disk, and network usage from your VMs.

- Kube-state-metrics: Kube-state-metrics is another Prometheus exporter that exposes various Kubernetes resource metrics. You can use it to monitor the state of your VMs, such as the number of running VMs, their status, and other relevant metadata.

- Logging: In addition to metrics, monitoring logs generated by your VMs can provide valuable insights into their behavior and performance. You can use tools like Elasticsearch, Fluentd, and Kibana (EFK stack) or Loki and Grafana (Promtail) to collect, store, and visualize logs generated by your VMs.

- Alerting: Set up alerts based on predefined thresholds or conditions to proactively identify and address issues with your VMs. You can configure alerting rules in Prometheus and send alerts to various notification channels, such as email, Slack, or PagerDuty.

- Custom Metrics: Depending on your specific requirements, you may need to collect custom metrics from your VMs. You can instrument your applications running inside the VMs to expose custom metrics and then scrape them using Prometheus.

By leveraging these monitoring tools and techniques, you can effectively monitor your virtual machines in OpenShift Virtualization and ensure their optimal performance and availability.

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
A checkup is an automated test workload that allows you to verify if a specific cluster functionality works as expected. The cluster checkup framework uses native Kubernetes resources to configure and execute the checkup.

By using predefined checkups, cluster administrators and developers can improve cluster maintainability, troubleshoot unexpected behavior, minimize errors, and save time. They can also review the results of the checkup and share them with experts for further analysis. Vendors can write and publish checkups for features or services that they provide and verify that their customer environments are configured correctly.

## Running a latency checkup on the CLI (Technical Preview feature)
You use a predefined checkup to verify network connectivity and measure latency between two virtual machines (VMs) that are attached to a secondary network interface. The latency checkup uses the ping utility.

You run a latency checkup by performing the following steps:

1. Create a service account, roles, and rolebindings to provide cluster access permissions to the latency checkup.

2. Create a config map to provide the input to run the checkup and to store the results.

3. Create a job to run the checkup.

4. Review the results in the config map.

5. Optional: To rerun the checkup, delete the existing config map and job and then create a new config map and job.

6. When you are finished, delete the latency checkup resources.

Prerequisites
- You installed the OpenShift CLI (oc).

- The cluster has at least two worker nodes.

- You configured a network attachment definition for a namespace.

Procedure
1. Create a ServiceAccount, Role, and RoleBinding manifest for the latency checkup:

Collapse all
Example role manifest file

```yaml
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: vm-latency-checkup-sa
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: kubevirt-vm-latency-checker
rules:
- apiGroups: ["kubevirt.io"]
  resources: ["virtualmachineinstances"]
  verbs: ["get", "create", "delete"]
- apiGroups: ["subresources.kubevirt.io"]
  resources: ["virtualmachineinstances/console"]
  verbs: ["get"]
- apiGroups: ["k8s.cni.cncf.io"]
  resources: ["network-attachment-definitions"]
  verbs: ["get"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: kubevirt-vm-latency-checker
subjects:
- kind: ServiceAccount
  name: vm-latency-checkup-sa
roleRef:
  kind: Role
  name: kubevirt-vm-latency-checker
  apiGroup: rbac.authorization.k8s.io
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: kiagnose-configmap-access
rules:
- apiGroups: [ "" ]
  resources: [ "configmaps" ]
  verbs: ["get", "update"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: kiagnose-configmap-access
subjects:
- kind: ServiceAccount
  name: vm-latency-checkup-sa
roleRef:
  kind: Role
  name: kiagnose-configmap-access
  apiGroup: rbac.authorization.k8s.io
```
Apply the ServiceAccount, Role, and RoleBinding manifest:


```
$ oc apply -n <target_namespace> -f <latency_sa_roles_rolebinding>.yaml 
```

<target_namespace> is the namespace where the checkup is to be run. This must be an existing namespace where the NetworkAttachmentDefinition object resides.

3. Create a ConfigMap manifest that contains the input parameters for the checkup:

Example input config map

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: kubevirt-vm-latency-checkup-config
  labels:
    kiagnose/checkup-type: kubevirt-vm-latency
data:
  spec.timeout: 5m
  spec.param.networkAttachmentDefinitionNamespace: <target_namespace>
  spec.param.networkAttachmentDefinitionName: "blue-network" 
  spec.param.maxDesiredLatencyMilliseconds: "10" 
  spec.param.sampleDurationSeconds: "5" 
  spec.param.sourceNode: "worker1" 
  spec.param.targetNode: "worker2" 
```
The name of the NetworkAttachmentDefinition object.
- Optional: The maximum desired latency, in milliseconds, between the virtual machines. If the measured latency exceeds this value, the checkup fails.
- Optional: The duration of the latency check, in seconds.
- Optional: When specified, latency is measured from this node to the target node. If the source node is specified, the spec.param.targetNode field cannot be empty.
- Optional: When specified, latency is measured from the source node to this node.

4. Apply the config map manifest in the target namespace:

```shell
$ oc apply -n <target_namespace> -f <latency_config_map>.yaml
```
5. Create a Job manifest to run the checkup:

Example job manifest
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: kubevirt-vm-latency-checkup
  labels:
    kiagnose/checkup-type: kubevirt-vm-latency
spec:
  backoffLimit: 0
  template:
    spec:
      serviceAccountName: vm-latency-checkup-sa
      restartPolicy: Never
      containers:
        - name: vm-latency-checkup
          image: registry.redhat.io/container-native-virtualization/vm-network-latency-checkup-rhel9:v4.15.0
          securityContext:
            allowPrivilegeEscalation: false
            capabilities:
              drop: ["ALL"]
            runAsNonRoot: true
            seccompProfile:
              type: "RuntimeDefault"
          env:
            - name: CONFIGMAP_NAMESPACE
              value: <target_namespace>
            - name: CONFIGMAP_NAME
              value: kubevirt-vm-latency-checkup-config
            - name: POD_UID
              valueFrom:
                fieldRef:
                  fieldPath: metadata.uid
```
6. Apply the Job manifest:

```shell
$ oc apply -n <target_namespace> -f <latency_job>.yaml
```
7. Wait for the job to complete:

```
$ oc wait job kubevirt-vm-latency-checkup -n <target_namespace> --for condition=complete --timeout 6m
```
8. Review the results of the latency checkup by running the following command. If the maximum measured latency is greater than the value of the `spec.param.maxDesiredLatencyMilliseconds` attribute, the checkup fails and returns an error.

```
$ oc get configmap kubevirt-vm-latency-checkup-config -n <target_namespace> -o yaml
```
Example output config map (success)

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: kubevirt-vm-latency-checkup-config
  namespace: <target_namespace>
  labels:
    kiagnose/checkup-type: kubevirt-vm-latency
data:
  spec.timeout: 5m
  spec.param.networkAttachmentDefinitionNamespace: <target_namespace>
  spec.param.networkAttachmentDefinitionName: "blue-network"
  spec.param.maxDesiredLatencyMilliseconds: "10"
  spec.param.sampleDurationSeconds: "5"
  spec.param.sourceNode: "worker1"
  spec.param.targetNode: "worker2"
  status.succeeded: "true"
  status.failureReason: ""
  status.completionTimestamp: "2022-01-01T09:00:00Z"
  status.startTimestamp: "2022-01-01T09:00:07Z"
  status.result.avgLatencyNanoSec: "177000"
  status.result.maxLatencyNanoSec: "244000" 
  status.result.measurementDurationSec: "5"
  status.result.minLatencyNanoSec: "135000"
  status.result.sourceNode: "worker1"
  status.result.targetNode: "worker2"
```
The maximum measured latency in nanoseconds.

9. Optional: To view the detailed job log in case of checkup failure, use the following command:
```
$ oc logs job.batch/kubevirt-vm-latency-checkup -n <target_namespace>
```
10. Delete the job and config map that you previously created by running the following commands:

```shell
$ oc delete job -n <target_namespace> kubevirt-vm-latency-checkup

$ oc delete config-map -n <target_namespace> kubevirt-vm-latency-checkup-config
```

11. Optional: If you do not plan to run another checkup, delete the roles manifest:

```
$ oc delete -f <latency_sa_roles_rolebinding>.yaml
```

# Live migration , Backup restore and DR related topic

# Openshift virtualization upgrade

# Trouble shooting
How to collection related information to analyze the root cause 