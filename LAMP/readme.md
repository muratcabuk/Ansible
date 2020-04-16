LAMP playbook example

- https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html

- https://coderwall.com/p/6zm8rq/how-to-create-a-lamp-stack-with-ansible

- https://github.com/drunomics/ansible-role-lamp/blob/master/vars/main.yml

- https://github.com/Aplyca/ansible-role-lamp

- https://programmer.group/ansible-s-role-playing-roles-installation-of-lamp-environment-using-roles.html

- https://www.mydailytutorials.com/ansible-template-module-examples/

- https://blog.codecentric.de/en/2014/08/jinja2-better-ansible-playbooks-templates/

- https://docs.ansible.com/ansible/latest/user_guide/playbooks_templating.html



- roles/
  - webserver/
    - files/
    - templates/
    - tasks/
    - handlers/
    - vars/
    - defaults/
    - meta/



- Interpretation of the meanings of directories in roles
- Files: Used to store files invoked by the copy module or script module.
- templates: used to store jinjia2 template, template module will automatically find jinjia2 template files in this directory.
- Tasks: This directory should contain a main.yml file that defines the list of tasks for this role. This file can use include
- Contains other task files located in this directory. Default executor for mail.yml
- handlers: This directory should contain a main.yml file that defines the actions to be performed when triggering conditions in this role.
- vars: This directory should contain a main.yml file that defines the variables used by this role.
- defaults: This directory should contain a main.yml file to set default variables for the current role.
- meta: This directory should contain a main.yml file that defines the special settings and dependencies of this role.


Steps to use roles in a playbook: