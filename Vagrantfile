Vagrant.configure("2") do |config|

  # Ubuntu 20.04
  config.vm.box = "ubuntu/focal64"
  config.vm.hostname = "dev"
  config.vm.box_check_update = false

  config.vm.network "public_network",
     :mac => "0022334455DA"

  config.vm.provider "virtualbox" do |vb|
    vb.cpus = 1
    vb.memory = 2024
  end

  config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/me.pub"
  config.vm.provision "file", source: "~/.ssh/id_rsa", destination: "~/.ssh/me.private"
  config.vm.provision "shell", inline: <<-SHELL
    cat /home/vagrant/.ssh/me.pub >> /root/.ssh/authorized_keys
    cat /home/vagrant/.ssh/me.pub >> /home/vagrant/.ssh/authorized_keys
    mv /home/vagrant/.ssh/me.pub /home/vagrant/.ssh/id_rsa.pub
    mv /home/vagrant/.ssh/me.private /home/vagrant/.ssh/id_rsa
    chmod 400 /home/vagrant/.ssh/id_rsa*
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    useradd -m -s /bin/bash daniel
    echo daniel:calella | chpasswd
    usermod -aG sudo daniel

    rsync -r /home/vagrant/.ssh /home/daniel
    chown -R daniel:daniel /home/daniel/.ssh
  SHELL
  
end

=begin
config.vm.provision "shell", inline: <<-SHELL
sudo apt update
sudo apt install net-tools -y 
sudo ifconfig enp0s3 down
SHELL

sudo apt update
sudo apt install network-manager -y

=end