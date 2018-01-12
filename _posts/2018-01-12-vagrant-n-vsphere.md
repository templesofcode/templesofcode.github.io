---
tags: vagrant vsphere
description: vagrantfile
---
First, install the vsphere plugin at [vagrant-vsphere](https://github.com/nsidc/vagrant-vsphere).

Here is a template the can be used to setup a VM in vsphere using vagrant.

```ruby
Vagrant.configure("2") do |config|

  # vagrant-vsphere plugin defines how to produce this file
  config.vm.box = "vsphere-dummy"

  # If you have created a template with a custom key pair, define the private key here. path is relative to the location of Vagrantfile
  # Otherwise comment out
  config.ssh.private_key_path = 'resources/id_rsa'


  # If you created a user for vagrant, define it here.
  # Otherwise, comment it out, the defautl is vagrant
  config.ssh.username ='vagrant'

  # Define the IP that will be given to the VM
  config.vm.network 'private_network', ip: '[[NIP]]'

  # Define the hostname for the VM
  config.vm.hostname = '[[somehost]]'

  config.vm.provider :vsphere do |vsphere, override|

    # NFS usually doesn't work, disable.
    override.nfs.functional = false

    # The vsphere client host
    vsphere.host = '[[CHOST]]'

    vsphere.compute_resource_name ='[[CLUSTER]]'

    # The template we're going to clone
    vsphere.template_name = '[[TEMPLATE]]'

    # The customization specication that we are going to apply on top of the template.
    vsphere.customization_spec_name = '[[MY_CUSTOMIZATION_SPEC]]'

    # The name of the new machine
    vsphere.name = '[[CHOST]]]'

    # vSphere login
    vsphere.user = '[[USER]]'

    # vSphere password
    vsphere.password = '[[PASSWORD]]]'

    # If you don't have SSL configured correctly, set this to 'true'
    vsphere.insecure = true
  end

  # Add and execute pre puppet provisioning scripts, i.e. install puppet if not already installed in the
  # initial template
  pre_puppet_scripts = []
  pre_puppet_scripts.each_with_index {|script, index| 
    puts "#{val} => #{index}" 
      if File.exist?(File.join(File.dirname(__FILE__), "/resources/#{script}"))
        $content = File.read(File.join(File.dirname(__FILE__), "/resources/#{script}"))
        config.vm.provision "shell", inline: $content
     end
  }

  config.vm.provision "puppet_server" do |puppet|
    puppet.puppet_server = "[[PUPPETSERVER]]"
    puppet.puppet_node = "[[CHOST]]"
    puppet.options = "--verbose --debug --environment=[[ENV]]"

    # Make sure you add the cert and key in the resource dir so as to be 
    puppet.client_cert_path = File.join(File.dirname(__FILE__),"/resources/puppet-cert.pem")
    puppet.client_private_key_path = File.join(File.dirname(__FILE__),"/resources/puppet-key.pem")
  end

  # Add and execute post puppet provisioning scripts.
  post_puppet_scripts = []
  post_puppet_scripts.each_with_index {|script, index| 
    puts "#{val} => #{index}" 
      if File.exist?(File.join(File.dirname(__FILE__), "/resources/#{script}"))
        $content = File.read(File.join(File.dirname(__FILE__), "/resources/#{script}"))
        config.vm.provision "shell", inline: $content
     end
  }

end

```
