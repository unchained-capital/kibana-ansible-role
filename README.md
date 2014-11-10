This is an [Ansible](http://www.ansible.com/home) role for a
reasonably secure installation of
[Kibana](http://www.elasticsearch.org/overview/kibana).  Kibana sits
on top of [ElasticSearch](http://www.elasticsearch.org/) and
[nginx](http://nginx.org) is used to provide a virtual server.

It assumes an
[nginx upstream](http://nginx.org/en/docs/http/ngx_http_upstream_module.html) backend named
`elasticsearch` exists.  Define it like this somewhere

```
upstream elasticsearch {
  server localhost:9200;
  ...
}
```

The server defined for Kibana (`kibana_fqdn`) should match the
ElasticSearch URL used by Kibana (`kibana_elasticsearch_url`) as nginx
will proxy traffic for both Kibana and ElasticSearch via the same
virtual host.

Firewall rules are proxy handling parameters are assumed to be defined
in existing nginx configuration files (`kibana_firewall_rules` and
`kibana_proxy_params`).

A logstash input file is also defined for the nginx logs created by
the virtual server.
