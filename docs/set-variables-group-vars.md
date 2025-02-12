# Step 2: Set Variables (group_vars)
## Overview
* In a text editor of your choice, open the [environment variables file](https://github.com/IBM/Ansible-OpenShift-Provisioning/blob/main/inventories/default/group_vars/all.yaml).
* This is the master variables file and you will likely reference it many times throughout the process. The default inventory can be found at [inventories/default/group_vars/all.yaml](https://github.com/IBM/Ansible-OpenShift-Provisioning/blob/main/inventories/default/group_vars/all.yaml).
* The variables mark with an `X` are required to be filled in. Many values are pre-filled or are optional. Optional values are commented out; in order to use them, remove the `#` and fill them in.
* This is the most important step in the process. Take the time to make sure everything here is correct.
* <u>Note on YAML syntax</u>: Only the lowest value in each hierarchicy needs to be filled in. For example, at the top of the variables file env and z don't need to be filled in, but the cpc_name does. There are X's where input is required to help you with this.
* Scroll table to the right to see examples for each variable.

## 1 - Workstation
**Variable Name** | **Description** | **Example**
:--- | :--- | :---
**env.workstation.sudo_pass** | The password to your workstation running Ansible.<br /> This will only be used for two things. To ensure you've installed the<br /> pre-requisite packages if you're on Linux, and to add the login URL<br /> to your /etc/hosts file. | Pas$w0rd!

## 2 - LPAR(s)
**Variable Name** | **Description** | **Example**
:--- | :--- | :---
**env.z.high_availability** | Is this cluster spread across three LPARs? If yes, mark True. If not (just in<br /> one LPAR), mark False | True
**env.z.lpar1.create** | To have Ansible create an LPAR and install RHEL on it for the KVM<br /> host, mark True. If using a pre-existing LPAR with RHEL already<br /> installed, mark False. | True
**env.z.lpar1.hostname** | The hostname of the KVM host. | kvm-host-01
**env.z.lpar1.ip** | The IPv4 address of the KVM host. | 192.168.10.1
**env.z.lpar1.user** | Username for Linux admin on KVM host 1. Recommended to run as a non-root user with sudo access. | admin
**env.z.lpar1.pass** | The password for the user that will be created or exists on the KVM host.  | ch4ngeMe!
**env.z.lpar2.create** | To create a second LPAR and install RHEL on it to act as<br /> another KVM host, mark True. If using pre-existing LPAR(s) with RHEL<br /> already installed, mark False. | True
**env.z.lpar2.hostname** | <b>(Optional)</b> The hostname of the second KVM host. | kvm-host-02
**env.z.lpar2.ip** | <b>(Optional)</b> The IPv4 address of the second KVM host. | 192.168.10.2
**env.z.lpar2.user** | Username for Linux admin on KVM host 2. Recommended to run as a non-root user with sudo access. | admin
**env.z.lpar2.pass** | <b>(Optional)</b> The password for the admin user on the second KVM host. | ch4ngeMe!
**env.z.lpar3.create** | To create a third LPAR and install RHEL on it to act as<br /> another KVM host, mark True. If using pre-existing LPAR(s) with RHEL<br /> already installed, mark False. | True
**env.z.lpar3.hostname** | <b>(Optional)</b> The hostname of the third KVM host. | kvm-host-03
**env.z.lpar3.ip** | <b>(Optional)</b> The IPv4 address of the third KVM host. | 192.168.10.3
**env.z.lpar3.user** | Username for Linux admin on KVM host 3. Recommended to run as a non-root user with sudo access. | admin
**env.z.lpar3.pass** | <b>(Optional)</b> The password for the admin user on the third KVM host. | ch4ngeMe!

## 3 - FTP Server
**Variable Name** | **Description** | **Example**
:--- | :--- | :---
**env.ftp.ip** | IPv4 address for the FTP server that will be used to pass config files and<br /> iso to KVM host LPAR(s) and bastion VM during their first boot. | 192.168.10.201
**env.ftp.user** | Username to connect to the FTP server. Must have sudo and SSH access. | ftp-user
**env.ftp.pass** | Password to connect to the FTP server as above user. | FTPpa$s!
**env.ftp.iso_mount_dir** | Directory path relative to FTP root where RHEL ISO is mounted. If FTP root is /var/ftp/pub<br /> and the ISO is mounted at /var/ftp/pub/RHEL/8.5 then this variable would be<br /> RHEL/8.5. No slash before or after. | RHEL/8.5
**env.ftp.cfgs_dir** | Directory path relative to FTP root where configuration files can be stored. If FTP root is /var/ftp/pub<br /> and you would like to store the configs at /var/ftp/pub/ocpz-config then this variable would be<br /> ocpz-config. No slash before or after. | ocpz-config

## 4 - Red Hat Info
**Variable Name** | **Description** | **Example**
:--- | :--- | :---
**env.redhat.username** | Red Hat username with a valid license or free trial to Red Hat<br /> OpenShift Container Platform (RHOCP), which comes with<br /> necessary licenses for Red Hat Enterprise Linux (RHEL) and<br /> Red Hat CoreOS (RHCOS). | redhat.user
**env.redhat.password** | Password to Red Hat above user's account. Used to auto-attach<br /> necessary subscriptions to KVM Host, bastion VM, and pull live<br /> images for OpenShift. | rEdHatPa$s!
**env.redhat.pull_secret** | Pull secret for OpenShift, comes from Red Hat's [Hybrid Cloud Console](https://console.redhat.com/openshift/install/ibmz/user-provisioned).<br /> Make sure to enclose in 'single quotes'.<br />  | '{"auths":{"cloud.openshift<br />.com":{"auth":"b3Blb<br />...<br />4yQQ==","email":"redhat.<br />user@gmail.com"}}}' 

## 5 - Bastion
**Variable Name** | **Description** | **Example**
:--- | :--- | :---
**env.bastion.create** | True or False. Would you like to create a bastion KVM guest to host essential infrastructure services like DNS,<br /> load balancer, firewall, etc? Can de-select certain services with the env.bastion.options<br /> variables below. | True
**env.bastion.vm_name** | Name of the bastion VM. Arbitrary value. | bastion
**env.bastion.resources.disk_size** | How much of the storage pool would you like to allocate to the bastion (in<br /> Gigabytes)? Recommended 30 or more. | 30
**env.bastion.resources.ram** | How much memory would you like to allocate the bastion (in<br /> megabytes)? Recommended 4096 or more | 4096
**env.bastion.resources.swap** | How much swap storage would you like to allocate the bastion (in<br /> megabytes)? Recommended 4096 or more. | 4096
**env.bastion.resources.vcpu** | How many virtual CPUs would you like to allocate to the bastion? Recommended 4 or more. | 4
**env.bastion.resources.os_variant** | Version of Red Hat Enterprise Linux to use for the bastion's operating system.<br /> Recommended 8.4 and above. Must match version of mounted ISO on the FTP server. | 8.5
**env.bastion.networking.ip** | IPv4 address for the bastion. | 192.168.10.3
**env.bastion.networking.hostname** | Hostname of the bastion. Will be combined with<br /> env.bastion.networking.base_domain to create a Fully Qualified Domain Name (FQDN). | ocpz-bastion
**env.bastion.networking.<br />subnetmask** | Subnet of the bastion. | 255.255.255.0
**env.bastion.networking.gateway** | IPv4 of he bastion's gateway server. | 192.168.10.0
**env.bastion.networking.name<br />server1** | IPv4 address of the server that resolves the bastion's hostname. | 192.168.10.200
**env.bastion.networking.name<br />server2** | <b>(Optional)</b> A second IPv4 address that resolves the bastion's hostname. | 192.168.10.201
**env.bastion.networking.interface** | Name of the networking interface on the bastion from Linux's perspective. Most likely enc1. | enc1
**env.bastion.networking.base_<br />domain** | Base domain that, when combined with the hostname, creates a fully-qualified<br /> domain name (FQDN) for the bastion? | ihost.com
**env.bastion.access.user** | What would you like the admin's username to be on the bastion?<br /> If root, make pass and root_pass vars the same. | admin
**env.bastion.access.pass** | The password to the bastion's admin user. If using root, make<br /> pass and root_pass vars the same. | cH4ngeM3!
**env.bastion.access.root_pass** | The root password for the bastion. If using root, make<br /> pass and root_pass vars the same. | R0OtPa$s!
**env.bastion.options.dns** | Would you like the bastion to host the DNS information for the<br /> cluster? True or False. If false, resolution must come from<br /> elsewhere in your environment. Make sure to add IP addresses for<br /> KVM hosts, bastion, bootstrap, control, compute nodes, AND api,<br /> api-int and *.apps as described [here](https://docs.openshift.com/container-platform/4.8/installing/installing_bare_metal/installing-bare-metal-network-customizations.html) in section "User-provisioned<br /> DNS Requirements" Table 5. If True this will be done for you in<br /> the dns and check_dns roles. | True
**env.bastion.options.load<br />balancer.on_bastion** | Would you like the bastion to host the load balancer (HAProxy) for the cluster?<br /> True or False (boolean).<br /> If false, this service must be provided elsewhere in your environment, and public and<br /> private IP of the load balancer must be<br /> provided in the following two variables. | True
**env.bastion.options.load<br />balancer.public_ip** | (Only required if env.bastion.options.loadbalancer.on_bastion is True). The public IPv4<br /> address for your environment's loadbalancer. api, apps, *.apps must use this. | 192.168.10.50
**env.bastion.options.load<br />balancer.private_ip** | (Only required if env.bastion.options.loadbalancer.on_bastion is True). The private IPv4 address<br /> for your environment's loadbalancer. api-int must use this. | 10.24.17.12

## 6 - Cluster Networking
**Variable Name** | **Description** | **Example**
:--- | :--- | :---
**env.cluster.networking.metadata_name** | Name to describe the cluster as a whole, can be anything if DNS will be hosted on the bastion. If<br /> DNS is not on the bastion, must match your DNS configuration. Will be combined with the base_domain<br /> and hostnames to create Fully Qualified Domain Names (FQDN). | ocpz
**env.cluster.networking.base_domain** | The site name, where is the cluster being hosted? This will be combined with the metadata_name<br /> and hostnames to create FQDNs.  | ihost.com
**env.cluster.networking.nameserver1** | IPv4 address that the cluster get its hostname resolution from. If env.bastion.options.dns<br /> is True, this should be the IP address of the bastion. | 192.168.10.200
**env.cluster.networking.nameserver2** | <b>(Optional)</b> A second IPv4 address will the cluster get its hostname resolution from? If env.bastion.options.dns<br /> is True, this should be left commented out. | 192.168.10.201
**env.cluster.networking.forwarder** | What IPv4 address will be used to make external DNS calls? Can use 1.1.1.1 or 8.8.8.8 as defaults. | 8.8.8.8

## 7 - Bootstrap Node
**Variable Name** | **Description** | **Example**
:--- | :--- | :---
**env.cluster.nodes.bootstrap.disk_size** | How much disk space do you want to allocate to the bootstrap node (in Gigabytes)? Bootstrap node<br /> is temporary and will be brought down automatically when its job completes. 120 or more recommended. | 120
**env.cluster.nodes.bootstrap.ram** | How much memory would you like to allocate to the temporary bootstrap node (in<br /> megabytes)? Recommended 16384 or more. | 16384
**env.cluster.nodes.bootstrap.vcpu** | How many virtual CPUs would you like to allocate to the temporary bootstrap node?<br /> Recommended 4 or more. | 4
**env.cluster.nodes.bootstrap.vm_name** | Name of the temporary bootstrap node VM. Arbitrary value. | bootstrap
**env.cluster.nodes.bootstrap.ip** | IPv4 address of the temporary bootstrap node. | 192.168.10.4
**env.cluster.nodes.bootstrap.hostname** | Hostname of the temporary boostrap node. If DNS is hosted on the bastion, this can be anything.<br /> If DNS is hosted elsewhere, this must match DNS definition. This will be combined with the<br /> metadata_name and base_domain to create a Fully Qualififed Domain Name (FQDN). | bootstrap-ocpz

## 8 - Control Nodes
**Variable Name** | **Description** | **Example**
:--- | :--- | :---
**env.cluster.nodes.control.disk_size** | How much disk space do you want to allocate to each control node (in Gigabytes)? 120 or more recommended. | 120
**env.cluster.nodes.control.ram** | How much memory would you like to allocate to the each control<br /> node (in megabytes)? Recommended 16384 or more. | 16384
**env.cluster.nodes.control.vcpu** | How many virtual CPUs would you like to allocate to each control node? Recommended 4 or more. | 4
**env.cluster.nodes.control.vm_name** | Name of the control node VMs. Arbitrary values. Usually no more or less than 3 are used. Must match<br /> the total number of IP addresses and hostnames for control nodes. Use provided list format. | control-1<br />control-2<br />control-3
**env.cluster.nodes.control.ip** | IPv4 address of the control nodes. Use provided<br /> list formatting. | 192.168.10.5<br />192.168.10.6<br />192.168.10.7
**env.cluster.nodes.control.hostname** | Hostnames for control nodes. Must match the total number of IP addresses for control nodes<br /> (usually 3). If DNS is hosted on the bastion, this can be anything. If DNS is hosted elsewhere,<br /> this must match DNS definition. This will be combined with the metadata_name and<br /> base_domain to create a Fully Qualififed Domain Name (FQDN). | control-01<br />control-02<br />control-03

## 9 - Compute Nodes
**Variable Name** | **Description** | **Example**
:--- | :--- | :---
**env.cluster.nodes.compute.disk_size** | How much disk space do you want to allocate to each compute<br /> node (in Gigabytes)? 120 or more recommended. | 120
**env.cluster.nodes.compute.ram** | How much memory would you like to allocate to the each compute<br /> node (in megabytes)? Recommended 16384 or more. | 16384
**env.cluster.nodes.compute.vcpu** | How many virtual CPUs would you like to allocate to each compute node? Recommended 2 or more. | 2
**env.cluster.nodes.compute.vm_name** | Name of the compute node VMs. Arbitrary values. This list can be expanded to any<br /> number of nodes, minimum 2. Must match the total number of IP<br /> addresses and hostnames for compute nodes. Use provided list format. | compute-1<br />compute-2
**env.cluster.nodes.compute.ip** | IPv4 address of the compute nodes. Must match the total number of VM names and<br /> hostnames for compute nodes. Use provided list formatting. | 192.168.10.8<br />192.168.10.9
**env.cluster.nodes.compute.hostname** | Hostnames for compute nodes. Must match the total number of IP addresses and<br /> VM names for compute nodes. If DNS is hosted on the bastion, this can be anything.<br /> If DNS is hosted elsewhere, this must match DNS definition. This will be combined with the<br /> metadata_name and base_domain to create a Fully Qualififed Domain Name (FQDN). | compute-01<br />compute-02

## 10 - Infra Nodes
**Variable Name** | **Description** | **Example**
:--- | :--- | :---
**env.cluster.nodes.infra.disk_size** | <b>(Optional)</b> Set up compute nodes that are made for infrastructure workloads (ingress,<br /> monitoring, logging)? How much disk space do you want to allocate to each infra node (in Gigabytes)?<br /> 120 or more recommended. | 120
**env.cluster.nodes.infra.ram** | <b>(Optional)</b> How much memory would you like to allocate to the each infra node (in<br /> megabytes)? Recommended 16384 or more. | 16384
**env.cluster.nodes.infra.vcpu** | <b>(Optional)</b> How many virtual CPUs would you like to allocate to each infra node?<br /> Recommended 2 or more. | 2
**env.cluster.nodes.infra.vm_name** | <b>(Optional)</b> Name of additional infra node VMs. Arbitrary values. This list can be<br /> expanded to any number of nodes, minimum 2. Must match the total<br /> number of IP addresses and hostnames for infra nodes. Use provided list format. | infra-1<br />infra-2
**env.cluster.nodes.infra.ip** | <b>(Optional)</b> IPv4 address of the infra nodes. This list can be expanded to any number of nodes,<br /> minimum 2. Use provided list formatting. | 192.168.10.8<br />192.168.10.9
**env.cluster.nodes.infra.hostname** | <b>(Optional)</b> Hostnames for infra nodes. Must match the total number of IP addresses for infra nodes.<br /> If DNS is hosted on the bastion, this can be anything. If DNS is hosted elsewhere, this must match<br /> DNS definition. This will be combined with the metadata_name and base_domain<br /> to create a Fully Qualififed Domain Name (FQDN). | infra-01<br />infra-02

## 11 - (Optional) Packages
**Variable Name** | **Description** | **Example**
:--- | :--- | :---
**env.pkgs.galaxy** | A list of Ansible Galaxy collections that will be installed during the setup playbook. The<br /> collections listed are required. Feel free to add more as needed, just make sure to follow the same list format. | community.general
**env.pkgs.workstation** | A list of packages that will be installed on the workstation running Ansible during the setup<br /> playbook. Feel free to add more as needed, just make sure to follow the same list format. | openssh
**env.pkgs.kvm** | A list of packages that will be installed on the KVM Host during the setup_kvm_host playbook.<br /> Feel free to add more as needed, just make sure to follow the same list format. | qemu-kvm
**env.pkgs.bastion** | A list of packages that will be installed on the bastion during the setup_bastion playbook.<br /> Feel free to add more as needed, just make sure to follow the same list format. | haproxy

## 12 - (Optional) Mirror Links
**Variable Name** | **Description** | **Example**
:--- | :--- | :---
**env.openshift.client** | Link to the mirror for the OpenShift client from Red Hat. Feel free to change to a different version, but <br /> make sure it is for s390x architecture. | https://mirror.openshift.com<br />/pub/openshift-v4/s390x/clients<br />/ocp/stable/openshift-<br />client-linux.tar.gz
**env.openshift.installer** | Link to the mirror for the OpenShift installer from Red Hat.<br /> Feel free to change to a different version, but make sure it is for s390x architecture. | https://mirror.openshift.com<br />/pub/openshift-v4/s390x<br />/clients/ocp/stable/openshift-<br />install-linux.tar.gz
**env.coreos.kernel** | Link to the mirror of the CoreOS kernel to be used for the bootstrap, control and compute nodes.<br /> Feel free to change to a different version, but make sure it is for s390x architecture. | https://mirror.openshift.com<br />/pub/openshift-v4/s390x<br />/dependencies/rhcos<br />/4.9/latest/rhcos-4.9.0-s390x-<br />live-kernel-s390x
**env.coreos.initramfs** | Link to the mirror of the CoreOS initramfs to be used for the bootstrap, control and compute nodes.<br /> Feel free to change to a different version, but make sure it is for s390x architecture. | https://mirror.openshift.com<br />/pub/openshift-v4<br />/s390x/dependencies/rhcos<br />/4.9/latest/rhcos-4.9.0-s390x-<br />live-initramfs.s390x.img
**env.coreos.rootfs** | Link to the mirror of the CoreOS rootfs to be used for the bootstrap, control and compute nodes.<br /> Feel free to change to a different version, but make sure it is for s390x architecture. | https://mirror.openshift.com<br />/pub/openshift-v4<br />/s390x/dependencies/rhcos<br />/4.9/latest/rhcos-4.9.0-<br />s390x-live-rootfs.s390x.img 

## 13 - (Optional) OCP Install Config
**Variable Name** | **Description** | **Example**
:--- | :--- | :---
**env.install_config.api_version** | Kubernetes API version for the cluster. These install_config variables will be passed to the OCP<br /> install_config file. This file is templated in the get_ocp role during the setup_bastion playbook.<br /> To make more fine-tuned adjustments to the install_config, you can find it at<br /> roles/get_ocp/templates/install-config.yaml.j2 | v1
**env.install_config.compute.architecture** | Computing architecture for the compute nodes. Must be s390x for clusters on IBM zSystems. | s390x
**env.install_config.compute.hyperthreading** | Enable or disable hyperthreading on compute nodes. Recommended enabled. | Enabled
**env.install_config.control.architecture** | Computing architecture for the control nodes. Must be s390x for clusters on IBM zSystems. | s390x
**env.install_config.control.hyperthreading** | Enable or disable hyperthreading on control nodes. Recommended enabled. | Enabled
**env.install_config.cluster_network.cidr** | IPv4 block in Internal cluster networking in Classless Inter-Domain<br /> Routing (CIDR) notation. Recommended to keep as is. | 10.128.0.0/14
**env.install_config.cluster_network.host_prefix** | The subnet prefix length to assign to each individual node. For example, if<br /> hostPrefix is set to 23 then each node is assigned a /23 subnet out of the given cidr. A hostPrefix<br /> value of 23 provides 510 (2^(32 - 23) - 2) pod IP addresses. | 23
**env.install_config.cluster_network.type** | The cluster network provider Container Network Interface (CNI) plug-in to install.<br /> Either OpenShiftSDN (recommended) or OVNKubernetes. | OpenShiftSDN
**env.install_config.service_network** | The IP address block for services. The default value is 172.30.0.0/16. The OpenShift SDN<br /> and OVN-Kubernetes network providers support only a single IP address block for the service<br /> network. An array with an IP address block in CIDR format. | 172.30.0.0/16
**env.install_config.fips** | True or False (boolean) for whether or not to use the United States' Federal Information Processing<br /> Standards (FIPS). Not yet certified on IBM zSystems. Enclosed in 'single quotes'. | 'false'

## 14 - (Optional) Proxy
**Variable Name** | **Description** | **Example**
:--- | :--- | :---
**proxy_env.http_proxy** | (Optional) A proxy URL to use for creating HTTP connections outside the cluster. Will be<br /> used in the install-config and applied to other Ansible hosts unless set otherwise in<br /> no_proxy below. Must follow this pattern: http://username:pswd>@ip:port | http://ocp-admin:Pa$sw0rd@9.72.10.1:80
**proxy_env.https_proxy** | (Optional) A proxy URL to use for creating HTTPS connections outside the cluster. Will be<br /> used in the install-config and applied to other Ansible hosts unless set otherwise in<br /> no_proxy below. Must follow this pattern: https://username:pswd@ip:port | https://ocp-admin:Pa$sw0rd@9.72.10.1:80  
**proxy_env.no_proxy** | (Optional) A comma-separated list (no spaces) of destination domain names, IP<br /> addresses, or other network CIDRs to exclude from proxying. When using a<br /> proxy, all necessary IPs and domains for your cluster will be added automatically. See<br /> roles/get_ocp/templates/install-config.yaml.j2 for more details on the template. <br />Preface a domain with . to match subdomains only. For example, .y.com matches<br /> x.y.com, but not y.com. Use * to bypass the proxy for all listed destinations. | example.com,192.168.10.1

## 15 - (Optional) Misc
**Variable Name** | **Description** | **Example**
:--- | :--- | :---
**env.language** | What language would you like Red Hat Enterprise Linux to use? In UTF-8 language code.<br /> Available languages and their corresponding codes can be found [here](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html-single/international_language_support_guide/index), in the "Locale" column of Table 2.1. | en_US.UTF-8
**env.timezone** | Which timezone would you like Red Hat Enterprise Linux to use? A list of available timezone<br /> options can be found [here](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones). | America/New_York
**env.ansible_key_name** | (Optional) Name of the SSH key that Ansible will use to connect to hosts. | ansible-ocpz
**env.ocp_key_name** | Comment to describe the SSH key used for OCP. Arbitrary value. | OCPZ-01 key
**env.bridge_name** | (Optional) Name of the macvtap bridge that will be created on the KVM host. | macvtap-net
