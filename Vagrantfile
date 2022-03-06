
Vagrant.configure("2") do |config|

	config.vm.box = "geerlingguy/ubuntu1804"
	config.vm.network "public_network", ip: "192.168.0.15"

	config.vm.provider "virtualbox" do |vb|
		vb.memory = 4096
		vb.cpus = 3
	end

	config.vm.provision "shell", inline: <<-SHELL
		
		#Requirements
		sudo apt-get update -y
		sudo apt-get install make
		sudo apt-get install gcc -y	
		
		sudo apt-get update -y
		sudo apt-get install -y libreadline-dev
		sudo apt-get install zlib1g-dev
		sudo apt-get install bison -y

		#Download postgres
		wget https://ftp.postgresql.org/pub/source/v8.4.22/postgresql-8.4.22.tar.gz
		tar xfz postgresql-8.4.22.tar.gz
		cd postgresql-8.4.22

		#Installation
		./configure
		make
		su
		make install
		pass=$(perl -e 'print crypt("infodba", "password")' $password)
		echo $pass
		useradd -m -p "$pass" "postgres"
		
		sudo mkdir /usr/local/pgsql/data
		sudo chown postgres /usr/local/pgsql/data
		
		sudo su
		cd postgresql-8.4.22
		
		su --login -c "/usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data" postgres
		su --login -c "/usr/local/pgsql/bin/psql --version" postgres

	SHELL
end
