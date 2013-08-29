vagrant-elasticsearch-puppet
============================

Creates a multi-node elasticsearch cluster using Vagrant and puppet.

Default: Uses virtualbox to setup the machines.
-----------------------------------------------

* Install virtual box and vagrant
* Run 'vagrant up'.

Rackspace Provider:
-------------------

* Install vagrant
* Install the rackspace plugin: '$ vagrant plugin install vagrant-rackspace'
* Install the rackspace dummy box: '$ vagrant box add dummy https://github.com/mitchellh/vagrant-rackspace/raw/master/dummy.box'
* Edit the Vagrantfile with you rackspace information. (http://developer.rackspace.com/blog/vagrant-now-supports-rackspace-open-cloud.html)

Take into account that we are using puppet as a provisioner, so provide an image with puppet installed.

Unfortunately, the network configuration cannot be access on provisioning process. In this scenario the following key:

```
discovery.zen.ping.unicast.hosts: ["public-ip1", "public-ip2", ..]  
```

needs to be changes to the public ips of the machines used in:

```
/usr/local/share/elasticsearch-$version/config/elasticsearch.yml
```

and the service restarted.