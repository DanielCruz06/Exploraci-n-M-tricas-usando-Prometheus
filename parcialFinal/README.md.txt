#===========Parcial final
#===========Instalación de bento/ubuntu-22.04
#-----------------Comandos importanes para distribución Ubuntu
#Comando puertos abiertos 
ss -tuln
nano text1.txt
#Guardar cambios
Ctrl o
#Salir 
Ctrl X
#Comando para mirar Ip de la maquina
ip addr show

#===========Pasos instalación:
mkdir proyectoFinal
cd proyectoFinal
#Comando vagrant para realizar la instalación de la maquina con vagrant
vagrant init bento/ubuntu-22.04
#-----VagrantFinal
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Especifica la caja a usar
  config.vm.box = "bento/ubuntu-22.04"

  # Configuración de la red
  config.vm.network "private_network", ip: "192.168.33.10"

  # Configuración para el proveedor VirtualBox
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1500"
    vb.cpus = 2
  end
end

#-----Comando para verificar el estado de firewall
sudo ufw status
root@vagrant:~# ufw status
Status: inactive
#root@vagrant:~# sudo ufw status numbered
#Status: inactive
#----------sudo ufw disable
sudo systemctl disable ufw
sudo ufw status
sudo reboot
#=====================================================
#=====================================================
#====================Prometheus=======================
#Primer paso
--sudo apt update

#Segundo paso Creamos usuarios para Prometheus
--sudo groupadd --system prometheus
--sudo useradd -s /sbin/nologin --system -g prometheus prometheus

#Tercer paso creamos los directorios Prometheus
--sudo mkdir /etc/prometheus
--sudo mkdir /var/lib/prometheus

#Cuarto paso Descargar Prometheus y extracción de archivos
--wget https://github.com/prometheus/prometheus/releases/download/v2.43.0/prometheus-2.43.0.linux-amd64.tar.gz
--tar vxf prometheus*.tar.gz

#Quinto paso Navegar en el directorio de Prometheus
cd prometheus*/
#--Configuración Prometheus on Ubunto 22.04

#1.Mover los archivos binarios y tomar permisos
sudo mv prometheus /usr/local/bin
sudo mv promtool /usr/local/bin
sudo chown prometheus:prometheus /usr/local/bin/prometheus
sudo chown prometheus:prometheus /usr/local/bin/promtool

#2.Mover configuración de archivos 
sudo mv consoles /etc/prometheus
sudo mv console_libraries /etc/prometheus
sudo mv prometheus.yml /etc/prometheus

sudo chown prometheus:prometheus /etc/prometheus
sudo chown -R prometheus:prometheus /etc/prometheus/consoles
sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries
sudo chown -R prometheus:prometheus /var/lib/prometheus

#Comando para el ingreso a prometheus.yml
#El archivo prometheus.yml es el principal archivo de configuración de #Prometheus. Incluye la configuración de los objetivos que se van a supervisar, #la frecuencia de extracción de datos, el procesamiento de datos y el #almacenamiento. Puede establecer reglas de alerta y condiciones de notificación #en el archivo. No es necesario modificar este archivo para esta demostración, #pero no dude en abrirlo en un editor para echar un vistazo más de cerca a su #contenido.
sudo nano /etc/prometheus/prometheus.yml

#3.Creamos el servicio Prometheus en el sistema 
#Es necesario crear un archivo de servicio del sistema para Prometheus. Cree y #abra un archivo prometheus.service con el editor de texto Nano utilizando
sudo nano /etc/systemd/system/prometheus.service
#Incluir los siguientes comandos de configuración
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
#4.Reiniciar el sistema
sudo systemctl daemon-reload

#5.Iniciar el servicio Prometheus
sudo systemctl enable prometheus
sudo systemctl start prometheus

#6.Revisar estatus Prometheus
--sudo systemctl status prometheus
#Acceder a web interface
#Prometheus corre sobre el puerto 9090 por defecto, necesitaras tener habilitado #este puerto en tu firewall, para poder usar este comando
sudo ufw allow 9090/tcp
#Una vez iniciado el servicio y con el comando de del puerto 9090
#Se puede ingresar usando el navegador con la siguiente dirección localhost:9090 
# O usando la ip_mv:9090


#=======================================================================
#=======================================================================
#=========================PRUEBAS PROMETHEUS============================

1. Verifica la Configuración de Prometheus
Antes de comenzar con las pruebas, asegúrate de que Prometheus está configurado para escuchar en la dirección y el puerto correctos. En el archivo de configuración de Prometheus (/etc/prometheus/prometheus.yml), la línea --web.listen-address=0.0.0.0:9090 debería hacer que Prometheus escuche en todos los interfaces de red en el puerto 9090.


https://prometheus.io/docs/prometheus/latest/getting_started/




