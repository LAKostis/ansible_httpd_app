Vagrant.configure("2") do |config|

  config.vm.box = "centos/7"
  
  config.vm.define "backend1"
  config.vm.define "backend2"
  config.vm.define "backend3"
  config.vm.define "lb"
  
  config.vm.provision "ansible_local" do |ansible|
    ansible.become   = true
    ansible.verbose  = true
    ansible.playbook = "site.yml"
    ansible.groups   = {
  	"backends" => ["backend[1:3]"],
  	"lbs" => ["lb"],
  	"all:children" => ["backends", "lbs"],
    }
    ansible.host_vars = {
      "backend1" => {"ipaddr" => "1.2.3.101"},
      "backend2" => {"ipaddr" => "1.2.3.102"},
      "backend3" => {"ipaddr" => "1.2.3.103"},
    }
  end
end

