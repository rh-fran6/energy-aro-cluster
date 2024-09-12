Pre-Requisites

1. Service Principals
a. Cluster Service Principal - Unique per cluster. This service principal will create have contributor role in the ARO subscription. It must also have Network Contributor role in VNET created by network team

b. Entra ID App Registration - Unique per cluster. 
 - https://docs.openshift.com/rosa/cloud_experts_tutorials/cloud-experts-entra-id-idp.html
c. Installer Service Principal - Common. This service principal must have the following roles:

- Reader role in Network Resource Group scope
- Contributor role in ARO resource Group scope (this will be applied by installer)
- Contributor role at Subscription Level. You can use PIM but the lifetime must be more 1hr or more.
- Be a directory Reader in Entra ID.

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

All subscription IDs are moved to common_var_sample.yaml
Populate and rename common_var_sample.yaml to common_var.yaml

Run installer

SPs to be ready
Create Installer & Cluster SPs --> 
Assign RBAC and Entra ID roles to SPs --> 
Create Entra ID App registration for OAUTH --> 
Collect Azure Monitor Shared Key --> 
Grab Pull Secret from RH Console
:
:
---> Save SPs in Azure Key Vault using the keys in test_var.yaml

Collect VNET names, RGs, Subnets, Storage Accounts and private endpoints and NFS share names from Network team

:
:
:
Update test_var.yaml
:
:
:
Update common_var.yaml

Run cluster installation steps