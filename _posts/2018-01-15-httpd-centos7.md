---
tags: centos7 httpd selinux firewalld symfony
description: setup httpd in centos7
---

HTTPD setup steps for a symfony application

Install httpd from the ius repo
```bash
# yum install -y httpd24u.x86_64
```

Configure firewall
```bash
# firewall-cmd --add-service=http --permanent && sudo firewall-cmd --add-service=https --permanent
# systemctl restart firewalld
```

Configure service
```bash
sudo systemctl enable httpd
```

Configure SELINUX
```bash
# yum -y install policycoreutils-python
# semanage fcontext -a -t httpd_sys_content_t "/PATH/TO/WEBAPP(/.*)?"
# semanage fcontext -a -t httpd_sys_rw_content_t "/PATH/TO/WEBAPP/var/logs(/.*)?"
# semanage fcontext -a -t httpd_cache_t "/PATH/TO/WEBAPP/var/cache(/.*)?"
# semanage fcontext -a -t httpd_cache_t "/PATH/TO/WEBAPP/var/sessions(/.*)?"
# restorecon -Rv /PATH/TO/WEBAPP
# setsebool -P httpd_can_network_connect 1
```
