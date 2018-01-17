---
tags: centos7 postgres96
description: install postgres 9.6 on centos 7
---

Install
```bash
echo "Installing postgres 9.6"
wget https://download.postgresql.org/pub/repos/yum/9.6/redhat/rhel-7-x86_64/pgdg-centos96-9.6-3.noarch.rpm
sudo yum install -y pgdg-centos96-9.6-3.noarch.rpm
sudo yum -y update
sudo yum -y install postgresql96-server postgresql96-contrib
sudo /usr/pgsql-9.6/bin/postgresql96-setup initdb
sudo systemctl start postgresql-9.6
sudo systemctl enable postgresql-9.6
```

Configure.. for example
```bash
echo "Updating postgresql connection info"
sed -i -e 's/host    all             all             ::1\/128                 ident/host    all             all             ::1\/128                 password/g' /var/lib/pgsql/9.6/data/pg_hba.conf
sed -i -e 's/host    all             all             127.0.0.1\/32            ident/host    all             all             127.0.0.1\/32            password/'  /var/lib/pgsql/9.6/data/pg_hba.conf
echo 'host    all             +[[APPUSER]]            [[IP]]/32               password' >> /var/lib/pgsql/9.6/data/pg_hba.conf
sudo chmod 600 /var/lib/pgsql/9.6/data/pg_hba.conf
sed -i -e"s/^#listen_addresses.*$/listen_addresses = '0.0.0.0'/" /var/lib/pgsql/9.6/data/postgresql.conf
sudo chmod 600 /var/lib/pgsql/9.6/data/postgresql.conf
sudo systemctl restart postgresql-9.6

```
The configuration is roughly:
- the pg_hba.conf is being modified to only use passwords.
- An application user is being added, \[\[APPUSER\]\] and \[\[IP\]\] should be substituted for hard values instead of these placeholders.
- The postgresql.conf file is being modified to accept connections outside the localhost.


Firewall settings
```bash
sudo firewall-cmd --add-service=postgresql --permanent
sudo firewall-cmd --reload
```
