# -*- mode: ruby -*-
# vi: set ft=ruby :

## ElasticSearch cluster
server_count = 4
network = '192.168.234.'
first_ip = 10

servers = []
seeds = ""
(0..server_count-1).each do |i|
  name = 'esnode' + (i + 1).to_s
  ip = network + (first_ip + i).to_s
  seeds << "\"" << ip << "\""
  if i < server_count - 1
    seeds << ", "
  end
  servers << {'name' => name,
              'ip' => ip,
              'num' => i.to_s
             }
end

Vagrant.configure("2") do |config2|
  servers.each do |server|
    config2.vm.define server['name'] do |config|
        #we default to the provider
        config.vm.box = "Puppetlabs_Ubuntu_12_04"
        config.vm.box_url = "http://puppet-vagrant-boxes.puppetlabs.com/ubuntu-server-12042-x64-vbox4210.box"
        config.vm.hostname = server['name']

        config.vm.provider :virtualbox do |vb, override|
            vb.customize ["modifyvm", :id, "--memory", "1024"]
            override.vm.network :private_network, ip: server['ip']
        end

        config.vm.provider :rackspace do |rs, override|
            override.vm.box = "dummy"
            override.ssh.private_key_path = "/home/myuser/.ssh/id_rsa_my_private_key"

            rs.username = "YOUR USERNAME"
            rs.api_key = "YOUR API KEY"
            rs.flavor = /1GB/
            rs.image = /Ubuntu/
            rs.public_key_path = "my_custom_key.pub"
        end
        config.vm.provision :puppet do |puppet|
            puppet.options = "--verbose --debug"
            puppet.facter = { "esversion" => "0.90.3", "seeds" => seeds }
            puppet.module_path = "puppet/modules"
            puppet.manifests_path = "puppet/manifests"
            puppet.manifest_file  = "elasticsearchnode.pp"
        end
    end

  end

end
