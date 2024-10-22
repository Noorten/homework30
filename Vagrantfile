MACHINES = {
  :inetRouter  => {
        :box_name => "ubuntu/jammy64",
        :vm_name => "inetRouter",
        :net => [   
                    ["192.168.255.1", 2, "255.255.255.252", "router-net"],
                    ["192.168.56.10", 3, "255.255.255.0"]
                ]
  },

  :centralRouter => {
        :box_name => "ubuntu/jammy64",
        :vm_name => "centralRouter",
        :net => [
                   ["192.168.255.2",  2, "255.255.255.252",  "router-net"],
                   ["192.168.0.1",    3, "255.255.255.240",  "dir-net"],
                   ["192.168.0.33",   4, "255.255.255.240",  "hw-net"],
                   ["192.168.0.65",   5, "255.255.255.192",  "mgt-net"],
                   ["192.168.255.9",  6, "255.255.255.252",  "office1-central"],
                   ["192.168.56.20",  7, "255.255.255.0"],
                   ["192.168.255.13",  8, "255.255.255.252",  "router2"]
                ]
  },

  :centralServer => {
        :box_name => "ubuntu/jammy64",
        :vm_name => "centralServer",
        :net => [
                   ["192.168.0.2",    2, "255.255.255.240",  "dir-net"],
                   ["192.168.56.30", 3, "255.255.255.0"]
               ]
  },
  :inetRouter2 => {
        :box_name => "ubuntu/jammy64",
        :vm_name => "inetRouter2",
        :net => [
		   ["192.168.255.14", 2, "255.255.255.252",  "router2"],
                   ["192.168.56.100", 3, "255.255.255.0"]
               ]
  }
}

Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
    config.vm.define boxname do |box|
      box.vm.box = boxconfig[:box_name]
      box.vm.host_name = boxconfig[:vm_name]
      
      box.vm.provider "virtualbox" do |v|
        v.memory = 768
        v.cpus = 1
      end

      boxconfig[:net].each do |ipconf|
        # Настройка сети
        if ipconf[3] == "private_network"
          box.vm.network "private_network", ip: ipconf[0], adapter: ipconf[1], netmask: ipconf[2], virtualbox__intnet: true
        else
          box.vm.network "private_network", ip: ipconf[0], adapter: ipconf[1], netmask: ipconf[2], virtualbox__intnet: ipconf[3]
        end
      end

      if boxconfig.key?(:public)
        box.vm.network "public_network", boxconfig[:public]
      end
# Настройка Ansible
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.inventory_path = "hosts"
  end
end

  end
end
