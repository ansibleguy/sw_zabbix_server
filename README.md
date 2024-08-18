<a href="https://www.zabbix.com">
<img src="https://assets.zabbix.com/img/logo/zabbix_logo_313x82.png" alt="Zabbix Logo" width="300"/>
</a>

# Ansible Role - Zabbix Server Dockerized

**WARNING: This role is still in early development! DO NOT USE IN PRODUCTION!**

Role to deploy dockerized Zabbix Server on a Linux Server.

<a href='https://ko-fi.com/ansible0guy' target='_blank'><img height='35' style='border:0px;height:46px;' src='https://az743702.vo.msecnd.net/cdn/kofi3.png?v=0' border='0' alt='Buy me a coffee' />

[![Molecule Test Status](https://badges.ansibleguy.net/sw_zabbix_server.molecule.svg)](https://github.com/ansibleguy/_meta_cicd/blob/latest/templates/usr/local/bin/cicd/molecule.sh.j2)
[![YamlLint Test Status](https://badges.ansibleguy.net/sw_zabbix_server.yamllint.svg)](https://github.com/ansibleguy/_meta_cicd/blob/latest/templates/usr/local/bin/cicd/yamllint.sh.j2)
[![PyLint Test Status](https://badges.ansibleguy.net/sw_zabbix_server.pylint.svg)](https://github.com/ansibleguy/_meta_cicd/blob/latest/templates/usr/local/bin/cicd/pylint.sh.j2)
[![Ansible-Lint Test Status](https://badges.ansibleguy.net/sw_zabbix_server.ansiblelint.svg)](https://github.com/ansibleguy/_meta_cicd/blob/latest/templates/usr/local/bin/cicd/ansiblelint.sh.j2)
[![Ansible Galaxy](https://badges.ansibleguy.net/galaxy.badge.svg)](https://galaxy.ansible.com/ui/standalone/roles/ansibleguy/sw_zabbix_server)

Molecule Logs: [Short](https://badges.ansibleguy.net/log/molecule_sw_zabbix_server_test_short.log), [Full](https://badges.ansibleguy.net/log/molecule_sw_zabbix_server_test.log)

**Tested:**
* Debian 12

## Install

```bash
# latest
ansible-galaxy role install git+https://github.com/ansibleguy/sw_zabbix_server

# from galaxy
ansible-galaxy install ansibleguy.sw_zabbix_server

# or to custom role-path
ansible-galaxy install ansibleguy.sw_zabbix_server --roles-path ./roles

# install dependencies
ansible-galaxy install -r requirements.yml
```

----

## Usage

### Config

Minimal example:

```yaml
zabbix_server:
  domain: 'mon.template.ansibleguy.net'

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
  
  domain: 'mon.template.ansibleguy.net'
  aliases:
    - 'monitoring.template.ansibleguy.net'

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

  For all available options - see the default-config located in [the main defaults-file](https://github.com/ansibleguy/sw_zabbix_server/blob/latest/defaults/main/1_main.yml)!


* **Warning:** Not every setting/variable you provide will be checked for validity. Bad config might break the role!


* **Info:** The default Zabbix Server Login is:

  User: **Admin**
  Password: **zabbix**
