Vagrant.configure("2") do |config|
	config.vm.box = "ubuntu/focal64"
 	config.vm.provider "virtualbox" do |v|
		v.memory = 1024
		v.cpus = 1
		v.name = "Server DB"
	end
  	config.vm.hostname = "star-db"
        config.vm.network "public_network", bridge: "<interface>", ip: "<ip>"   
        config.vm.network "forwarded_port", guest: 3306, host: 3306
        
        # Install e config mariaDB 
        $script = <<-SCRIPT
        sudo apt-get update
        sudo apt-get upgrade

        sudo apt-get install software-properties-common -y
        sudo apt-key adv --fetch-keys 'https://mariadb.org/mariadb_release_signing_key.asc'
        sudo add-apt-repository 'deb [arch=amd64,arm64,ppc64el] https://mirror1.cl.netactuate.com/mariadb/repo/10.5/ubuntu focal main'
        sudo apt-get update
        sudo apt-get install mariadb-server -y

        
        sudo mysqladmin -u root password 'DEV@DEV'
        sudo mysqladmin flush-privileges

        
        sudo sed -i 's/bind-address            = 127.0.0.1/bind-address = 0.0.0.0/g' /etc/mysql/mariadb.conf.d/50-server.cnf
        sudo systemctl restart mysql.service

        
        SQL="create user 'devEnv'@'%' identified by 'DEV@ENV'; Grant all privileges on *.* to 'devEnv'; FLUSH PRIVILEGES;"
        mysql -u root -p'DEV@DEV' -e"$SQL" mysql
        SCRIPT
        config.vm.provision "shell", inline: $script	
	config.disksize.size = "50GB"  	
end
