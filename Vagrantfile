Vagrant.configure("2") do |config|
  config.vm.box = "fedora/36-cloud-base"
  config.vm.box_version = "36-20220504.1"

  config.vm.provider :libvirt do |libvirt|
    libvirt.uri = "qemu:///system"
    libvirt.memory = 3000
    libvirt.storage :file, :type => 'qcow2'
  end

  N = 3
  (1..N).each do |machine_id|
    config.vm.define "node#{machine_id}" do |machine|
      machine.vm.hostname = "node#{machine_id}"
      machine.vm.network :private_network,
        :libvirt__network_name => "macvlan-example",
        :libvirt__dhcp_enabled => false,
        :libvirt__netmask => "255.255.255.0",
        :libvirt__host_ip => "10.10.60.0/24"
      if machine_id == N
        machine.vm.provision :ansible do |ansible|
          # Disable default limit to connect to all the machines
          ansible.limit = "all"
          ansible.playbook = "playbook.yaml"
          ansible.compatibility_mode = "2.0"
        end
      end
    end
  end
end
