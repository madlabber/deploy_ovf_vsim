deploy_ovf_vsim
================================

Deploys NetApp ONTAP Simulator (vsims) virtual appliances.

Requirements
------------

Requires the vsim-netapp-DOT{{ovf_version}}-cm_nodar.ova virtual appliance downloaded from NetApp.com.  Store the archive in either the role's files directory, or the playbook's files directory.
Requires the community.vmware ansible galaxy collection.
Requires pyvmoni.

Role Variables
--------------

| Variable                | Required | Default           | Example                         | Comments                                                                     |
|-------------------------|----------|-------------------|---------------------------------|------------------------------------------------------------------------------|
| state                   |          | "present"         | "absent"                        | present creates VMs, absent deletes them.
| vm_name                 |          | "SimulateONTAP"   | "vsim1"                         | a valid VM name                                                              |
| vm_datastore            | yes      |                   | "datastore1"                    | the VMware datastore where the node will be placed                           |
| data_network            |          | "VM Network"      | "pgVLAN1"                       | The vSphere portgroup used for ONTAP data and mgmt traffic                   |
| cluster_network         |          |                   | "pgVLAN2"                       | The vSphere portgroup used the for ONTAP cluster network                     |
| vm_disk_provisioning    |          | "thin"            |                                 | Virtual Disk provisioning mode                                               |
| vm_num_cpus             |          | "2"               | "2"                             | number of vCPUs to assign to the VM                                          |
| vm_memory_mb            |          | "8192"            | "8192"                          | amount of memory, in MB, to assign to the VM                                 |
| vm_num_nics             |          | "4"               | "8"                             | number of nics to create on the VM (4-10)                                    |
| vcenter_address         | yes      |                   | "vc.demo.lab"                   | The hostname or IP address of the vCenter server                             |
| vcenter_username        | yes      |                   | "administrator@vsphere.local"   | A vcenter username with rights to deploy the OVA                             |
| vcenter_password        | yes      |                   | "ChangeMe2!"                    | The password for the vcenter user                                            |
| vcenter_datacenter      | yes      |                   | "Datacenter 1"                  | vCenter datacenter where the node will be deployed                           |
| vcenter_cluster         |          |                   |                                 |                                                                              |
| ovf_version             |          | "9.9.1"           | "9.9.1"                         | The StorageGrid OVF version                                                  |
| ovf_file                |          |                   | "/files/vsim-netapp-DOT9.8.ova" | defaults to"{{role_path}}/files/vsim-netapp-DOT{{ovf_version}}-cm_nodar.ova" |
| sys_serial_number       |          | "4082368-50-7"    | "4082368-50-7"                  | The system serial number to assign to the vsim, must match avail. licenses   |
| nvram_sysid             |          |                   | "4082368507"                    | The NVRAM SYSID can optionally be specified.  Randomly generated by default. |
| vdevinit                |          | "34:14:0,34:14:1" | "34:14:0,34:14:1"               | simulated disk configuration.  see vars/main.yml for more information.       |
| ontap_node_mgmt_ip      |          |                   | "192.168.0.91"                  | If specified, node setup will be attempted using the supplied IP             |
| ontap_netmask           |          |                   | "255.255.255.0"                 | Subnet mask for the data network                                             |
| ontap_gateway           |          |                   | "192.168.0.1"                   | Gateway IP for the data network                                              |
| ontap_cluster_mgmt_ip   |          |                   | "192.168.0.90"                  | If specified, cluster setup will be attempted using the supplied IP          |
| ontap_cluster_name      |          |                   | "democluster"                   | ONTAP cluster name                                                           |
| ontap_password          |          | "netapp123"       | "ChangeMe2!"                    | ONTAP admin password                                                         |
| ontap_dns_domain        |          |                   |                                 | DNS domain                                                                   | 
| ontap_dns_server        |          |                   |                                 | DNS Server on the data/mgmt network                                          |
| ontap_location          |          |                   |                                 | optional ONTAP SNMP location value used for cluster setup                    |
| set_admin_password      |          | "false"           | "true"                          | set admin password even on unconfigured nodes                                |
|shelf0_disk_count        |
|shelf0_disk_size         |
|shelf0_disk_type         |
|shelf1_disk_count        |
|shelf1_disk_size         |
|shelf1_disk_type         |
|shelf2_disk_count        |
|shelf2_disk_size         |
|shelf2_disk_type         |
|shelf3_disk_count        |
|shelf3_disk_size         |
|shelf3_disk_type         |


Dependencies
------------

Example Playbook
----------------
      
    ---
    - hosts: localhost 
      name: Build ONTAP Simulator from OVA
      gather_facts: false
      vars: 
        vcenter_address: vcenter.demo.lab
        vcenter_username: administrator@vsphere.local
        vcenter_password: ChangeMe2!
        vcenter_datacenter: "Datacenter"
        vm_datastore: "datastore1"
      tasks:
        - include_role: 
            name: deploy_ovf_vsim
          vars:
            vm_name: vsim1
            ontap_node_mgmt_ip: "192.168.0.91"
            ontap_netmask: "255.255.255.0"
            ontap_gateway: "192.168.0.1"
            ontap_cluster_mgmt_ip: "192.168.0.90"
      

NOTES
-----
Cluster setup currently only works against the 1st node.

Author Information
------------------

Sean Hatfield
sean.hatfield@netapp.com
github.com/madlabber
