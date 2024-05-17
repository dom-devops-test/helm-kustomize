# ENVIRONMENT

Folder containing sub-directories for all your environments. Every environment contains:
* a cluster directory that contains your cluster's kubeconfig file
* a resources directory containing both your addons and tenants sub-directories
  * addons: directory for your application that only need to be deployed once not per tenant on the cluster
  * tenants: contains each of your tenants directories and a shared directory for the shared files between tenants