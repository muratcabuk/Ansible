https://docs.ansible.com/ansible/latest/user_guide/vault.html

vault gizli kalması gereken verieleirn şifrelenmesini ve sadecekulanım esnasında  şifreyi bilenlerin kullanabilmesini saglar.

## Creating Encrypted Files
To create a new encrypted data file, run the following command:

``` shell
ansible-vault create foo.yml
```

## Editing Encrypted Files
To edit an encrypted file in place, use the ansible-vault edit command. This command will decrypt the file to a temporary file and allow you to edit the file, saving it back when done and removing the temporary file:

```
ansible-vault edit foo.yml
```
## Encrypting Unencrypted Files
If you have existing files that you wish to encrypt, use the ansible-vault encrypt command. This command can operate on multiple files at once:
```
ansible-vault encrypt foo.yml bar.yml baz.yml
```

To encrypt existing files with the ‘project’ ID and be prompted for the password:
```
ansible-vault encrypt --vault-id project@prompt foo.yml bar.yml baz.yml
```

## Viewing Encrypted Files
If you want to view the contents of an encrypted file without editing it, you can use the ansible-vault view command:
```
ansible-vault view foo.yml bar.yml baz.yml

```

## Use encrypt_string to create encrypted variables to embed in yaml
The ansible-vault encrypt_string command will encrypt and format a provided string into a format that can be included in ansible-playbook YAML files.

To encrypt a string provided as a cli arg:

```
ansible-vault encrypt_string --vault-password-file a_password_file 'foobar' --name 'the_secret'
#Result:

the_secret: !vault |
      $ANSIBLE_VAULT;1.1;AES256
      62313365396662343061393464336163383764373764613633653634306231386433626436623361
      6134333665353966363534333632666535333761666131620a663537646436643839616531643561
      63396265333966386166373632626539326166353965363262633030333630313338646335303630
      3438626666666137650a353638643435666633633964366338633066623234616432373231333331
      6564

```

To use a vault-id label for ‘dev’ vault-id:

```
ansible-vault encrypt_string --vault-id dev@a_password_file 'foooodev' --name 'the_dev_secret'
# Result:

the_dev_secret: !vault |
          $ANSIBLE_VAULT;1.2;AES256;dev
          30613233633461343837653833666333643061636561303338373661313838333565653635353162
          3263363434623733343538653462613064333634333464660a663633623939393439316636633863
          61636237636537333938306331383339353265363239643939666639386530626330633337633833
          6664656334373166630a363736393262666465663432613932613036303963343263623137386239
          6330

```

To encrypt a string read from stdin and name it ‘db_password’:

```
echo -n 'letmein' | ansible-vault encrypt_string --vault-id dev@a_password_file --stdin-name 'db_password'
```

## Providing Vault Passwords

When all data is encrypted using a single password the --ask-vault-pass or --vault-password-file cli options should be used.

For example, to use a password store in the text file /path/to/my/vault-password-file:
```
ansible-playbook --vault-password-file /path/to/my/vault-password-file site.yml
```

To prompt for a password:

```
ansible-playbook --ask-vault-pass site.yml
```
To get the password from a vault password 
executable script my-vault-password.py:
```
ansible-playbook --vault-password-file my-vault-password.py
```