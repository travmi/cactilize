$hostnames = <<EOF
echo "Setting up /etc/hosts"
echo -e "172.20.20.5\tansible\n172.20.20.6\tserver\n172.20.20.10\tclient1\n172.20.20.11\tclient2\n172.20.20.12\tclient3\n172.20.20.13\tclient4\n172.20.20.14\tclient5\n" >> /etc/hosts
EOF

$packages = <<EOF
echo "Installing extra packages"
yum update -y
yum install vim git rsync wget telnet bind-utils traceroute net-tools -y
setenforce 0
sed -i s/SELINUX=enforcing/SELINUX=disabled/g /etc/sysconfig/selinux
sed -i s/SELINUX=permissive/SELINUX=disabled/g /etc/sysconfig/selinux
EOF

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "geerlingguy/centos7"

  config.vm.box_url = "https://atlas.hashicorp.com/geerlingguy/boxes/centos7"

  config.vm.define :ansible do |ansible|
    config.vm.hostname = "ansible"
    ansible.vm.network "private_network", ip: "172.20.20.5"
    ansible.vm.provider "virtualbox" do |vb|
     vb.customize ["modifyvm", :id, "--memory", "512"]
     vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end
  end

  config.vm.define :client1 do |client1|
    config.vm.hostname = "client1"
    client1.vm.network "private_network", ip: "172.20.20.10"
    client1.vm.provider "virtualbox" do |vb|
     vb.customize ["modifyvm", :id, "--memory", "512"]
     vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end
  end

  config.vm.define :client2 do |client2|
    config.vm.hostname = "client2"
    client2.vm.network "private_network", ip: "172.20.20.11"
    client2.vm.provider "virtualbox" do |vb|
     vb.customize ["modifyvm", :id, "--memory", "512"]
     vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end
  end

  config.vm.define :client3 do |client3|
    config.vm.hostname = "client3"
    client3.vm.network "private_network", ip: "172.20.20.12"
    client3.vm.provider "virtualbox" do |vb|
     vb.customize ["modifyvm", :id, "--memory", "512"]
     vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end
  end

  config.vm.define :client4 do |client4|
    config.vm.hostname = "client4"
    client4.vm.network "private_network", ip: "172.20.20.13"
    client4.vm.provider "virtualbox" do |vb|
     vb.customize ["modifyvm", :id, "--memory", "512"]
     vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end
  end

  config.vm.define :client5 do |client5|
    config.vm.hostname = "client5"
    client5.vm.network "private_network", ip: "172.20.20.14"
    client5.vm.provider "virtualbox" do |vb|
     vb.customize ["modifyvm", :id, "--memory", "512"]
     vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end
  end

  config.vm.define :server do |server|
    config.vm.hostname = "server"
    server.vm.network "private_network", ip: "172.20.20.6"
    server.vm.network "forwarded_port", guest: 80, host: 8080
    server.vm.provider "virtualbox" do |vb|
     vb.customize ["modifyvm", :id, "--memory", "512"]
     vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end
  end

  hosts = ["ansible", "server", "client1", "client2", "client3", "client4", "client5"]
  hosts.each do |i|
    config.vm.define "#{i}" do |node|
        node.vm.provision "shell", inline: $hostnames
        node.vm.provision "shell", inline: $packages
    end
  end

  #config.vm.provision "ansible" do |ansible|
  #  ansible.playbook = "playbook.yml"
  #  ansible.sudo = true
  #  #ansible.verbose = 'vvv'
  #  ansible.host_key_checking = false
  #  ansible.groups = {
  #        "server" => "server",
  #        "client" => "client",
  #  }
  #end
end
