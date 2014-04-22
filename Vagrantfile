# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/precise64"

  # config.vm.network :private_network, ip: "192.168.33.10"
  #config.vm.network :public_network
  config.vm.network :public_network, bridge: "eth1", :use_dhcp_assigned_default_route => true

  #default account/password
  #config.ssh.username = "root"
  #config.ssh.password = "justfortesting"

  config.vm.provider :virtualbox do |vb|
    # Don't boot with headless mode
    # vb.gui = true

    # Use VBoxManage to customize the VM. For example to change memory:
    vb.customize ["modifyvm", :id, "--memory", "1024"]
  end

  # You need to ensure you have the right version of Chef installed on the box
  # vagrant plugin install vagrant-omnibus
  config.omnibus.chef_version = "11.12.2"

  # in addition to the very basics, we also have vbguest installed
  # vagrant plugin install vagrant-vbguest
  config.vbguest.auto_update = true

  # Chef Solo
  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = "cookbooks"
    chef.add_recipe "apt"
    chef.add_recipe "graphite"
    chef.add_recipe "statsd"
    chef.add_recipe "riemann"
    chef.add_recipe "vim"

    chef.json = {
      :graphite => {
        :python_version => "2.7",
        :password => "justfortesting",
        :timezone=> "Asia/Taipei",
        :carbon => {
          :line_receiver_interface => "0.0.0.0"
        },
        :whisper => {
        },
        :graphite_web => {
        }
      },
      :statsd => {
        :dir => "/opt/statsd",
        :graphite => {
          :global_prefix => "statsd"
        },
        :backends => {
          "riemann" => nil
        },
        :extra_config => { 
          "riemannPort" => 5555,
          "riemannHost"=> "127.0.0.1",
        }
      },
      :riemann => {
	
      }
    }
  end
end
