This is an [Ansible](http://www.ansible.com/home) role for
installation of
[Kibana](http://www.elasticsearch.org/overview/kibana).


You might also want to look at

* an Ansible role for [nginx](https://github.com/dhruvbansal/nginx-ansible-role)
* an Ansible role for [ElasticSearch](https://github.com/dhruvbansal/elasticsearch-ansible-role)

# What it Does

Installs Kibana.

## Configuration & Logging

Creates the files:

* `/var/log/kibana` -- log directory

## Services

Creates a `kibana` system service.

# Usage

Use the role in a playbook like this:

```yaml
- hosts: all
  roles:
    - kibana
```
