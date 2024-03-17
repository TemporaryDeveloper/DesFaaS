# DesFaaS
DesFaaS is a cross-Level joint dynamic deployment system for serverless stateful functions.

For function migration, we realize a user-independent automated function migration process, support synchronous and asynchronous migration mechanisms of function instances under heterogeneous migration media, and support efficient and low-consumption migration of serverless stateful functions.

For state access, we realize a user-independent automated state data scheduling process, supports master-slave state consistency and state lifecycle management under distributed storage, and supports flexible and efficient access to state data of serverless functions.

Based on the function migration and state access components, we design DesFaaS to support the dynamic deployment of location-variable stateful functions. We achieve the autonomous and reliable operation of serverless stateful function clusters by combining the proposed function migration and state scheduling strategies.

# How DesFaaS is realized

The components in DesFaaS are implemented based on the Python Flask framework, totaling about 2000 lines of code.

In the hierarchical state access architecture, DesFaaS uses in-memory variables as local in-memory storage objects and JSON format files as storage objects on local disk and in external storage.

DesFaaS uses the buildah tool to complete the image construction during the function migration process.

In the migration process, the container running state is involved in preserving and recovering. CRI-O container runtime provides the ability to save and restore container runtime state based on the CRIU tool. At the same time, in the case of container runtime support, kubelet provides an interface to perform checkpointing operations on containers running in Kubernetes to save the runtime state and will automatically restore the container runtime state based on the checkpointing image in the case of container runtime support.

Function instances can read and write their state data by accessing the API provided by the system. We have also implemented a Python class, StateRW, to assist developers in accessing state data when writing function instances. We also provided OpenFaaS function template python3-stateful in conjunction with the system to support the development of stateful functions and the construction of mirrors.


# Deploy DesFaaS to manage your stateful serverless functions.

You can refer to the following document for the actual DesFaaS deployment.

[deployment.md](https://github.com/TemporaryDeveloper/DesFaaS/blob/main/docs/deployment.md)

# Access state data in OpenFaaS stateful functions

You could use StateRW class we provide to access state data of DesFaaS, contruct http requests directly to access state data of stateful functions.

Here is the doc of the StateRW class:

[StateRW.md](https://github.com/TemporaryDeveloper/DesFaaS/blob/main/docs/StateRW.md)
