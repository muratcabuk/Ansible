bu veryonda asyc ve parallel eklendi ve birazda hadler

dikkat edilmesi gereken ben önemli konu handler lar sadece ilfgili, serverda ilgili task le alakalı bir değişiklik (changed) olduğunda çalışır.

https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html#handlers-running-operations-on-change

bazen her ne olursa lsun ilgili handler ı her defasında yakalamak isteriz budurumda play_book şu şekilde çalıştırılır.

```
ansible_playbook playbook.yml -i inventory.txt --force-handlers
```

yada play içinde  force_handlers: True yapılır yada ansible.cfg içinde force_handlers: True yapıılr.


- https://www.middlewareinventory.com/blog/ansible-async/

- https://www.decodingdevops.com/ansible-asynch-poll-with-examples/

parallelism ve async için 3 yöntem

- Run and check later
- aynı anda Run multiple and check later
- Run all and forget


alttaki komut sitemizi check eden bir monitor.py programı. ve bu task ın tahmini çalışma süresi max 5 dakika.

bizde dakikada bir işin bitip bitmediğini check etmek istiyoruz buna göre diğer işler başlaycak diyelim


```
tasks:
  name: start monitoring
    command: /opt/monitor.py
    async: 360 # max çalışma süresi
    poll: 60 # dakikada bir check ediyoruz

```


eğer işi kontrol etmek istemiyorsak yani ne zaman  biterse ozaman haberimiz olsun

```
tasks:
  name: start monitoring
  command: /opt/monitor.py
  async: 360 # max çalışma süresi
  poll: 60 # dakikada bir check ediyoruz
  register: my_task

- name: get the task status using ansible async_status
  async_status:
    jid: "{{ my_task.ansible_job_id }}"
  register: result
  until: result.finished
  retries: 30

```
yada handler yardımıya bittiğinde haberdar da edebilirz


```
--- #HTTPD Install
- hosts: all
  user: ansible
  sudo: yes
  connection: ssh
  gather_facts: no
  tasks:
    - name: Install httpd
      yum: pkg=httpd state=installed
      async: 300
      poll: 3
      notify: Restart HTTPD
  handlers:
    - name: Restart HTTPD
      service: name=httpd state=restarted
      register: output
    - debug: var=output

```
bizde bu örnekte python için gerekli ola kütüphanelerin kurulumunu 2 gruba ayırdık.

altına 3-5 sniye sleep yapcak bir script ekyerek uzun sürecek bir uygulama haline getirdik.

daha sonra async işlemi belril i,aralıklarla kontrol edecek bir script ile handler ekleledik.



