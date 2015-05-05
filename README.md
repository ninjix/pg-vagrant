Percona Galera Vagrant Cluster
==============================

Creates a two node Percona Galera cluster with an HAProxy load balancer. Provisioning preformed by Ansible.

Testing
-------

You can test this using this mysql command:

```
mysql -ubob -ppassword -h 192.168.32.50 -e "show variables like 'wsrep_node_name';"
```

You should see the output display the database host name in an alternating round robin pattern.

License
-------

Apache v2

Author Information
------------------

Ninjix - Clayton Kramer (clayton.kramer@gmail.com)

