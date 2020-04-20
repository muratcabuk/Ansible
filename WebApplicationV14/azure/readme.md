Azure u çalıştırabilmek için azure cli ı indirip birkez az login komutunu çalıştımış olmak lazım.

windows makina create etmek biraz farklı oz yüzden aşağıdaki linkeri iyi takip etmekl lazım.

 microsoftun kendi başlattığı azure modulu var galaxy de birde community nin devam ettridiği. Aslında ikisi de aynı ansible sadece azure tarafında yapılan modulleri sayfasında toplamış modul issimlerine bakılırsa analaşılşıyor zaten

https://github.com/Azure/azure_preview_modules/blob/master/library/azure_rm_virtualmachine.py

https://docs.ansible.com/ansible/latest/modules/azure_rm_deployment_module.html

https://www.terraform.io/docs/providers/azurerm/r/virtual_machine.html

https://github.com/Azure-Samples/ansible-playbooks/blob/master/vm_create_windows.yml

https://docs.ansible.com/ansible/latest/user_guide/windows_setup.html

https://dev.to/cloudskills/configuring-azure-resources-with-ansible-gpn

https://help.quali.com/Online%20Help/8.3/portal/Content/Admn/Cnfg-WinRM-for-Ansible.htm

https://argonsys.com/microsoft-cloud/articles/configuring-ansible-manage-windows-servers-step-step/

https://dev.to/cloudskills/deploy-a-windows-vm-to-azure-with-ansible-2l9m


- Microsoft

https://docs.microsoft.com/en-us/azure/developer/ansible/

https://galaxy.ansible.com/azure/azure_modules


- Community

https://galaxy.ansible.com/community/azure

https://docs.ansible.com/ansible/latest/modules/list_of_cloud_modules.html#azure

https://docs.ansible.com/ansible/latest/modules/azure_rm_virtualmachine_module.html#azure-rm-virtualmachine-module

https://docs.ansible.com/ansible/latest/modules/azure_rm_virtualmachine_info_module.html#azure-rm-virtualmachine-info-module





```
failed: "msg": "Unsupported parameters for (azure_rm_virtualmachine) module: osProfile Supported parameters include: accept_terms, ad_user, adfs_authority_url, admin_password, admin_username, allocated, api_profile, append_tags, auth_source, availability_set, boot_diagnostics, cert_validation_mode, client_id, cloud_environment, custom_data, data_disks, generalized, image, license_type, location, managed_disk_type, name, network_interface_names, open_ports, os_disk_caching, os_disk_name, os_disk_size_gb, os_type, password, plan, profile, public_ip_allocation_method, remove_on_absent, resource_group, restarted, secret, short_hostname, ssh_password_enabled, ssh_public_keys, started, state, storage_account_name, storage_blob_name, storage_container_name, subnet_name, subscription_id, tags, tenant, virtual_network_name, virtual_network_resource_group, vm_identity, vm_size, winrm, zones"}

```


Azure Dynamic Inventory için rolebased olan klasörü kullandık

basic lan proje makinları create ediyor role based olan ise makinlara kurlumları yapıyor.

https://docs.ansible.com/ansible/latest/scenario_guides/guide_azure.html