Vagrant.configure("2") do |config|
	config.vm.box = "ubuntu/focal64"
 	config.vm.provider "virtualbox" do |v|
		v.memory = 1024
		v.cpus = 1
		v.name = "Server DB"
	end
  	
	config.vm.define :mariadbldi do |mariadbldi|
		config.vm.hostname = "mariadbldi.ldi.com"
        	config.vm.network :private_network, ip: "192.168.68.200"
        end
        # Install e config mariaDB 
        $script = <<-SCRIPT
        sudo apt-get update
        sudo apt-get upgrade

        sudo apt-get install software-properties-common -y
        sudo apt-key adv --fetch-keys 'https://mariadb.org/mariadb_release_signing_key.asc'
        sudo add-apt-repository 'deb [arch=amd64,arm64,ppc64el] https://mirror1.cl.netactuate.com/mariadb/repo/10.5/ubuntu focal main'
        sudo apt-get update
        sudo apt-get install mariadb-server -y

        
        sudo mysqladmin -u root password 'Ldi2019*'
        sudo mysqladmin flush-privileges

        
        sudo sed -i 's/bind-address            = 127.0.0.1/bind-address = 0.0.0.0/g' /etc/mysql/mariadb.conf.d/50-server.cnf
        sudo systemctl restart mysql.service

        
        SQL="create user 'devEnv'@'%' identified by 'Ldi2019*'; Grant all privileges on *.* to 'devEnv'; FLUSH PRIVILEGES;"
        mysql -u root -p'Ldi2019*' -e"$SQL" mysql
        SCRIPT
        config.vm.provision "shell", inline: $script	
end
