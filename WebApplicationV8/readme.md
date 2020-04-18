https://docs.ansible.com/ansible/latest/plugins/lookup.html

bu plugin dişarıdan doaya okumak iin kullanılır.

txt, csv etc . farklı dosya formaları için yazılmış extension lar var.


You can combine lookups with Filters, Tests and even each other to do some complex data generation and manipulation. For example:

``` yml
tasks:
  - name: valid but useless and over complicated chained lookups and filters
    debug: msg="find the answer here:\n{{ lookup('url', 'https://google.com/search/?q=' + item|urlencode)|join(' ') }}"
    with_nested:
      - "{{lookup('consul_kv', 'bcs/' + lookup('file', '/the/question') + ', host=localhost, port=2000')|shuffle}}"
      - "{{lookup('sequence', 'end=42 start=2 step=2')|map('log', 4)|list)}}"
      - ['a', 'c', 'd', 'c']

```

You can now control how errors behave in all lookup plugins by setting errors to ignore, warn, or strict. The default setting is strict, which causes the task to fail. For example:

To ignore errors:
``` yml
- name: file doesnt exist, but i dont care .. file plugin itself warns anyways ...
  debug: msg="{{ lookup('file', '/idontexist', errors='ignore') }}"
```


## Invoking lookup plugins with Query

In Ansible 2.5, a new jinja2 function called query was added for invoking lookup plugins. The difference between lookup and query is largely that query will always return a list. The default behavior of lookup is to return a string of comma separated values. lookup can be explicitly configured to return a list using wantlist=True.


This was done primarily to provide an easier and more consistent interface for interacting with the new loop keyword, while maintaining backwards compatibility with other uses of lookup.

The following examples are equivalent:
``` yml
lookup('dict', dict_variable, wantlist=True)

query('dict', dict_variable)
````


As demonstrated above the behavior of wantlist=True is implicit when using query.

Additionally, q was introduced as a shortform of query:

```yml
q('dict', dict_variable)
```