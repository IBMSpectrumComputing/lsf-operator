# Deploying the LSF Operator

The LSF Operator is the automation tool that is used to deploy the LSF cluster.  The LSF operator can deploy and delete LSF clusters in minutes.  The LSF operator must be running before the LSF cluster is deployed.  Instructions for deploying the LSF operator are below.

## Creating the Custom Resource Definitions (CRD) and RBAC
It is recommended that LSF be deployed in a seperate namespace.  Use the following to create a namespace/project to run the LSF cluster in:
On Kubernetes run:
```
kubectl create namespace {Your namespace}
```
On OpenShift run:
```
oc new-project {Your namespace}
```

**NOTE:  For the rest of the instructions the namespace will be assumed to be "lsf".**

Create the Custom Resource Definition using either **kubectl** or **oc** using:
```
kubectl create -f lsf-operator/config/crd/bases/lsf.spectrumcomputing.ibm.com_lsfclusters.yaml
```

Create the service account the LSF operator will run and set permissions:
```
kubectl create -n lsf -f lsf-operator/config/rbac/service_account.yaml
kubectl create -n lsf -f lsf-operator/config/rbac/role.yaml
kubectl create -n lsf -f lsf-operator/config/rbac/role_binding.yaml
```

## Deploy the LSF Operator
The LSF operator will use the image created previously.  The LSF operator image was pushed to a your registry.  The LSF operator yaml file needs to be updated with the image location.  Edit the **lsf-operator/config/manager/manager.yaml** file and change the **image:** value for your registry.  For example:
```
     image: my_registry_location/lsf-operator-amd64:1.0.1-v1
```

Use the following command to deploy the LSF Operator:
```bash
kubectl create -n lsf -f lsf-operator/config/manager/manager.yaml
```

To see the state of the LSF operator run:
```bash
kubectl get pods -n lsf
```

You should see something like:
```bash
oc get pods
NAME                               READY   STATUS    RESTARTS   AGE
ibm-lsf-operator-54b59f875-hbqqc   1/1     Running   0          2h
```
The status should be running and ready should be '1/1'.  It may take a minute or two to reach this state.  
If there is a image pull error check the **image:** value from the steps above.
If it fails to get ready, check the logs by running:
```
kubectl logs -n lsf -f $(kubectl get pods -n lsf |grep ibm-lsf-operator |awk '{ print $1 }')
```
You can also watch the operator run when it is deploying the LSF cluster using this command.

## Deleting the LSF Operator
The LSF operator can be deleted by running:
```
kubectl create -n lsf -f lsf-operator/config/manager/manager.yaml
```

To remove the account, permissions and custom resource definition run the following as a cluster administrator:
```
kubectl delete -n lsf -f lsf-operator/config/rbac/role_binding.yaml
kubectl delete -n lsf -f lsf-operator/config/rbac/role.yaml
kubectl delete -n lsf -f lsf-operator/config/rbac/service_account.yaml
kubectl create -f lsf-operator/config/crd/bases/lsf.spectrumcomputing.ibm.com_lsfclusters.yaml
```

**NOTE:  This will also cause any LSF clusters to be deleted.**

[Return to previous page](README.md)
