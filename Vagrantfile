VAGRANT_API_VERSION = "2"
Vagrant.configure(VAGRANT_API_VERSION) do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.hostname = "open5gsinbox" 
  config.vm.network "private_network", ip:"192.168.56.101"
  config.vm.synced_folder "data" , "/home/vagrant/data"
  config.vm.synced_folder "open5gs_config" , "/etc/open5gs/"
  config.vm.network "forwarded_port", guest: 3000, host: 8080

  config.vm.provider "VirtualBox" do |vb|
    # Display the VirtualBox GUI when booting the machine
     vb.gui = true
     vb.cpu = 2    
     vb.memory = "2048"
   end
   config.vm.provision "shell", inline: <<-SHELL
     apt-get -y update
     apt-get install -y gnupg wget tcpdump 
     wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc > key.asc 
     sudo apt-key add key.asc
     echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
     apt-get -y update
     apt install -y mongodb-org
     sudo systemctl start mongod
     sudo systemctl enable mongod
     add-apt-repository ppa:open5gs/latest
     apt-get install -y software-properties-common
     apt-get -y update
     apt install -y open5gs
     #sudo apt install libc6=2.31-0ubuntu9.2 libc-bin=2.31-0ubuntu9.2 - To be used if getting error for nodejs

     curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
     sudo apt install -y nodejs
     mkdir -p /usr/lib/node_modules/open5gs
     sudo curl -fsSL https://open5gs.org/open5gs/assets/webui/install | sudo -E bash -
     sudo cp /home/vagrant/data/open5gs_webui.service /lib/systemd/system/open5gs-webui.service
     sudo systemctl daemon-reload
     sudo systemctl restart open5gs-webui
      SHELL
end