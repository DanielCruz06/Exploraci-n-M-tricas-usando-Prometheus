#Instalación de Node Exporter
#Primer paso es actualizar los paquetes
sudo apt update
sudo apt upgrade

#NOTA: La instalación anterior arrojo un error al crear un servicio local 
#Se opta por el siguiente paso de instalación:

wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz
--tar xvfz node_exporter-1.5.0.linux-amd64.tar.gz
sudo mv node_exporter-1.5.0.linux-amd64/node_exporter /usr/local/bin


sudo useradd -rs /bin/false node_exporter
sudo vi /etc/systemd/system/node_exporter.service
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target

sudo systemctl enable node_exporter

sudo systemctl daemon-reload
sudo systemctl start node_exporter
sudo systemctl status node_exporter

#Configurar Prometheus to Monitor Cliente Nodes

sudo vi /etc/prometheus/prometheus.yml

Se modifica el archivo prometheus.yml
[!ALT](/prometheyusYML.png)


