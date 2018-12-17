Vagrant.configure("2") do |config|

  config.vm.box = "centos/7"
  
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true
  config.vm.define 'backend1' do |node|
    node.vm.hostname = 'backend1.home'
    node.vm.network :private_network, ip: '192.168.42.41'
    node.hostmanager.aliases = %w(backend1)
  end
  config.vm.define "backend2" do |node|
    node.vm.hostname = 'backend2.home'
    node.vm.network :private_network, ip: '192.168.42.42'
    node.hostmanager.aliases = %w(backend2)
  end
  config.vm.define "backend3" do |node|
    node.vm.hostname = 'backend3.home'
    node.vm.network :private_network, ip: '192.168.42.43'
    node.hostmanager.aliases = %w(backend3)
  end
  config.vm.define "lb" do |node|
    node.vm.hostname = 'lb.home'
    node.vm.network :private_network, ip: '192.168.42.44'
    node.hostmanager.aliases = %w(lb)
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.raw_arguments  = "--vault-password-file vault_pw"
    ansible.become   = true
    ansible.verbose  = true
    ansible.playbook = "site.yml"
    ansible.groups   = {
  	"backends" => ["backend[1:3]"],
  	"web" => ["backend[1:3]"],
  	"lbs" => ["lb"],
  	"all:children" => ["backends", "lbs"],
    }
    ansible.host_vars = {
      "backend1" => {"ipaddr" => "192.168.42.41"},
      "backend2" => {"ipaddr" => "192.168.42.42"},
      "backend3" => {"ipaddr" => "192.168.42.43"},
      "lb"       => {"ipaddr" => "192.168.42.44"},
    }
  end
end
