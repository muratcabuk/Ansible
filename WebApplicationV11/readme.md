## Module Installation


Galaxy

Bir Community edition ama Ansible koordinasyonunda devam  Modules, plugin, Roles vb var

https://github.com/ansible-collections/community.general

örnek kurulum

```
ansible-galaxy collection install community.general
```


## Dev Guide Sayfası


https://docs.ansible.com/ansible/latest/dev_guide/index.html

Yazılcak modullerde Galaxy ye konulakca meta kurallarına uyulmalı ayrıca gemel larka modul yazamnında ansible modulu yazmamnında standardizasyonu var bunklara dikkat edilmeli

diğerlerini saymazsak Python ve Powershell ile module yazılabilir

### Ansible Architecture

https://docs.ansible.com/ansible/latest/dev_guide/overview_architecture.html


### Getting Started

https://docs.ansible.com/ansible/latest/dev_guide/developing_modules_general.html

### Module Format and Documentatian

https://docs.ansible.com/ansible/latest/dev_guide/developing_modules_documenting.html#

### Galaxy Metadata Structure

https://docs.ansible.com/ansible/latest/dev_guide/collections_galaxy_meta.html

### Ansible Module Architecture

https://docs.ansible.com/ansible/latest/dev_guide/developing_program_flow_modules.html

### Developing Plugin

https://docs.ansible.com/ansible/latest/dev_guide/developing_plugins.html

Plugins augment Ansible’s core functionality with logic and features that are accessible to all modules. Ansible ships with a number of handy plugins, and you can easily write your own. All plugins must:

- be written in Python
- raise errors
- return strings in unicode
- conform to Ansible’s configuration and documentation standards

## Module vs Plugin

- Modules are reusable, standalone scripts that can be used by the Ansible API, the ansible command, or the ansible-playbook command. Modules provide a defined interface, accepting arguments and returning information to Ansible by printing a JSON string to stdout before exiting. Modules execute on the target system (usually that means on a remote system) in separate processes.
- Plugins augment Ansible’s core functionality and execute on the control node within the /usr/bin/ansible process. Plugins offer options and extensions for the core features of Ansible - transforming data, logging output, connecting to inventory, and more.
Adding a module locally