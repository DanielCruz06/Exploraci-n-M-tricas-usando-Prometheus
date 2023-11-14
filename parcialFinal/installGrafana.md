#Como primer paso herramientas necesarias para la instalación 
sudo apt-get install -y apt-transport-https software-properties-common
#Importamos Grafana GPG key
sudo wget -q -O /usr/share/keyrings/grafana.key https://apt.grafana.com/gpg.key
#Add the Grafana 
echo "deb [signed-by=/usr/share/keyrings/grafana.key] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
#Update the packages in the repository, including the new Grafana packages
sudo apt-get update
#Install the open-source version of Grafana 
--open source sudo apt-get install grafana
--enterprise sudo apt-get install grafana-enterprise
#Reload the systemctl deamon
sudo systemctl daemon-reload
#Enable and start the Grafana server. Using systemctl enable configures the server to launch Grafana when the system boots.
sudo systemctl enable grafana-server.service
sudo systemctl start grafana-server
#Verify the status 
sudo systemctl status grafana-server

#=========================================================================
#=========================================================================
#=========================================================================
#Integración
#Ingresamos a la dirección http:192.168.33.10:3000
cambiamos la contraseña 
user: admin
pwd:  admin

#Segundo paso vamos hacía Data source 
[!ALT](/integrations1.png)
