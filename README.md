# langerma-ansible-hadoop

an ansible role to install and configure hadoop on linux

## sample playbook
```yml
- hosts: hadoop
  become: true
  vars:
    java_version: 11
    java_install_dir: "/app/java"
    supervisord_ini_dir: "/app/supervisord.d"
    hadoop_user: "hadoop"
    hadoop_group: "hadoop"
    hadoop_install_tar: "files/hadoop-2.9.2.tar.gz"
    hadoop_install_dir: "/app/zookeeper"
    hadoop_config_path: "{{ hadoop_install_dir }}/etc/hadoop"
    hadoop_tmp: "{{ hadoop_install_dir }}/tmp"
    hadoop_dfs_name: "{{ hadoop_install_dir }}/dfs/name"
    hadoop_dfs_data: "{{ hadoop_install_dir }}/dfs/data"
    hadoop_log_path: "{{ hadoop_install_dir }}/log"
    hadoop_ssh_key: ""
    hadoop_ssh_key_pub: ""
    add_user: true
    open_firewall: false
    disable_firewall: false
    install_hadoop: true
    config_hadoop: true

    hadoop_create_path:
      - "{{ hadoop_install_dir }}"
      - "{{ hadoop_tmp }}"
      - "{{ hadoop_dfs_name }}"
      - "{{ hadoop_dfs_data }}"
      - "{{ hadoop_log_path }}"

    # hadoop configration
    hdfs_port: 9000
    core_site_properties:
      - {
          "name":"fs.defaultFS",
          "value":"hdfs://{{ master_ip }}:{{ hdfs_port }}"
      }
      - {
          "name":"hadoop.tmp.dir",
          "value":"file:{{ hadoop_tmp }}"
      }
      - {
        "name":"io.file.buffer.size",
        "value":"131072"
      }

    dfs_namenode_httpport: 9001
    hdfs_site_properties:
      - {
          "name":"dfs.namenode.secondary.http-address",
          "value":"{{ master_hostname }}:{{ dfs_namenode_httpport }}"
      }
      - {
          "name":"dfs.namenode.name.dir",
          "value":"file:{{ hadoop_dfs_name }}"
      }
      - {
          "name":"dfs.namenode.data.dir",
          "value":"file:{{ hadoop_dfs_data }}"
      }
      - {
          "name":"dfs.replication",
          "value":"{{ groups['workers']|length }}"
      }
      - {
        "name":"dfs.webhdfs.enabled",
        "value":"true"
      }

    mapred_site_properties:
      - {
       "name": "mapreduce.framework.name",
       "value": "yarn"
      }
      - {
       "name": "mapreduce.admin.user.env",
       "value": "HADOOP_MAPRED_HOME=$HADOOP_COMMON_HOME"
      }
      - {
        "name":"yarn.app.mapreduce.am.env",
        "value":"HADOOP_MAPRED_HOME=$HADOOP_COMMON_HOME"
      }

    yarn_resourcemanager_port: 8040
    yarn_resourcemanager_scheduler_port: 8030
    yarn_resourcemanager_webapp_port: 8088
    yarn_resourcemanager_tracker_port: 8025
    yarn_resourcemanager_admin_port: 8141

    yarn_site_properties:
      - {
        "name":"yarn.resourcemanager.address",
        "value":"{{ master_hostname }}:{{ yarn_resourcemanager_port }}"
      }
      - {
        "name":"yarn.resourcemanager.scheduler.address",
        "value":"{{ master_hostname }}:{{ yarn_resourcemanager_scheduler_port }}"
      }
      - {
        "name":"yarn.resourcemanager.webapp.address",
        "value":"{{ master_hostname }}:{{ yarn_resourcemanager_webapp_port }}"
      }
      - {
        "name": "yarn.resourcemanager.resource-tracker.address",
        "value": "{{ master_hostname }}:{{ yarn_resourcemanager_tracker_port }}"
      }
      - {
        "name": "yarn.resourcemanager.admin.address",
        "value": "{{ master_hostname }}:{{ yarn_resourcemanager_admin_port }}"
      }
      - {
        "name": "yarn.nodemanager.aux-services",
        "value": "mapreduce_shuffle"
      }
      - {
        "name": "yarn.nodemanager.aux-services.mapreduce.shuffle.class",
        "value": "org.apache.hadoop.mapred.ShuffleHandler"
      }

    dashbord_port: 9870
    firewall_ports:
      - "{{ dashbord_port}}"
      - "{{ hdfs_port }}"
      - "{{ dfs_namenode_httpport }}"
      - "{{ yarn_resourcemanager_port }}"
      - "{{ yarn_resourcemanager_scheduler_port }}"
      - "{{ yarn_resourcemanager_webapp_port }}"
      - "{{ yarn_resourcemanager_tracker_port }}"
      - "{{ yarn_resourcemanager_admin_port }}"

  roles:
    - role: langerma-ansible-hadoop
```
