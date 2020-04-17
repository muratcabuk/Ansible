bu veryonda asyc ve parallel eklendi


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
bizde bu örnekte python için gerekli ola kütüphanelerin kurulumunu 2 gruba ayırıp async olarka çalıştırdık.



