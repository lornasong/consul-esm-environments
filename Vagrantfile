# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<SCRIPT

echo "Installing dependencies ..."
sudo apt-get update
sudo apt-get install -y unzip curl jq dnsutils

echo "Determining Consul version to install ..."
CHECKPOINT_URL="https://checkpoint-api.hashicorp.com/v1/check"
if [ -z "$CONSUL_VERSION" ]; then
    CONSUL_VERSION=$(curl -s "${CHECKPOINT_URL}"/consul | jq .current_version | tr -d '"')
fi

echo "Fetching Consul version ${CONSUL_VERSION} ..."
cd /tmp/
curl -s https://releases.hashicorp.com/consul/${CONSUL_VERSION}/consul_${CONSUL_VERSION}_linux_amd64.zip -o consul.zip

echo "Installing Consul version ${CONSUL_VERSION} ..."
unzip consul.zip
sudo chmod +x consul
sudo mv consul /usr/bin/consul

sudo mkdir /etc/consul.d
sudo chmod a+w /etc/consul.d

## CONSUL_ESM

echo "Determining Consul-ESM version to install ..."
if [ -z "$CONSUL_ESM_VERSION" ]; then
    CONSUL_ESM_VERSION=0.3.3
fi

echo "Fetching Consul-ESM version ${CONSUL_ESM_VERSION} ..."

cd /tmp/
curl -s https://releases.hashicorp.com/consul-esm/${CONSUL_ESM_VERSION}/consul-esm_${CONSUL_ESM_VERSION}_linux_amd64.zip -o consul-esm.zip

echo "Installing Consul-ESM version ${CONSUL_ESM_VERSION} ..."
unzip consul-esm.zip
sudo chmod +x consul-esm
sudo mv consul-esm /usr/bin/consul-esm

SCRIPT

# Specify a Consul version
CONSUL_VERSION = ENV['CONSUL_VERSION']
CONSUL_ESM_VERSION = ENV['CONSUL_ESM_VERSION']

# Specify a custom Vagrant box for the demo
DEMO_BOX_NAME = ENV['DEMO_BOX_NAME'] || "debian/stretch64"

# Vagrantfile API/syntax version.
# NB: Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = DEMO_BOX_NAME

  config.vm.provision "shell",
                          inline: $script,
                          env: {'CONSUL_VERSION' => CONSUL_VERSION, 'CONSUL_ESM_VERSION' => CONSUL_ESM_VERSION}

  config.vm.define "n1" do |n1|
      n1.vm.hostname = "n1"
      n1.vm.network "private_network", ip: "172.20.20.10"
  end

  config.vm.define "n2" do |n2|
      n2.vm.hostname = "n2"
      n2.vm.network "private_network", ip: "172.20.20.11"
  end

  config.vm.define "n3" do |n3|
      n3.vm.hostname = "n3"
      n3.vm.network "private_network", ip: "172.20.20.12"
  end

end
