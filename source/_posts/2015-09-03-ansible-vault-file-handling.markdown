---
layout: post
title: "Ansible Vault File Handling"
date: 2015-09-03 18:38:54 +0200
comments: true
categories:
  - devops
  - ansible
---

If you are familiar with DevOps you hopefully know this advice:

**Never ever** store secret keys in your source code repository!

If someone gains access to your repository then they will also gain access to your whole infrastructure!
<!-- more -->
But when you setup a server with Ansible you maybe need to copy a private key or SSL/TLS certificates to your servers at some time. Copying them manually was not a real option for me so I was happy when I found [Ansible Vault](http://docs.ansible.com/ansible/playbooks_vault.html) which seemed at first like a simple and secure solution for encrypting files for storing them in a repository. However I was very disappointed when I tried it out because it **only** works with variables and not with files in a transparent way. I really wonder why?!

So when you copy a file you maybe have something like this in your playfile (danger! this is the wrong way to do it):

```yaml
- name: copy deployment private key
  copy:
    src: deployment_id_rsa
    dest: "/home/marvin/.ssh/id_rsa"
    owner: marvin
    group: marvin
    mode: 0600
```

So I thought I can simply run `ansible-vault encrypt deployment_id_rsa` and do the same copy command as above. But this will not work because Ansible Vault is only working for variables.

The solution is simple as long as you do not have binary secret data: You have to convert the secret private key file to a variable file which you need to import and also keep hidden in the (verbose `-vvvv`) logs!

### How to modify the copy command:

Copy&Paste your key into a variable file in your roles directory like e.g.  `roles/yourrole/vars/secret_key.yml`. It should look like this, look at the syntax and the usage of `|` in the YAML:

```yaml
---
private_key_content: |
  -----BEGIN RSA PRIVATE KEY-----
  MIIJKAIBAAKCAgEA25qk2rd29s4Z6vTBs1gHznsNS9e3J/C87H5pX0zm0UJtyUfm
  +3hrX5Ag+U368OFO7XeR09iuWhae5FYe5GGFz7CRcIo29hLroXXQgh5cFfrmOdJw
  GCvASbVJhIES0Q/XifS2KNU72fTge7teCsqXo9RgNzNqGK7pgXutPQ8e9NF3NfZj
  XKJhzThde75Ih2Or36R7JKfNfq+jv9t8ulbSrlDltCXafo5+k/RdYtG8QhGKHTHW
  ...
  ...
  ...
  -----END RSA PRIVATE KEY-----  
```

Now encrypt that yaml with Ansible Vault:

```bash
ansible-vault encrypt secret_key.yml
```

At this point it is safe to include it also into your git repository because it is encrypted.

This variable file now needs to be imported in a playbook without logging `no_log: true` and copied to the right destination. The `copy` command can use a `content` attribute for reading from a string. So we will use the variable here.

{% raw %}
```yaml
- name: import secret variable file
  include_vars: "secret_key.yml"
  no_log: true

- name: copy deployment private key
  copy:
    content: "{{private_key_content}}"
    dest: "/home/marvin/.ssh/id_rsa"
    owner: marvin
    group: marvin
    mode: 0600
  no_log: true
```
{% endraw %}

This way you can copy encrypted non-binary files contents without logging them.

On provisioning you only need to add `--ask-vault-pass` to the command. Example:
```bash
ansible-playbook site.yml -i production_hosts -l production -vvvv --ask-vault-pass
```
