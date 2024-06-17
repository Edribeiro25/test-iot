# test-iot
Vagrant.configure(2) do |config|
    common = <<-SHELL
    sudo yum -y install vim tree net-tools telnet git python3
    sudo echo "autocmd filetype yaml setlocal ai ts=2 sw=2 et" > /home/vagrant/.vimrc
    sudo echo "alias python=/usr/bin/python3" >> /home/vagrant/.bashrc
    sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/g' /etc/ssh/sshd_config
    sudo systemctl restart sshd
    SHELL
          config.vm.box = "centos/7"
          config.vm.box_url = "centos/7"

          config.vm.define "lletournS" do |control|
                  control.vm.hostname = "lletournS"
                  control.vm.network "private_network", ip: "192.168.56.110"
                  control.vm.provider "virtualbox" do |v|
                          v.customize [ "modifyvm", :id, "--cpus", "1"]
                          v.customize [ "modifyvm", :id, "--memory", "512"] 
                          v.customize [ "modifyvm", :id, "--natdnshostresolver1", "on"] 
                          v.customize [ "modifyvm", :id, "--natdnsproxy1", "on"] 
                          v.customize [ "modifyvm", :id, "--name", "lletournS"]
                  end
                  config.vm.provision :shell, :inline => common
          end

          config.vm.define "lletournSW" do |control|
                  control.vm.hostname = "lletournSW"
                  control.vm.network "private_network", ip: "192.168.56.111" 
                  control.vm.provider "virtualbox" do |v|
                          v.customize [ "modifyvm", :id, "--cpus", "1"] 
                          v.customize [ "modifyvm", :id, "--memory", "512"] 
                          v.customize [ "modifyvm", :id, "--natdnsresolver", "on"] 
                          v.customize [ "modifyvm", :id, "--natdnsproxy", "on"] 
                          v.customize [ "modifyvm", :id, "--name", "lletournSW"]
                  end
                  config.vm.provision :shell, :inline => common
          end

end
