https://docs.ansible.com/ansible/latest/user_guide/playbooks_error_handling.html

Default da aynı anda 3 makinada tasklar çalışırken makinlaradn birinde hataolur diğerleri çalışmaya devam eder.

ağer makinlardan biri hata verdiğinde hepsinin durmasını istiyorsak ozman aşağıdaki ayarı yapmalıyız.

```
-
  name: Deploy Web App
  hosts: server1, server2
  any_error_fatal:true
  tasks:
    - name: task 1 
```

bazende hata olduğunda işilemin devam etmesini diğer tasklara deevam etmesini isteyebiliriz bu durumda aşağıdaki ayar yapılır.

```
-
  name: Deploy Web App
  hosts: server1, server2
  tasks:
    - name: task 1 
      shell: do something
      ignore_errors: yes
```

Bazende hata oluştuğunda bir işlem  yapmak isteyebiliriz bu durumda aşağıdaki kod kullanılır

```

- command: do something
  register: command_output (bu özel bir isim değil)
  failed_when: "'Hata' command_output.stdout" 

```

