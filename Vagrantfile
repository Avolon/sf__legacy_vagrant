# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Указываем использование образа Ubuntu Bionic 64
  config.vm.box = "vagrant.box"
  # Настройки провайдера libvirt
#  config.vm.provider :libvirt do |libvirt|
 #   libvirt.driver = "kvm"  # Используем KVM в качестве драйвера
  #  libvirt.host = "localhost"  # Хост libvirt
#    libvirt.qemu_use_session = false
   # libvirt.connect_via_ssh = false  # ПОтключаем подключение через SSH
  #end

  # Указываем провижионер для установки и настройки PostgreSQL 8.4
  config.vm.provision "shell", inline: <<-SHELL
    # Добавляем репозиторий с PostgreSQL 8.4
  sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
  wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add - 
   sudo apt-get update
    # Устанавливаем PostgreSQL и пакеты, необходимые для его работы
    sudo apt-get install -y postgresql postgresql-client postgresql-contrib
    # Включаем сервис PostgreSQL и настраиваем автозапуск при загрузке
   sudo systemctl enable postgresql
#    sudo update-rc.d postgresql defaults
    sudo service postgresql start

    # Настраиваем доступ к PostgreSQL извне
    sudo sed -i "s/#listen_addresses = 'localhost'/listen_addresses = '*'/g" /etc/postgresql/10/main/postgresql.conf
    echo "host all all 0.0.0.0/0 md5" | sudo tee -a /etc/postgresql/10/main/pg_hba.conf
    sudo service postgresql restart
  SHELL

  # Проброс порта для доступа к PostgreSQL извне
  config.vm.network "forwarded_port", guest: 5433, host: 5432, host_ip: "127.0.0.1"
end
