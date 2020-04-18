öncelikle şunu bilmek lazum. Ansible vagrant gibi bir araç değil yani virtualbox yada vmware gibi sanallaştırma araçlarını konfşigure edemez ancak onların üzerinde çalışan VM lerin dynamic listesini alabilir.

- https://docs.ansible.com/ansible/latest/plugins/inventory/virtualbox.html
- https://docs.ansible.com/ansible/latest/plugins/inventory/vmware_vm_inventory.html
- https://docs.ansible.com/ansible/latest/plugins/inventory/docker_swarm.html
- https://docs.ansible.com/ansible/latest/plugins/inventory/docker_machine.html


Bazı sanallaştırma sisitemleriyle de haberleşerek bulut çözümlerinde olduğu gibi otomasyon yaplabilir

- https://docs.ansible.com/ansible/latest/scenario_guides/guide_docker.html
- https://docs.ansible.com/ansible/latest/scenario_guides/guide_kubernetes.html
- https://docs.ansible.com/ansible/latest/scenario_guides/guide_vagrant.html
- https://docs.ansible.com/ansible/latest/scenario_guides/guide_vmware.html


Bu durumda dynamic inventory dışında anlın da VirtuaBox üzerinde direct olarka birşey yapamıyor.

Ancak VmWare üzerinde direkt işlem yapabiliyor. 

https://docs.ansible.com/ansible/latest/scenario_guides/guide_vmware.html

## Ansible for VMware Scenarios

- [Deploy a virtual machine from a template](https://docs.ansible.com/ansible/latest/scenario_guides/vmware_scenarios/scenario_clone_template.html)
- [Rename an existing virtual machine](https://docs.ansible.com/ansible/latest/scenario_guides/vmware_scenarios/scenario_rename_vm.html)
- [Remove an existing VMware virtual machine](https://docs.ansible.com/ansible/latest/scenario_guides/vmware_scenarios/scenario_remove_vm.html)
- [Find folder path of an existing VMware virtual machine](https://docs.ansible.com/ansible/latest/scenario_guides/vmware_scenarios/scenario_find_vm_folder.html)
- [Using VMware HTTP API using Ansible](https://docs.ansible.com/ansible/latest/scenario_guides/vmware_scenarios/scenario_vmware_http.html)



### Deploy a Virtual Machine

``` yml
---
- name: Create a VM from a template
  hosts: localhost
  gather_facts: no
  tasks:
  - name: Clone the template
    vmware_guest:
      hostname: "{{ vcenter_ip }}"
      username: "{{ vcenter_username }}"
      password: "{{ vcenter_password }}"
      validate_certs: False
      name: testvm_2
      template: template_el7
      datacenter: "{{ datacenter_name }}"
      folder: /DC1/vm
      state: poweredon
      cluster: "{{ cluster_name }}"
      wait_for_ip_address: yes
```


#### Scenario Requirements

- Software
  - Ansible 2.5 or later must be installed
  - The Python module Pyvmomi must be installed on the Ansible (or Target host if not executing against localhost)
  - Installing the latest Pyvmomi via pip is recommended [as the OS provided packages are usually out of date and incompatible]

- Hardware
  - vCenter Server with at least one ESXi server

- Access / Credentials
  - Ansible (or the target server) must have network access to the either vCenter server or the ESXi server you will be deploying to
  - Username and Password
  - Administrator user with following privileges
    - Datastore.AllocateSpace on the destination datastore or datastore folder
    - Network.Assign on the network to which the virtual machine will be assigned
    - Resource.AssignVMToPool on the destination host, cluster, or resource pool
    - VirtualMachine.Config.AddNewDisk on the datacenter or virtual machine folder
    - VirtualMachine.Config.AddRemoveDevice on the datacenter or virtual machine folder
    - VirtualMachine.Interact.PowerOn on the datacenter or virtual machine folder
    - VirtualMachine.Inventory.CreateFromExisting on the datacenter or virtual machine folder
    - VirtualMachine.Provisioning.Clone on the virtual machine you are cloning
    - VirtualMachine.Provisioning.Customize on the virtual machine or virtual machine folder if you are customizing the guest operating system
    - VirtualMachine.Provisioning.DeployTemplate on the template you are using
    - VirtualMachine.Provisioning.ReadCustSpecs on the root vCenter Server if you are customizing the guest operating system
  
  - Depending on your requirements, you could also need one or more of the following privileges:
    - VirtualMachine.Config.CPUCount on the datacenter or virtual machine folder
    - VirtualMachine.Config.Memory on the datacenter or virtual machine folde
    - VirtualMachine.Config.DiskExtend on the datacenter or virtual machine folder
    - VirtualMachine.Config.Annotation on the datacenter or virtual machine folder
    - VirtualMachine.Config.AdvancedConfig on the datacenter or virtual machine folder
    - VirtualMachine.Config.EditDevice on the datacenter or virtual machine folder
    - VirtualMachine.Config.Resource on the datacenter or virtual machine folder
    - VirtualMachine.Config.Settings on the datacenter or virtual machine folder
    - VirtualMachine.Config.UpgradeVirtualHardware on the datacenter or virtual machine folder
    - VirtualMachine.Interact.SetCDMedia on the datacenter or virtual machine folder
    - VirtualMachine.Interact.SetFloppyMedia on the datacenter or virtual machine folder
    - VirtualMachine.Interact.DeviceConnection on the datacenter or virtual machine folder
  
- Assumptions
  - All variable names and VMware object names are case sensitive
  - VMware allows creation of virtual machine and templates with same name across datacenters and within datacenters
  - You need to use Python 2.7.9 version in order to use validate_certs option, as this version is capable of changing the SSL verification behaviours


## Vagrant with Ansible

vagrant up komutu çalıştırıldığında sanal makin ayağa kaldırıp hemen provisionar olarka Ansible playbook vagrantfile içinde çağrılabilir.

https://www.vagrantup.com/docs/provisioning/ansible.html

https://docs.ansible.com/ansible/latest/scenario_guides/guide_vagrant.html


```
# This guide is optimized for Vagrant 1.8 and above.
# Older versions of Vagrant put less info in the inventory they generate.
Vagrant.require_version ">= 1.8.0"

Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/bionic64"

  config.vm.provision "ansible" do |ansible|
    ansible.verbose = "v"
    ansible.playbook = "playbook.yml"
  end
end
```


daha birçok işlem yaptırılabilir

https://www.vagrantup.com/docs/provisioning/ansible.html
