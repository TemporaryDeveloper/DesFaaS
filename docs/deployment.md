# Infrastructure Config
## Python Environment

Python3.10 should be installed to support the running of DesFaaS in the master VM and worker VMs, and related dependencies should be installed. In master VM, the dependencies is in [requirements_master.txt](https://github.com/WhaleSpring/DesFaaS/blob/main/components/requirements_master.txt), while worker VMs in [requirements_worker.txt](https://github.com/WhaleSpring/DesFaaS/blob/main/components/requirements_worker.txt)

## Worker VMs

To run DesFaaS, every worker VM needs to meet such conditions:
- CRI-O is deployed as the basic container runtime. 
- The checkpoint/restore(C/R) capacity of CRI-O is active.
- CRI-O version is greater than 1.25.
- The Buildah tool is installed as an image build tool.
- Shared disk among VMs of one physical machine is configured.

## Kubernetes 

In order to use DesFaaS, the Kubernetes cluster needs to meet such conditions:
- OpenFaaS is deployed as the basic serverless runtime.
- CAdvisor, Node-exporter, Kube-metrics, and Prometheus are deployed and configured to monitor cluster information DesFaaS requires.
- The checkpoint capacity of Kubernetes is active.
- The Kubernetes version is greater than 1.25.

## External Infrastructure

Desfaas requires the following external infrastructure:
- Disk of an external server is required as state external storage.
- An external image repository is required to store function runtime images.

# DesFaaS Config

The config.py must be modified to meet the cluster config and other infrastructures on which DesFaaS relies. Here are the related global parameters.


| Global Variable Name | Meaning | Type |
|---|---|---|
|MASTRE_IP| The IP address of the master node of kubernetes|string|
|CLUSTER_CONFIG|The IP and corresponding server name of worker nodes|dict|
|CLUSTER_SETTINGS|The shared disk config about with CLUSTER_CONFIG|dict|
|NAMESPACE|The Kubernetes namespace where the stateful functions are running|string|
| PROMETHEUS | The path of Prometheus | string |
| FUNCTION_CONFIG_PATH | The path of OpenFaaS stateful function config YAML file | string |
|FUNCTION_CONFIG_PATH|the path to store the OpenFaaS YAML files|string|
|WORKER_NODES_USERNAME|The user name of worker nodes|string|
|WORKER_NODES_PASSWORD|The password of worker nodes|string|
|WORKER_NODES_PORT|The SSH connection port of worker nodes|int|
|WORKER_STATE_DISK_PATH |The shared disk path in worker nodes|string|
|WORKER_CHECKPOINT_DISK_PATH |The path to store checkpoint files in worker nodes|string|
|EXTERNAL_IMAGE_STORAGE| The URL of external image storage| string |
|EXTERNAL_STATE_SERVER_IP| The IP of state external storage server|string|  
|EXTERNAL_STATE_SERVER_USERNAME| The username of state external storage server|string|  
|EXTERNAL_STATE_SERVER_PASSWORD|The password of the state external storage server|string|  
|PORT|The SSH port of the state external storage server|int|  
|EXTERNAL_STATE_SERVER_STORAGE_PATH|The storage path of state external storage server|string|  


# DesFaaS Deployment

After completing the above configuration, DesFaaS can be used. The modified source code of DesFaaS  needs to be copied to the master VM and each worker VM, and run the components of DesFaaS on the corresponding ports in the corresponding VM:

|conponent name|port|Introduction|runnninga location|
|--|--|--|--|
|state monitor|8053|Monitor and schedule global state messages.|master VM|
|state manager|8054|Manage local state replicas.|worker VM|
|migration monitor|8055|Monitor cluster and function information.|master VM|
|migration decision-maker|/|Make migration decisions for local functions.|worker VM|
|migration scheduler|8051|Coordinate the entire process of migration decisions.|master VM|
|migration excutor|8052|Perform specific migration operations on the worker nodes.|worker VM|