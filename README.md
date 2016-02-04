This is an [Ansible](http://www.ansible.com/home) role for a
reasonably secure installation of
[Kibana](http://www.elasticsearch.org/overview/kibana).  Kibana sits
on top of [ElasticSearch](http://www.elasticsearch.org/) and
[nginx](http://nginx.org) is used to provide a virtual server.

You might also want to look at

* an Ansible role for [nginx](https://github.com/dhruvbansal/nginx-ansible-role)
* an Ansible role for [ElasticSearch](https://github.com/dhruvbansal/elasticsearch-ansible-role)
* an Ansible role for [Marvel](https://github.com/dhruvbansal/marvel-ansible-role)

# What it Does

Creats a virtual host for nginx that forwards requests back to Kibana
AND ElasticSearch, making it so that you don't need to give full
access to port 9200 on your ElasticSearch cluster to the same browser
that is using Kibana.  Authentication is handled via an nginx
firewall.

## Assumptions

Assumes you have already installed nginx, ElasticSearch and that you
have a backend in your nginx configuration like this

```
upstream elasticsearch {
  server localhost:9200;
  ...
}
```

and a firewall and proxy rules defined already for nginx
(`kibana_firewall_rules` and `kibana_proxy_params`).

## Configuration & Logging

Creates the files:

* `/etc/nginx/sites-available/kibana.conf` -- nginx virtual host for kibana
* `/var/log/kibana` -- log directory

## Services

Creates an `nginx` virtual host.

# Usage

Use the role in a playbook like this:

```yaml
- hosts: all
  roles:
    - kibana
```

## Variables

The following variables are exposed for configuration:

* `kibana_version` -- the version of Kibana to install (default: 3.1.2)
* `kibana_user` -- Kibana user (default: www-data)
* `kibana_group` -- Kibana group (default: www-data)
* `kibana_fqdn` -- Host nginx virtual server will listen for (e.g. - kibana.example.com)
* `kibana_elasticsearch_url` -- full URL for nginx virtual server (e.g. - https://kibana.example.com)
* `kibana_firewall_rules` -- path to nginx firewall configuration
* `kibana_proxy_params` -- path to nginx proxy params
* `kibana_default_route` -- default dashboard to open
* `kibana_elasticsearch_index` -- name of ElasticSearch index used by Kibana (default: kibana-int)

Ensure that `kibana_elasticsearch_url` and `kibana_fqdn` match.  It is
**not** necessary to directly connect the end-user's browser to port
9200 on the ElasticSearch cluster.  The point of the nginx virtual
server created by this role is to obviate that.  The nginx virtual
server will proxy traffic for **both** Kibana and ElasticSearch.
