---
tags: centos7 puppet-agent
description: Install puppet agent on centos7
---

Run the following

```bash
echo "Installing repositories"
sudo yum -y install epel-release yum-utils
sudo rpm -ivh https://yum.puppetlabs.com/puppetlabs-release-pc1-el-6.noarch.rpm
sudo rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
sudo yum clean all
sudo yum repolist
echo "Installing puppet"
sudo yum -y install puppet-agent
```
