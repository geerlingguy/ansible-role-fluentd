# Ansible Role: Fluentd

[![Build Status](https://travis-ci.com/geerlingguy/ansible-role-fluentd.svg?branch=master)](https://travis-ci.com/geerlingguy/ansible-role-fluentd)

An Ansible Role that installs Fluentd on RedHat/CentOS or Debian/Ubuntu. This role installs `td-agent`, which is a standalone version that doesn't require Ruby to be installed on the system separately. See [differences between td-agent and Fluentd here](https://www.fluentd.org/faqs).

## Requirements

N/A

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    fluentd_version: 3 (or 4)
    
The `td-agent` version to install. See https://docs.fluentd.org/quickstart/td-agent-v2-vs-v3-vs-v4 for more details.

    fluentd_package_state: present

The `td-agent` Fluentd package state; set to `latest` to upgrade or change versions.

    fluentd_service_name: td-agent
    fluentd_service_state: started
    fluentd_service_enabled: true

Controls the Fluentd service options.

    fluentd_plugins:
      - fluent-plugin-elasticsearch

    # Alternative format:
    fluentd_plugins:
      - name: fluent-plugin-elasticsearch
        version: '4.0.6'
        state: present

A list of Fluentd plugins to install.

    fluentd_conf_sources: |
      [see defaults/main.yml for default content]

    fluentd_conf_filters: |
      [see defaults/main.yml for default content]

    fluentd_conf_matches: |
      [see defaults/main.yml for default content]

The configuration which will be placed into the `td-agent.conf` file which controls how Fluentd listens for, filters, and routes log data. The defaults set up some basic options which can direct data to Treasure Data, but you should override these values with what's appropriate for your logs.

For example, if you want to monitor an Apache HTTP server's access log, you would add a source:

    fluentd_conf_sources: |
      <source>
        @type tail
        @id input_tail
        <parse>
          @type apache2
        </parse>
        path /var/log/httpd-access.log
        tag apache.access
      </source>

And then you could route `apache` log entries to Elasticsearch using:

    fluentd_conf_matches: |
      <match apache.**>
        @type elasticsearch
        host log.example.com
        port 9200
        user myuser
        password mypassword
        logstash_format true
      </match>

Note that Elasticsearch would require the Fluentd plugin `fluent-plugin-elasticsearch` to be installed.

## Dependencies

N/A

## Example Playbook

```yaml
- hosts: search

  vars:
    fluentd_conf_sources: |
      <source>
        @type tail
        @id input_tail
        <parse>
          @type apache2
        </parse>
        path /var/log/httpd-access.log
        tag apache.access
      </source>

  roles:
    - geerlingguy.fluentd
```

## License

MIT / BSD

## Author Information

This role was created in 2020 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
