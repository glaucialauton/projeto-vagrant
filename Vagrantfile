$script_mysql = <<-SCRIPT
	apt-get update && \
	apt-get install -y mysql-server-5.7 && \
	mysql -e "create user 'phpuser'@'%' identified by 'pass';"
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"

  config.vm.define "mysqldb" do |mysql|
    mysql.vm.network "private_network", ip: "192.168.50.4"
  
    mysql.vm.synced_folder "./configs", "/configs"  
    mysql.vm.synced_folder ".", "/vagrant", disabled: true	

    mysql.vm.provision "shell", 
    inline: "cat /configs/id_bionic.pub >> .ssh/authorized_keys"
    
    mysql.vm.provision "shell", inline: $script_mysql  

    mysql.vm.provision "shell", 
     inline: "cat /configs/mysqld.conf > /etc/mysql/mysql.conf.d/mysqld.cnf"
  
    mysql.vm.provision "shell", 
     inline: "service mysql restart"
  
  end

  config.vm.define "phpweb" do |phpweb|
    phpweb.vm.network "private_network", ip: "192.168.50.6"
    
  end

end
