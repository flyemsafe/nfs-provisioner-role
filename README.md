NFS Provisioner Role
=========

nfs-provisioner used for allowing stroageclass testing in an OpenShift Platform. This role will provision and NFS server and create an nfs-provisioner class on an OpenShift Cluster.

Requirements
------------

* RHEL7 Should be installed on machine.
* Ansible should be installed on machine

Understanding OpenShift Persistent Storage Types
------------------------------------------------
Access Mode  |  CLI Abbreviation  | Description
--|---|--
ReadWriteOnce  |  RWO  | The volume can be mounted as read-write by a single node.
ReadOnlyMany  | ROX  | The volume can be mounted as read-only by many nodes.
ReadWriteMany  | RWX  | The volume can be mounted as read-write by many nodes.

Supported Access modes For PVs
-------------------------------
Volume Plug-in  | ReadWriteOnce  |  ReadOnlyMany | ReadWriteMany  targetserver
--|---|---|--
AWS EBS  | :heavy_check_mark:  | :x:  | :x:  
Azure File  | :heavy_check_mark:  | :heavy_check_mark:  | :heavy_check_mark:
Azure Disk  |  :heavy_check_mark: | :x:  | :x:
Cinder  | :heavy_check_mark:  | :x:  | :x:
Fibre Channel  | :heavy_check_mark:  | :heavy_check_mark:  | :x:
GCE Persistent Disk  | :heavy_check_mark:  | :x:  |  :x:
HostPath  | :heavy_check_mark:  |  :x: |  :x:
iSCSI  | :heavy_check_mark:  | :heavy_check_mark:  | :x:
Local volume  |  :heavy_check_mark: | :x:  | :x:
NFS  | :heavy_check_mark:  | :heavy_check_mark:  | :heavy_check_mark:
VMware vSphere  | :heavy_check_mark:  | :x:  | :x:

Role Variables
--------------
Type  | Description  | Default Value
--|---|--
provision_nfs_server  | Install and configure nfs server packages  | true
nfs_server_directory_path  |  Set Directory path of nfs storage  | /exports
provision_nfs_provisoner |Configure the nfs-provisioner container on OpenShift | true
configure_registry  |  Configure Registry with nfs-provisioner storage  |  false
nfs_server_ip | Set the ip address of the nfs server | 192.168.1.2
registry_pvc_size | Configure the default size of regisitry | 100Gi  
openshift_install_dir location of auth/kubeconfig | "/home/qubi/qubinode-installer/ocp4"
project_namespace | OpenShift Project name for the nfs-provisioner | nfs-provisioner
rbac_location  | default path of yaml file  | "/usr/local/src/nfs-provisioner-rbac.yaml"
nfs_provisioner_deploy_loc  | default path of yaml file  | "/usr/local/src/nfs-provisioner-deployment.yaml"
sc_location  | default path of yaml file  | "/usr/local/src/nfs-provisioner-sc.yaml"
storage_class_name  |  default storage class name  |  nfs-storage-provisioner
registry_pvc_location  |  default path of yaml file  |   "/usr/local/src/registry-pvc.yaml"
delete_deployment  | delete the deployment and project for nfs-provisioner  | false
openshift_version  | OpenShift version that will will be deploying nfs-provisioner to | ocp4 (ocp3 would be for OpenShift 3.11)
insecure_skip_tls_verify  |  Skip insecure tls verify  |  true

Dependencies
------------

* Ansible
* OpenShift cli

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:
```
    - hosts: targetserver
      become: yes
      vars:
        provision_nfs_server: true
        nfs_server_directory_path: /export
        provision_nfs_provisoner: true
        configure_registry: false
        nfs_server_ip:  changeme
        registry_pvc_size: 100Gi
        storage_class_result: true
        openshift_install_dir: "/home/qubi/qubinode-installer/ocp4"
        openshift_version: ocp4
        project_namespace: nfs-provisioner
        set_as_default: true
        delete_deployment: false
        insecure_skip_tls_verify: true
      roles:
      - nfs-provisioner-role
```

License
-------

GPLv3

Author Information
------------------

This role was created in 2019 by Tosin Akinosho
