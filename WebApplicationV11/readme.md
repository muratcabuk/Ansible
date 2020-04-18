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
