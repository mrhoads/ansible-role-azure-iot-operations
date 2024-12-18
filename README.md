Role Name
=========

This role onboards a k3s cluster to Azure Arc, creates a storage account and schema registry, and then deploys Azure IoT Operations on the cluster.

Requirements
------------

This role assumes that the k3s cluster has already been created.  In addition, a service principal must exist that has permission to onboard the Kubernetes cluster, create a storage account, and deploy the Azure IoT Operations extension.  This var looks like the following and should be stored securely (e.g. using Vault)

```
azure_service_principal:
  client_id: <your client ID>
  client_secret: <your client secret>
  tenant_id: <your tenant ID>
  subscription_id: <your subscription ID>
```

In addition, the [jmespath package](https://pypi.org/project/jmespath/) must be installed on the target.

Role Variables
--------------

Aside from the service principal described earlier, the following variables are used:

- azure_arc_resource_group: "test-ansible-k3s-rg" <-- the already existing resource group in which the cluster and storage account will be created
- azure_arc_cluster_name: "ansibletest" <-- the name for the Arc-enabled Kubernetes cluster
- azure_arc_location: "eastus2" <-- the Azure region in which the Arc-enabled Kubernetes cluster and storage account cluster will be created.  Note the [supported regions](https://learn.microsoft.com/azure/iot-operations/overview-iot-operations#supported-regions) for Azure IoT Operations
- azure_aio_schema_registry_name: "schemaregistry" <-- the name of the schema registry
- storage_account_name: "{{ lookup('ansible.builtin.password', '/dev/null', length=13, chars=['ascii_letters', 'digits'], seed=inventory_hostname) | lower }}" <-- the name of the storage account.  Using the default will result in a random but idempotent storage account name


Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

This role can be deployed using something like the following:

```yaml
---
- name: Apply Azure IoT Operations Role
  hosts: all
  vars_files:
    - azure_service_principal.yml
  roles:
    - role: azure_iot_ops_role
```

If using Ansible Vault to store the service principal credentials, add `--ask-vault-pass` when executing the playbook.

License
-------

GPLv3

Author Information
------------------

[Mike Rhoads](https://www.linkedin.com/in/mike-rhoads/)
