Pre-Requisites

1. Service Principals
a. Cluster Service Principal - Unique per cluster.
b. Entra ID App Registration - Unique per cluster. 
 - https://docs.openshift.com/rosa/cloud_experts_tutorials/cloud-experts-entra-id-idp.html
c. Installer Service Principal - Common 

Save service principals in AKV using keys found in test_var.yaml.

2. Azure Monitor Shared Key (get from monitoring team)
- https://cloud.redhat.com/experts/aro/clf-to-azure/

3. Add pullsecret to AKV

4. Network team creates VNET, Subnets and storage account and its private endpoint in shared PE subnet.

5. Update test_var.yaml file with installation details.
    vnet section - collect actual values from network team. 
    Create storage account (FileStorage type) and create a file share. Update the vnet.storageaccount fields. 
    Use values from networ team to update cluster.network fields.

    Cluster details - update cluster names, resource group names, domain, region and subscription ID. Do not change the control plane instance type.

    Update integrations.helm_repository to internal repo location.

    Update integrations.azuremonitor.workspaceid

    Update installer.subscription_id and installer.tenant_id

6. Update gitops.infra.values.yaml
  - Update storageaccount names, resource group and fileshare name.
  - Update eso akv tenant id and vault name
  - Update entra id service principal client ID and tenant ID
  - Update group id for cluster rule binding
  - Update image mirror source and mirrors.
  - Add the ACR username, password and name to AKV.
  - Leave autoscaler.enabled as false at cluster build. Once cluster build is completed, update autoscaler.machineset with the actual machineset names and turn to true and push to repository.

