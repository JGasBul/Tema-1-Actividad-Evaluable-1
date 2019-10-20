# Tarea Pepe Gascó Bule.
# Este texto es el VagrantFile.
# Lo que hace es que se creen en la carpeta Vagrant una carpeta llamada apache en las que se creen dos carpetas llamadas www y sites-available.
# Gracias a esto, en la maquina solo se tendria que modificar el archivo /etc/hosts. El resto se puede hacer tranquilamente en nuestro windows ya que las carpetas estan vinculadas con la maquina.

Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/precise32"
  config.vm.box_version = "1.0.0"

  config.vm.network "forwarded_port", guest: 80, host: 8081
  config.vm.network "public_network", ip: "192.168.33.10"
  
  config.vm.provision "shell", inline: <<-SHELL
    apt-get -y update
    apt-get install -y apache2
    apt-get install -y elinks
    mkdir /vagrant/apache
    mkdir /vagrant/apache/www 
    cp /var/www/index.html /vagrant/apache/www/ 
    sudo rm -r /var/www/ 
    sudo ln -s /vagrant/apache/www/ /var/ 
    cp -r /etc/apache2/sites-available/ /vagrant/apache/
    sudo rm -r /etc/apache2/sites-available/
    sudo ln -s /vagrant/apache/sites-available/ /etc/apache2/
  SHELL
end
