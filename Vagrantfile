# -*- mode: ruby -*-
# vi: set ft=ruby :
# vi: set tabstop=2 :
# vi: set shiftwidth=2 :

Vagrant.configure("2") do |config|

  vms = [
    { :name => "app1", :box => "mpeetani/centos7-latest", :eth1 => "192.168.50.15", :host => "2327" },
    { :name => "app2", :box => "mpeetani/centos7-latest", :eth1 => "192.168.50.16", :host => "2328" },
  ]

  vms.each do |opts|
  config.vm.define opts[:name] do |m|
     m.vm.synced_folder ".", "/vagrant", :disabled => true
     m.ssh.insert_key = false
     m.ssh.insert_key = false
     m.ssh.private_key_path = ["~/.ssh/id_rsa", "~/.vagrant.d/insecure_private_key"]
     m.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys"
     m.ssh.username = "vagrant"
     m.ssh.password = "vagrant"
     m.vm.provider "virtualbox" do |v|
      v.cpus = 1
      v.memory = 512
      v.name = opts[:name]

     m.vm.box = opts[:box]
     m.vm.hostname = opts[:name]
     m.vm.network :private_network, ip: opts[:eth1]
     m.vm.network :forwarded_port, guest: 22, host: opts[:host], id: "ssh"
    end
  end
  end
    config.vm.provision "shell", inline: <<-SHELL
    echo "Create Ansible User"
    useradd ansible; echo -e '%ansible\tALL=(ALL)\tNOPASSWD:\tALL' > /etc/sudoers.d/ansible
    echo "password" | passwd ansible --stdin
    mkdir -p /home/ansible/.ssh
    echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDYDijNC+uXVBv5+qLLWcI0dh6Um7azVv+W8PH9qIRPlAjP/ijwZonTJ+pm8n30dgOWEEAikp5c18AOiemh0ppcPvqVC8CIyAsmtlXPz2a2+VTB/2RQQT9+yhjA/P2cVA/V1YiQOEmUhlQ/Gc8vBCBkIURoB114lVvCK3UBwLgZAKvGQtIBxtnpCXzx0UZugInpvHSb4Mgg3d8NdPNpBGjPArRmaLn1frWKjjCzRrmMDM7ZCZETO9gVtlPGDw2EBSsfGeTLd9OUYqopGTLl4+B0aq/5nzQyL3O3WBUhvexAU7H9qX11G7Mvx4vbs1jC5QgjwjSrevpd0fkuEb31qdiXQBYLRlsCuy2eWFnysHe9z8Z5KEHregJ0MTzPSrFvnR32jL1VM5vdKuGsBA3oQqdcV+h5BfG6SHvHRmvdO/hSgK0z4ncIcdBxsIZYa2LR7FUtakjwoEZpUp78MzB0c5AQrcdtUBfV6NWDQvCyFxD1segIWioK0nfmJ6xX6/Ew8mk= harshatheegela@Harsha.local" > /home/ansible/.ssh/authorized_keys
    chown -R ansible:ansible /home/ansible/
    yum update -y
    SHELL
    config.vm.box_check_update = false
#    config.vbguest.auto_update = false
    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "playbook.yml"
    end
 end
