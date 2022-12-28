221202

redis 실습

# 환경구축

```bash
# base.yml

- name: Configure users on all servers
  hosts: all
  become: yes

  roles:
     - { role: base, tags: [ 'base' ] }
     - { role: docker, tags: [ 'docker' ] }
     - { role: locust, tags: [ 'locust' ] }
     - { role: prometheus_node_exporter, tags: [ 'prometheus_node_exporter' ] }
     - { role: pyenv, tags: [ 'pyenv' ] }
# jdk.yml

- name: Install JVM tools with sdkman
  hosts: all
  roles:
    - role: comcast.sdkman
      sdkman_user: ubuntu
      sdkman_group: ubuntu
      sdkman_auto_answer: true
      sdkman_update: true

      sdkman_install_packages:
        - { candidate: java, version: 11.0.11.9.1-amzn }
        - { candidate: scala, version: 2.13.5 }
        - { candidate: kotlin, version: 1.5.0 }
        - { candidate: sbt, version: 1.5.2 }
        - { candidate: maven, version: 3.8.1 }
        - { candidate: gradle, version: 7.0.1 }

      tags:
        - jvm
# monitor.yml

- name: Install Prometheus
  hosts: prometheus
  become: yes

  roles:
     - { role: prometheus, tags: [ 'prometheus' ] }

- name: Install Grafana
  hosts: grafana
  become: yes

  roles:
     - { role: grafana, tags: [ 'grafana' ] }
# geoip.yml

- name: Install GeoIP
  hosts: geoip
  become: yes

  roles:
     - { role: geoip, tags: [ 'geoip' ] }

- name: Install Nginx
  hosts: geoip
  become: yes

  roles:
     - { role: nginx, tags: [ 'nginx' ] }
# geoip_stop.yml

- name: stop gunicorn
  hosts: geoip
  become: yes

  vars:
     geoip_port: 7001
  roles:
     - { role: geoip_stop, tags: [ 'geoip_stop' ] }
# lb.yml

- name: setup loadbalancer
  hosts: geoip
  become: yes

  vars:
     geoip_port: 7002
  roles:
     - { role: loadbalancer_config, tags: [ 'loadbalancer_config' ] }
# scrap.yml

- name: Install Scrap Servers
  hosts: scrap
  become: yes

  roles:
     - { role: scrap, tags: [ 'scrap' ] }

- name: Install Nginx
  hosts: scrap
  become: yes

  roles:
     - { role: nginx, tags: [ 'nginx' ] }
# ngrinder.yml

- name: Configure users on all servers
  hosts: ngrinder
  become: yes

- name: Configure ngrinder
  hosts: ngrinder
  become: yes

  roles:
     - { role: ngrinder, tags: [ 'ngrinder' ] }
# zookeeper.yml

- name: Configure users on all servers
  hosts: all
  become: yes

  roles:
     - { role: base, tags: [ 'base' ] }
     - { role: docker, tags: [ 'docker' ] }
     - { role: locust, tags: [ 'locust' ] }
     - { role: prometheus_node_exporter, tags: [ 'prometheus_node_exporter' ] }

- name: Install JVM tools with sdkman
  hosts: all
  roles:
    - role: comcast.sdkman
      sdkman_user: ubuntu
      sdkman_group: ubuntu
      sdkman_auto_answer: true
      sdkman_update: true

      sdkman_install_packages:
        - { candidate: java, version: 11.0.11.9.1-amzn }
        - { candidate: scala, version: 2.13.5 }
        - { candidate: kotlin, version: 1.5.0 }
        - { candidate: sbt, version: 1.5.2 }
        - { candidate: maven, version: 3.8.1 }
        - { candidate: gradle, version: 7.0.1 }

      tags:
        - jvm

- name: Install Zookeeper
  hosts: zookeeper
  become: yes

  roles:
     - { role: zookeeper, tags: [ 'zookeeper' ] }
```

[12/03](https://www.notion.so/12-03-2a345d3450044f1ca75d54ddaa529de5)