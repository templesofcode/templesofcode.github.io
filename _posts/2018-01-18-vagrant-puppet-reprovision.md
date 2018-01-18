---
tags: puppet vagrant
description: Invoke puppet provisioning
---

Whenever you need to re-execute the puppet provisioning part of the Vagrantfile (see  [vagrant-n-vsphere]({% post_url 2018-01-12-vagrant-n-vsphere %}),
execute the following in the host..

```bash
> vagrant provision --provision-with puppet_server
```
