Vagrant.configure("2") do |config|
  # Configuración para DNSA (Servidor Maestro)
  config.vm.define "DNSA" do |dnsa|
    dnsa.vm.box = "debian/bookworm64"
    dnsa.vm.network "private_network", ip: "192.168.57.10"
    dnsa.vm.hostname = "dnsa"

    # Sincronizar la carpeta de configuración
    dnsa.vm.synced_folder "./config_dns", "/vagrant/config_dns", owner: "vagrant", group: "vagrant"

    dnsa.vm.provision "shell", inline: <<-SHELL
      apt-get update -y
      apt-get install -y bind9 bind9utils bind9-doc

      # Copiar archivo de configuración específico para DNSA
      cp /vagrant/config_dns/named.conf.local.dnsa /etc/bind/named.conf.local

      # Copiar archivos de zona
      cp /vagrant/config_dns/db.ies.test /etc/bind/db.ies.test
      cp /vagrant/config_dns/db.informatica.ies.test /etc/bind/db.informatica.ies.test
      cp /vagrant/config_dns/db.aulas.ies.test /etc/bind/db.aulas.ies.test
      cp /vagrant/config_dns/db.departamentos.ies.test /etc/bind/db.departamentos.ies.test
      cp /vagrant/config_dns/db.192.168.57 /etc/bind/db.192.168.57

      # Reiniciar bind9 y verificar estado
      systemctl enable bind9
      systemctl restart bind9
      systemctl status bind9
    SHELL
  end

  # Configuración para DNSB (Servidor Esclavo)
  config.vm.define "DNSB" do |dnsb|
    dnsb.vm.box = "debian/bookworm64"
    dnsb.vm.network "private_network", ip: "192.168.57.20"
    dnsb.vm.hostname = "dnsb"

    # Sincronizar la carpeta de configuración
    dnsb.vm.synced_folder "./config_dns", "/vagrant/config_dns", owner: "vagrant", group: "vagrant"

    dnsb.vm.provision "shell", inline: <<-SHELL
      apt-get update -y
      apt-get install -y bind9 bind9utils bind9-doc

      # Copiar archivo de configuración específico para DNSB
      cp /vagrant/config_dns/named.conf.local.dnsb /etc/bind/named.conf.local

      # Reiniciar bind9 y verificar estado
      systemctl enable bind9
      systemctl restart bind9
      systemctl status bind9
    SHELL
  end
end
