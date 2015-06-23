# -*- mode: ruby -*-
# vi: set ft=ruby sw=2 :

Vagrant.require_version ">= 1.6.2"

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

CONTROL_IP = "172.16.255.250"
NODE_IP = "172.16.255.251"

ENV['VAGRANT_DEFAULT_PROVIDER'] = 'virtualbox'

$bootstrap_common = <<EOF
mkdir -p /etc/flocker
cp /home/vagrant/credentials/cluster.crt /etc/flocker/cluster.crt
EOF

$bootstrap_control = <<EOF
cp /home/vagrant/credentials/control-#{CONTROL_IP}.crt /etc/flocker/control-service.crt
cp /home/vagrant/credentials/control-#{CONTROL_IP}.key /etc/flocker/control-service.key
systemctl enable flocker-control
systemctl start flocker-control
EOF

$bootstrap_node1_cert = <<EOF
cp /home/vagrant/credentials/node-1.crt /etc/flocker/node.crt
cp /home/vagrant/credentials/node-1.key /etc/flocker/node.key
EOF

$bootstrap_node2_cert = <<EOF
cp /home/vagrant/credentials/node-2.crt /etc/flocker/node.crt
cp /home/vagrant/credentials/node-2.key /etc/flocker/node.key
EOF

$bootstrap_node = <<EOF
cat <<CONFIG > /etc/flocker/agent.yml
version: 1
control-service:
  hostname: "#{CONTROL_IP}"
dataset:
  backend: "zfs"
CONFIG
systemctl enable flocker-dataset-agent
systemctl start flocker-dataset-agent
systemctl enable flocker-container-agent
systemctl start flocker-container-agent
EOF

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "clusterhq/flocker-tutorial"
  config.vm.box_url = "https://clusterhq-archive.s3.amazonaws.com/vagrant/flocker-tutorial.json"
  config.vm.box_version = "= 1.0.0"

  config.vm.define "node1" do |node1|
    node1.vm.network :private_network, :ip => CONTROL_IP
    node1.vm.hostname = "node1"
    node1.vm.provision "shell", inline: $bootstrap_common, privileged: true
    node1.vm.provision "shell", inline: $bootstrap_control, privileged: true
    node1.vm.provision "shell", inline: $bootstrap_node1_cert, privileged: true
    node1.vm.provision "shell", inline: $bootstrap_node, privileged: true
  end

  config.vm.define "node2" do |node2|
    node2.vm.network :private_network, :ip => NODE_IP
    node2.vm.hostname = "node2"
    node2.vm.provision "shell", inline: $bootstrap_common, privileged: true
    node2.vm.provision "shell", inline: $bootstrap_node2_cert, privileged: true
    node2.vm.provision "shell", inline: $bootstrap_node, privileged: true
  end

  # Don't use a shared folder.
  config.vm.synced_folder ".", "/vagrant", disabled: true
end
