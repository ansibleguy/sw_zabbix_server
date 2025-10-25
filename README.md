<a href="https://www.zabbix.com">
<img src="https://assets.zabbix.com/img/logo/zabbix_logo_313x82.png" alt="Zabbix Logo" width="300"/>
</a>

# Ansible Role - Zabbix Server Dockerized

Role to deploy dockerized Zabbix Server on a Linux Server.

[![Lint](https://github.com/O-X-L/ansible-role-zabbix-server/actions/workflows/lint.yml/badge.svg)](https://github.com/O-X-L/ansible-role-zabbix-server/actions/workflows/lint.yml)
[![Ansible Galaxy](https://badges.oss.oxl.app/galaxy.badge.svg)](https://galaxy.ansible.com/ui/standalone/roles/oxlorg/zabbix_server)

**Molecule Integration-Tests**:

* Status: [![Molecule Test Status](https://badges.oss.oxl.app/sw_zabbix_server.molecule.svg)](https://github.com/O-X-L/ansible-role-oxl-cicd/blob/latest/templates/usr/local/bin/cicd/molecule.sh.j2) |
[![Functional-Tests](https://github.com/O-X-L/ansible-role-zabbix-server/actions/workflows/integration_test_result.yml/badge.svg)](https://github.com/O-X-L/ansible-role-zabbix-server/actions/workflows/integration_test_result.yml)
* Logs: [API](https://ci.oss.oxl.app/api/job/ansible-test-molecule-sw_zabbix_server/logs?token=2b7bba30-9a37-4b57-be8a-99e23016ce70&lines=1000) | [Short](https://badges.oss.oxl.app/log/molecule_sw_zabbix_server_test_short.log) | [Full](https://badges.oss.oxl.app/log/molecule_sw_zabbix_server_test.log)

Internal CI: [Tester Role](https://github.com/O-X-L/ansible-role-oxl-cicd) | [Jobs API](https://github.com/O-X-L/github-self-hosted-jobs-systemd)

**Tested:**
* Debian 12

----

## Install

```bash
# latest
ansible-galaxy role install git+https://github.com/O-X-L/ansible-role-zabbix-server

# from galaxy
ansible-galaxy install oxlorg.zabbix_server

# or to custom role-path
ansible-galaxy install oxlorg.zabbix_server --roles-path ./roles

# install dependencies
ansible-galaxy install -r requirements.yml
```

----

## Advertisement

* Need **professional support** using Ansible or Zabbix? Contact us:

  E-Mail: [contact@oxl.at](mailto:contact@oxl.at)

  Tel: [+43 3115 40 900 0](tel:+433115409000)

  Web: [EN](https://www.o-x-l.com) | [DE](https://www.oxl.at)

  Language: German or English

* You want a simple **Ansible GUI**?

  Check-out this [Ansible WebUI](https://github.com/O-X-L/ansible-webui)

----

## Usage

### Config

Minimal example:

```yaml
zabbix_server:
  domain: 'mon.template.oxl.at'

  db:
    root_pwd: !vault |
      ...
    app_pwd: !vault |
      ...
```


Define the config as needed:

```yaml
zabbix_server:
  version: '7.0'  # see docker image tags
  
  domain: 'mon.template.oxl.at'
  aliases:
    - 'monitoring.template.oxl.at'

  # provide settings as environmental variables
  settings:
    # see: https://hub.docker.com/r/zabbix/zabbix-web-nginx-mysql
    frontend:
      ZBX_SERVER_NAME: 'AnsibleGuy Monitoring'
      ZBX_SERVER_PORT: 10151

    # see: https://hub.docker.com/r/zabbix/zabbix-server-mysql
    backend:
      ZBX_LISTENPORT: 10151

  db:
    root_pwd: !vault |
      ...
    app_pwd: !vault |
      ...
```

You might want to use 'ansible-vault' to encrypt your passwords:
```bash
ansible-vault encrypt_string
```

### Execution

Run the playbook:
```bash
ansible-playbook -K -D -i inventory/hosts.yml playbook.yml
```

There are also some useful **tags** available:
* docker
* config
* backup
* update

To debug errors - you can set the 'debug' variable at runtime:
```bash
ansible-playbook -K -D -i inventory/hosts.yml playbook.yml -e debug=yes
```

----

## Functionality

* **Package installation**
  * Ansible dependencies (_minimal_)
  * Docker server + client
  * Nginx Webserver
  * MariaDB client


* **Configuration**
  * MariaDB database container
  * **Default opt-ins**:
    * Auto-Update
    * Installing and Configuring Nginx Webserver

----

## Info

* **Note:** this role currently only supports debian-based systems


* **Note:** Most of the role's functionality can be opted in or out.

  For all available options - see the default-config located in [the main defaults-file](https://github.com/O-X-L/ansible-role-zabbix-server/blob/latest/defaults/main/1_main.yml)!


* **Warning:** Not every setting/variable you provide will be checked for validity. Bad config might break the role!


* **Info:** The default Zabbix Server Login is:

  User: **Admin**
  Password: **zabbix**
