# langerma-ansible-hadoop

an ansible role to install and configure hadoop on linux

# example inventory
```ìni
[master]
master1
master2
masterq

[hdfs_namenodes]
master1
master2

[data]
data1
data2
data3
data4
data5
data6

[all:children]
master
data
```

## example playbook
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
    hadoop_install_dir: "/app/hadoop"
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
