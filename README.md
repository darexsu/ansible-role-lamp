# Ansible role LAMP
[![CI Molecule](https://github.com/darexsu/ansible-role-lamp/actions/workflows/ci.yml/badge.svg)](https://github.com/darexsu/ansible-role-lamp/actions/workflows/ci.yml)&emsp;![](https://img.shields.io/static/v1?label=idempotence&message=ok&color=success)&emsp;![Ansible Role](https://img.shields.io/ansible/role/d/59279?color=blue&label=downloads)

  - Role:
      - [platforms](#platforms)
      - [install](#install)
      - [Merge behaviour](#merge-behaviour)
  - Playbooks (merge version):
      - [install and configure: LAMP-stack, (Apache, MariaDB v10.5, PHP-fpm v7.4, Firewalld) ](#install-and-configure-lamp-with-mariadb-merge-version)
      - [install and configure: LAMP-stack, (Apache, MySQL v8.0, PHP-fpm v7.4, Firewalld) ](#install-and-configure-lamp-with-mysql-merge-version)
        - [install: LAMP-stack, (Apache, MariaDB v10.5, PHP-fpm v7.4) ](#install-lamp-with-mariadb-merge-version)
        - [install: LAMP-stack, (Apache, MySQL v8.0, PHP-fpm v7.4) ](#install-lamp-with-mysql-merge-version)
  - Playbooks (full version):
      - [install and configure: LAMP-stack, (Apache, MariaDB v10.5, PHP-fpm v7.4, Firewalld) ](#install-and-configure-lamp-with-mariadb-full-version)
      - [install and configure: LAMP-stack, (Apache, MySQL v8.0, PHP-fpm v7.4, Firewalld) ](#install-and-configure-lamp-with-mysql-full-version)
        - [install: LAMP-stack, (Apache, MariaDB v10.5, PHP-fpm v7.4) ](#install-lamp-with-mariadb-full-version)
        - [install: LAMP-stack, (Apache, MySQL v8.0, PHP-fpm v7.4) ](#install-lamp-with-mysql-full-version)

### Platforms

|  Platforms       | Testing            |
| :--------------: | :----------------: |
| Debian 11        | :heavy_check_mark: |
| Debian 10        | :heavy_check_mark: |
| Ubuntu 20.04     | :heavy_check_mark: |
| Ubuntu 18.04     | :heavy_check_mark: |
| Oracle Linux 8   | :heavy_check_mark: |
| Rocky Linux 8    | :heavy_check_mark: |

### Install

```
ansible-galaxy install darexsu.lamp --force
```

### Merge behaviour

Replace or Merge dictionaries (with "hash_behaviour=replace" in ansible.cfg):
```
# Replace             # Merge
---                   ---
  vars:                 vars:
    dict:                 merge:
      a: "value"            dict: 
      b: "value"              a: "value" 
                              b: "value"

# How does merge work?
Your vars [host_vars]  -->  default vars [current role] --> default vars [include role]
  
  dict:          dict:              dict:
    a: "1" -->     a: "1"    -->      a: "1"
                   b: "2"    -->      b: "2"
                                      c: "3"
    
```
##### Install and configure: LAMP with MariaDB (merge version)
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      # LAMP
      lamp:
        enabled: true
        domen: "example.site"
        project_dir: "/var/www/html/"

      # Apache
      apache:
        enabled: true
      # Apache -> install
      apache_install:
        enabled: true
      # Apache -> config -> apache.conf
      apache_conf:
        enabled: true
      # Apache -> config -> modules
      apache_modules:
        enabled: true
      # Apache -> config -> {virtualhost}.conf
      apache_virtualhost:
        default_conf:
          enabled: true

      # MariaDB
      mariadb:
        enabled: true
        version: "10.5"
        repo: "mariadb"      
      # MariaDB -> install
      mariadb_install:
        enabled: true

      # PHP
      php:
        enabled: true
        version: "7.4"
        repo: "third_party"
      # PHP -> install
      php_install:
        enabled: true
        modules: [common, fpm]

      # PHP -> config -> php.fpm
      php_fpm:
        www_conf:
          enabled: true

      # FirewallD
      firewalld:
        enabled: true
      # FirewallD -> rules
      firewalld_rules:
        service_http:
          enabled: true
        service_https:
          enabled: true

  tasks:
    - name: role darexsu LAMP
      include_role:
        name: darexsu.LAMP
```
##### Install and configure: LAMP with MySQL (merge version)
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      # LAMP
      lamp:
        enabled: true
        domen: "example.site"
        project_dir: "/var/www/html/"

      # Apache
      apache:
        enabled: true
      # Apache -> install
      apache_install:
        enabled: true
      # Apache -> config -> apache.conf
      apache_conf:
        enabled: true
      # Apache -> config -> modules
      apache_modules:
        enabled: true
      # Apache -> config -> {virtualhost}.conf
      apache_virtualhost:
        default_conf:
          enabled: true

      # MySQL
      mysql:
        enabled: true
        repo: "mysql"
        version: "8.0"
      # MySQL -> install
      mysql_install:
        enabled: true

      # PHP
      php:
        enabled: true
        version: "7.4"
        repo: "third_party"
      # PHP -> install
      php_install:
        enabled: true
        modules: [common, fpm]
      # PHP -> config -> php.fpm
      php_fpm:
        www_conf:
          enabled: true

      # FirewallD
      firewalld:
        enabled: true
      # FirewallD -> rules
      firewalld_rules:
        service_http:
          enabled: true
        service_https:
          enabled: true


  tasks:
    - name: role darexsu LAMP
      include_role:
        name: darexsu.LAMP
```
##### Install: LAMP with MariaDB (merge version)
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      # LAMP
      lamp:
        enabled: true
        domen: "example.site"
        project_dir: "/var/www/html/"

      # Apache
      apache:
        enabled: true
      # Apache -> install
      apache_install:
        enabled: true

      # MariaDB
      mariadb:
        enabled: true
        repo: "mariadb"
        version: "10.5"
      # MariaDB -> install
      mariadb_install:
        enabled: true

      # PHP
      php:
        enabled: true
        version: "7.4"
        repo: "third_party"
      # PHP -> install
      php_install:
        enabled: true
        modules: [common, fpm]

  tasks:
    - name: role darexsu LAMP
      include_role:
        name: darexsu.LAMP
```
##### Install: LAMP with MySQL (merge version)
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      # LAMP
      lamp:
        enabled: true
        domen: "example.site"
        project_dir: "/var/www/html/"

      # Apache
      apache:
        enabled: true
      # Apache -> install
      apache_install:
        enabled: true

      # MySQL
      mysql:
        enabled: true
        repo: "mysql"
        version: "8.0"
      # MySQL -> install
      mysql_install:
        enabled: true

      # PHP
      php:
        enabled: true
        version: "7.4"
        repo: "third_party"
      # PHP -> install
      php_install:
        enabled: true
        modules: [common, fpm]

  tasks:
    - name: role darexsu LAMP
      include_role:
        name: darexsu.LAMP
```
##### Install and configure: LAMP with MariaDB (full version)
```yaml
---
- hosts: all
  become: true

  vars:
    # LAMP
    lamp:
      enabled: true
      domen: "example.site"
      project_dir: "/var/www/html/"

    # Apache
    apache:
      enabled: true
      service:
        enabled: true
        state: "started"

    # Apache -> install
    apache_install:
      enabled: true

    # Apache -> config -> apache.conf
    apache_conf:
      enabled: true
      file: "{{ apache_const[ansible_os_family]['conf_file'] }}"
      src: "{{ apache_const[ansible_os_family]['conf_src'] }}"
      backup: false
      data:
        apache_user: "www-data"
        apache_group: "www-data"

    # Apache -> config -> modules
    apache_modules:
      enabled: true
      mods_enabled: [rewrite, proxy, proxy_fcgi]
      mods_disabled: []

    # Apache -> config -> {virtualhost}.conf
    apache_virtualhost:
      default_conf:
        enabled: true
        file: "{{ apache_const[ansible_os_family]['virtualhost_default_file'] }}"
        state: "present"
        src: "../../darexsu.lamp/templates/lamp_apache_virtualhost.j2"
        backup: false
        data:
          ip: "*"
          port: "80"
          ServerName: "{{ lamp.domen }}"
          ServerAdmin: "webmaster@localhost"
          DocumentRoot: "{{ lamp.project_dir }}"
          ErrorLog: "/var/log/{{ apache_const[ansible_os_family]['service_name'] }}/error.log"
          CustomLog: "/var/log/{{ apache_const[ansible_os_family]['service_name'] }}/access.log combined"
          SSLEngine: ""
          SSLCertificateFile: ""
          SSLCertificateKeyFile: ""
          unix_socket:
            enabled: true
            file: "php{{ php.version }}-fpm.sock"

    # MariaDB
    mariadb:
      enabled: true
      repo: "mariadb"
      version: "10.5"
      service:
        state: "started"
        enabled: true

    # MariaDB -> install
    mariadb_install:
      enabled: true

    # PHP
    php:
      enabled: true
      version: "7.4"
      repo: "third_party"
      service:
        enabled: true
        state: "started"

    # PHP -> install
    php_install:
      enabled: true
      modules: [common, fpm]

    # PHP -> config -> php.fpm
    php_fpm:
      www_conf:
        enabled: true
        file: "www.conf"
        state: "present"
        src: "php_fpm.j2"
        backup: false
        data:
          webserver_user: "www-data"
          webserver_group: "www-data"
          pm: "dynamic"
          pm_max_children: "10"
          pm_start_servers: "5"
          pm_min_spare_servers: "5"
          pm_max_spare_servers: "5"
          pm_max_requests: "500"
          tcp_ip_socket:
            enabled: false
            listen: "127.0.0.1:9000"
          unix_socket:
            enabled: true
            file: "php{{ php.version }}-fpm.sock"
            user: "www-data"
            group: "www-data"

    # FirewallD
    firewalld:
      enabled: true
      service:
        enabled: true
        state: "started"

    # FirewallD -> rules
    firewalld_rules:
      service_http:
        enabled: true
        zone: "public"
        state: "enabled"
        service: "http"
        permanent: true
        immediate: true
      service_https:
        enabled: true
        zone: "public"
        state: "enabled"
        service: "https"
        permanent: true
        immediate: true

  tasks:
    - name: role darexsu LAMP
      include_role:
        name: darexsu.LAMP
```
##### Install and configure: LAMP with MySQL (full version)
```yaml
---
- hosts: all
  become: true

  vars:
    # LAMP
    lamp:
      enabled: true
      domen: "example.site"
      project_dir: "/var/www/html/"

    # Apache
    apache:
      enabled: true
      service:
        enabled: true
        state: "started"

    # Apache -> install
    apache_install:
      enabled: true

    # Apache -> config -> apache.conf
    apache_conf:
      enabled: true
      file: "{{ apache_const[ansible_os_family]['conf_file'] }}"
      src: "{{ apache_const[ansible_os_family]['conf_src'] }}"
      backup: false
      data:
        apache_user: "www-data"
        apache_group: "www-data"

    # Apache -> config -> modules
    apache_modules:
      enabled: true
      mods_enabled: [rewrite, proxy, proxy_fcgi]
      mods_disabled: []

    # Apache -> config -> {virtualhost}.conf
    apache_virtualhost:
      default_conf:
        enabled: true
        file: "{{ apache_const[ansible_os_family]['virtualhost_default_file'] }}"
        state: "present"
        src: "../../darexsu.lamp/templates/lamp_apache_virtualhost.j2"
        backup: false
        data:
          ip: "*"
          port: "80"
          ServerName: "{{ lamp.domen }}"
          ServerAdmin: "webmaster@localhost"
          DocumentRoot: "{{ lamp.project_dir }}"
          ErrorLog: "/var/log/{{ apache_const[ansible_os_family]['service_name'] }}/error.log"
          CustomLog: "/var/log/{{ apache_const[ansible_os_family]['service_name'] }}/access.log combined"
          SSLEngine: ""
          SSLCertificateFile: ""
          SSLCertificateKeyFile: ""
          unix_socket:
            enabled: true
            file: "php{{ php.version }}-fpm.sock"

    # MySQL
    mysql:
      enabled: true
      repo: "mysql"
      version: "8.0"
      service:
        state: "started"
        enabled: true

    # MySQL -> install
    mysql_install:
      enabled: true

    # PHP
    php:
      enabled: true
      version: "7.4"
      repo: "third_party"
      service:
        enabled: true
        state: "started"

    # PHP -> install
    php_install:
      enabled: true
      modules: [common, fpm]

    # PHP -> config -> php.fpm
    php_fpm:
      www_conf:
        enabled: true
        file: "www.conf"
        state: "present"
        src: "php_fpm.j2"
        backup: false
        data:
          webserver_user: "www-data"
          webserver_group: "www-data"
          pm: "dynamic"
          pm_max_children: "10"
          pm_start_servers: "5"
          pm_min_spare_servers: "5"
          pm_max_spare_servers: "5"
          pm_max_requests: "500"
          tcp_ip_socket:
            enabled: false
            listen: "127.0.0.1:9000"
          unix_socket:
            enabled: true
            file: "php{{ php.version }}-fpm.sock"
            user: "www-data"
            group: "www-data"

    # FirewallD
    firewalld:
      enabled: true
      service:
        enabled: true
        state: "started"

    # FirewallD -> rules
    firewalld_rules:
      service_http:
        enabled: true
        zone: "public"
        state: "enabled"
        service: "http"
        permanent: true
        immediate: true
      service_https:
        enabled: true
        zone: "public"
        state: "enabled"
        service: "https"
        permanent: true
        immediate: true

  tasks:
    - name: role darexsu LAMP
      include_role:
        name: darexsu.LAMP
```
##### Install: LAMP with MariaDB (full version)
```yaml
---
- hosts: all
  become: true

  vars:
    # LAMP
    lamp:
      enabled: true
      domen: "example.site"
      project_dir: "/var/www/html/"

    # Apache
    apache:
      enabled: true
      service:
        enabled: true
        state: "started"

    # Apache -> install
    apache_install:
      enabled: true

    # MariaDB
    mariadb:
      enabled: true
      repo: "mariadb"
      version: "10.5"
      service:
        state: "started"
        enabled: true

    # MariaDB -> install
    mariadb_install:
      enabled: true

    # PHP
    php:
      enabled: true
      version: "7.4"
      repo: "third_party"
      service:
        enabled: true
        state: "started"

    # PHP -> install
    php_install:
      enabled: true
      modules: [common, fpm]

  tasks:
    - name: role darexsu LAMP
      include_role:
        name: darexsu.LAMP
```
##### Install: LAMP with MySQL (full version)
```yaml
---
- hosts: all
  become: true

  vars:
    # LAMP
    lamp:
      enabled: true
      domen: "example.site"
      project_dir: "/var/www/html/"

    # Apache
    apache:
      enabled: true
      service:
        enabled: true
        state: "started"
    # Apache -> install
    apache_install:
      enabled: true

    # MySQL
    mysql:
      enabled: true
      repo: "mysql"
      version: "8.0"
      service:
        state: "started"
        enabled: true
    # MySQL -> install
    mysql_install:
      enabled: true

    # PHP
    php:
      enabled: true
      version: "7.4"
      repo: "third_party"
      service:
        enabled: true
        state: "started"
    # PHP -> install
    php_install:
      enabled: true
      modules: [common, fpm]

  tasks:
    - name: role darexsu LAMP
      include_role:
        name: darexsu.LAMP
```
