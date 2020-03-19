# Ansible Role: Fluentd

[![Build Status](https://travis-ci.com/github/geerlingguy/ansible-role-fluentd.svg?branch=master)](https://travis-ci.com/github/geerlingguy/ansible-role-fluentd)

An Ansible Role that installs Fluentd on RedHat/CentOS or Debian/Ubuntu. This role installs `td-agent`, which is a standalone version that doesn't require Ruby to be installed on the system separately. See [differences between td-agent and Fluentd here](https://www.fluentd.org/faqs).

## Requirements

N/A

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    fluentd_package_state: present

The `td-agent` Fluentd package state; set to `latest` to upgrade or change versions.

    fluentd_service_state: started
    fluentd_service_enabled: true

Controls the Fluentd service options.

## Dependencies

  - geerlingguy.java

## Example Playbook

    - hosts: search
      roles:
        - geerlingguy.fluentd

## License

MIT / BSD

## Author Information

This role was created in 2020 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
