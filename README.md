# Notes 

## Examples

### Inventory 

#### 1. file for deploying a Django app
Note that the syntax changes when you are specifying a group of groups, as opposed
to a group of hosts. That’s so Ansible knows to interpret web and task as groups and
not as hosts.
```ini
    [production]
    delaware.example.com
    georgia.example.com
    maryland.example.com
    newhampshire.example.com
    newjersey.example.com
    newyork.example.com
    northcarolina.example.com
    pennsylvania.example.com
    rhodeisland.example.com
    virginia.example.com

    [staging]
    ontario.example.com
    quebec.example.com

    [vagrant]
    vagrant1 ansible_host=127.0.0.1 ansible_port=2222 color=red
    vagrant2 ansible_host=127.0.0.1 ansible_port=2200 color=green
    vagrant3 ansible_host=127.0.0.1 ansible_port=2201 color=purple

    [lb]
    delaware.example.com

    [web]
    georgia.example.com
    newhampshire.example.com
    newjersey.example.com
    ontario.exam

    [web-smart]
    web[01:20].example.com
    web-[a-t].example.com

    [django:children]
    web
    task
```

#### 2. This doesn’t work
The reason is that Ansible’s inventory can associate only a single host with 127.0.0.1,
so the Vagrant group would contain only one host instead of three.
```ini
    [vagrant]
    127.0.0.1:2222
    127.0.0.1:2200
    127.0.0.1:2201
```


#### 3. Specifying group variables in inventory
```ini
[all:vars]
ntp_server=ntp.ubuntu.com
[production:vars]
db_primary_host=rhodeisland.example.com
db_primary_port=5432
db_replica_host=virginia.example.com
db_name=widget_production
db_user=widgetuser
db_password=pFmMxcyD;Fc6)6
rabbitmq_host=pennsylvania.example.com
rabbitmq_port=5672
[staging:vars]
db_primary_host=quebec.example.com
db_primary_port=5432
db_name=widget_staging
db_user=widgetuser
db_password=L@4Ryz8cRUXedj
rabbitmq_host=quebec.example.com
rabbitmq_port=5672
[vagrant:vars]
db_primary_host=vagrant3
db_primary_port=5432
db_name=widget_vagrant
db_user=widgetuser
db_password=password
rabbitmq_host=vagrant3
rabbitmq_port=5672
```
