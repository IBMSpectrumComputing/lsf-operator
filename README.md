[Logo](Spectrum_icon-small.png)
# LSF On Kubernetes/OpenShift

*NOTE: This code is provided without support.*

## Introduction
Welcome to the LSF on Kubernetes repository.  IBM® Spectrum LSF (formerly IBM® Platform™ LSF®) is a complete workload management solution for demanding HPC environments. Featuring intelligent, policy-driven scheduling and easy to use interfaces for job and workflow management, it helps organizations to improve competitiveness by accelerating research and design while controlling costs through superior resource utilization.

## Repository Contents
This repository contains code for packaging LSF as images for running on Kubernetes / OpenShift.  It also contains the code for an Operator for automating the deployment of LSF.  The diagram below illustrates the LSF use case supported by the code in this repository.

[Architecture Diagram](LSFonK8s-overview-v2.png)

The operator will deploy a functioning LSF cluster on Kubernetes.  The LSF cluster will get its resources from Kubernetes.  The jobs will run inside the LSF Agent pods.  The LSF agent pods will provide the OS prerequisites, and LSF binaries.  The application can be built into the LSF Agent pods or mounted from an external NFS server.

User authentication services can be enabled in the LSF pods.  This allows regular users to login and run jobs.  Home directories, datasets, and application binaries can be mounted in the pods.

It is possible to build multiple LSF Compute pod types.  This is necessary to support jobs that have specific OS and library versions.  Instructions are provided.

## Other LSF Kubernetes Integrations
LSF has other native integrations with Kubernetes.  Those support different use cases.
* **LSF Connector for Kubernetes** - This integration supports LSF as the scheduler for Kubernetes.  The purpose of this integration is to allow a single cluster of machines to run both LSF jobs, and Kubernetes pods.  LSF manages the resources, and makes the scheduling decisions for Kubernetes.  For additional information see the [LSF Connector for Kubernetes](https://www.ibm.com/docs/en/spectrum-lsf/10.1.0?topic=lsf-connector-kubernetes)

* **LSF Resource Connector** - This integration allows LSF to dynamically borrow resources from a seperate Kubernetes cluster.  When triggered by job demand LSF will start additional pods on a Kubernetes cluster.  These pods will run LSF compute binaries and will join the existing LSF cluster, and when ready will run jobs.  When the job pressure drops the LSF pods will be automatically shutdown.  For more information see the [LSF Resource Connector](https://www.ibm.com/docs/en/spectrum-lsf/10.1.0?topic=lsf-resource-connnector)

## Preparing to Run LSF on Kubernetes
Running LSF on Kubernetes will take some time to get the LSF agents correctly configured for your workloads

1. [Build the Images](README-Building-the-images.md)
2. [Deploy the LSF Operator](README-lsf-operator.md)
3. [Deploying the LSF Cluster](README-deploying-lsf-cluster.md)
4. [Setting up Storage](README-setting-up-storage.md)
5. [Setting up User Authentication](README-setting-up-user-authentication.md)
6. [Customizing Images for Running Jobs](README-custom-images.md)


*NOTE: This code is provided without support.*
