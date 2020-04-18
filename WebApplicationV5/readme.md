https://docs.ansible.com/ansible/latest/user_guide/playbooks_strategies.html

çalıştıralcak task lerin aynı anda kaç makinada çalışcağını ve birlerini bekleyip beklemeyeceklerini belirleyen yapıdır. normald configurayon dosyasından ayarlanır ancak birde plugnini vardır çalışma esnasında o tasks a özel yarlamalar da yapılabilir. 


## Forks

Aynı anda kaç makinada iş yapabilir bunun ayarı. aynı nada kast yapabilir ayar ıdeil dikkat! bunun için serial kullanılır yanı aynı anda kaç tas çalıştırabilir. 

ansible.cfg dosyasında aşağıdaki ayarla yaılır
```
[defaults]
forks = 30
```

## Stratey Types

Default mod linear dir.


```
- hosts: all
  strategy: free
  tasks:
  ...
```


birde serial ayarı vardır bu da aynı ancak kaş



### Free

hiç bir task diğerini beklemez aynı anda serial 5 belirlendiyse ve 10 makinada çalışılacaksa ve fork 5 olarka belirlendiyse ilk 5 makinada task lar bir birini beklemeden devam eder. işi biten task diğer bir makinaya başlar.

### Linear

hiç bir task diğerini bekler aynı anda serial 5 belirlendiyse ve 10 makinada çalışılacaksa ve fork 5 olarak belirlendiyse ilk 5 makinada task lar bir birini bekleyerek (ve aynı anda 5 task çalıştırarak (serial=5)) hepsi bitince diğer 5 makinaya geçerler. 

### Debug

aslında linear çalışır ancak debug yapabilmek için interactive modda çalıştırır
