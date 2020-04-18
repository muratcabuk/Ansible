- https://docs.ansible.com/ansible/latest/user_guide/playbooks_templating.html

- https://docs.ansible.com/ansible/latest/modules/template_module.html

Yaml ile ilgili jinja template işlmelerinde aslında altaaki python modulu çalışıyor
- https://pyyaml.org/wiki/PyYAMLDocumentation


## Filters For Formatting Data

To parse multi-document yaml strings, the from_yaml_all filter is provided. The from_yaml_all filter will return a generator of parsed yaml documents.

```
tasks:
  - shell: cat /some/path/to/multidoc-file.yaml
    register: result
  - debug:
      msg: '{{ item }}'
    loop: '{{ result.stdout | from_yaml_all | list }}
```

gelen json verisini parse etmek ve ekrana yazdırmak için

```
tasks:
  - shell: cat /some/path/to/file.json
    register: result

  - set_fact:
      myvar: "{{ result.stdout | from_json }}"
```

## Forcing Variables To Be Defined
The default behavior from ansible and ansible.cfg is to fail if variables are undefined, but you can turn this off.

This allows an explicit check with this feature off:

{{ variable | mandatory }}

The variable value will be used as is, but the template evaluation will raise an error if it is undefined.

## Defaulting Undefined Variables

Jinja2 provides a useful ‘default’ filter that is often a better approach to failing if a variable is not defined:

{{ some_variable | default(5) }}

In the above example, if the variable ‘some_variable’ is not defined, the value used will be 5, rather than an error being raised.

If you want to use the default value when variables evaluate to false or an empty string you have to set the second parameter to true:

{{ lookup('env', 'MY_USER') | default('admin', true) }}


## List Filters
These filters all operate on list variables.

New in version 1.8.

To get the minimum value from list of numbers:

{{ list1 | min }}

To get the maximum value from a list of numbers:

{{ [3, 4, 2] | max }}

New in version 2.5.

Flatten a list (same thing the flatten lookup does):

{{ [3, [4, 2] ] | flatten }}

Flatten only the first level of a list (akin to the items lookup):

{{ [3, [4, [2]] ] | flatten(levels=1) }}


## Dict Filter
New in version 2.6.

To turn a dictionary into a list of items, suitable for looping, use dict2items:

{{ dict | dict2items }}

Which turns:
```
tags:
  Application: payment
  Environment: dev
into:

- key: Application
  value: payment
- key: Environment
  value: dev
  ```

New in version 2.8.

dict2items accepts 2 keyword arguments, key_name and value_name that allow configuration of the names of the keys to use for the transformation:

{{ files | dict2items(key_name='file', value_name='path') }}

Which turns:
```
files:
  users: /etc/passwd
  groups: /etc/group
into:

- file: users
  path: /etc/passwd
- file: groups
  path: /etc/group
  ```
## items2dict filter
New in version 2.7.

This filter turns a list of dicts with 2 keys, into a dict, mapping the values of those keys into key: value pairs:

{{ tags | items2dict }}

Which turns:
```
tags:
  - key: Application
    value: payment
  - key: Environment
    value: dev
into:

Application: payment
Environment: dev

```
## Regular Expression Filters

To search a string with a regex, use the “regex_search” filter:

```
# search for "foo" in "foobar"
{{ 'foobar' | regex_search('(foo)') }}

# will return empty if it cannot find a match
{{ 'ansible' | regex_search('(foobar)') }}

# case insensitive search in multiline mode
{{ 'foo\nBAR' | regex_search("^bar", multiline=True, ignorecase=True) }}
```
To search for all occurrences of regex matches, use the “regex_findall” filter:

```
# Return a list of all IPv4 addresses in the string
{{ 'Some DNS servers are 8.8.8.8 and 8.8.4.4' | regex_findall('\\b(?:[0-9]{1,3}\\.){3}[0-9]{1,3}\\b') }}
```
To replace text in a string with regex, use the “regex_replace” filter:
```
# convert "ansible" to "able"
{{ 'ansible' | regex_replace('^a.*i(.*)$', 'a\\1') }}

# convert "foobar" to "bar"
{{ 'foobar' | regex_replace('^f.*o(.*)$', '\\1') }}

# convert "localhost:80" to "localhost, 80" using named groups
{{ 'localhost:80' | regex_replace('^(?P<host>.+):(?P<port>\\d+)$', '\\g<host>, \\g<port>') }}

# convert "localhost:80" to "localhost"
{{ 'localhost:80' | regex_replace(':80') }}
```