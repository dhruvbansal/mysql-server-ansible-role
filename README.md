This is an [Ansible](http://www.ansible.com/home) role for
[MySQL](http://www.mysql.com/).

# What it Does

## Software

Installs

* `mysql-server` package
* Python MySQL client library (useful for Ansible itself)

Also does some addition work:

* setting the MySQL root password
* allowing the data directory to be moved to a storage volume without
  AppArmor freaking out about it
* creates users and privileges
* provides a logstash configuration (disable this by setting
  `mysql_server_use_logstash` to `false`)

## Configuration & Logging

Creates the files:

* `/etc/mysql/my.conf` -- MySQL configuration file
* `/var/log/mysql` -- log directory
* `/etc/logstash/conf.d/mysql.conf` -- inputs for logstash

## Services

Launches MySQL running on port 3306 as well as via a socket at
`/var/run/mysqld/mysqld.sock`.

# Usage

Use the role in a playbook like this:

```yaml
- hosts: all
  roles:
    - mysql-server
```

Set the `mysql_server_use_logstash` to `false` to skip the logstash
configuration.

## Variables

The following variables are exposed for configuration

* `mysql_root_password` -- the password for the MySQL root user
* `mysql_data_dir` -- data directory for MySQL
* `mysql_key_buffer` -- MySQL key buffer (default: 16M)
* `mysql_query_cache_limit` -- MySQL query cache limit (default: 1M)
* `mysql_query_cache_size` -- MySQL query cache size (default: 16M)
* `mysql_general_log` -- MySQL general log (default: 0)
* `mysql_long_query_time` -- MySQL long query time (default: 2)
* `mysql_log_queries_not_using_indexes` -- MySQL log queries not using indexes (default: 0)
* `mysql_bulk_insert_buffer_size` -- MySQL bulk insert buffer size (default: 8M)
* `mysql_ssl_ca` -- MySQL SSL certificate chain
* `mysql_ssl_certificate` -- MySQL SSL certificate
* `mysql_ssl_key` -- MySQL SSL key
* `mysql_users` -- List of users such as this
```yaml
mysql_users:
  john:
    name: jsmith
	password: foobar
  mike:
    name: mjones
	password: bazbooz
```
* `mysql_privs` -- List of user priviliges such as this
```yaml
mysql_privs:
  me:
    name: john
    priv: "*.*:ALL"
```
* `mysql_server_use_logstash` -- set to `false` to skip logstash configuration
