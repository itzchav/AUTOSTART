# AUTOSTART

## Configuraciones iniciales 
1. Dirijirse a la Pagina de [NVIDIA](https://developer.nvidia.com/embedded/jetpack-archive) para desargar el jetpack.
 
   Hay diversas caracteristicas que se deben tomar encuenta al elegir el Jetpack como la versión de Ubuntu que se tenga instalada en la computadora .

    En nuestro caso tenemos instalado Ubuntu 18.04 en la computadora por lo cual instalaremos[Jetpack 3.3.3](https://developer.nvidia.com/embedded/jetpack-3_3_3)


5. Grabar la imagen en la SD.

6. Posteriormente se coloca la SD en la jetson, enseguida se conecta la Jetson a la fuente alimentación.

   
7. Instalar los drives para WIFI  "rtl88x2bu".  

8. Instalar ROS en la Jetson.





## Configuraciones en la Jetson para el autostart

Una vez conectada la Jetson a la fuente de alimentación, se ejecuta en terminal

```
sudo touch /usr/local/bin/autostart.sh
sudo chmod +x /usr/local/bin/autostart.sh
cd /etc/systemd/system
sudo touch autostart.service
sudo gedit autostart.service 

```
Ahora se modificara un archivo, ingresamos a la siguiente ruta  

```
cd /etc/systemd/system
```
Se agrega el siguiente codigo al documento que se abrio previamente
```
[Unit]
Description="Robot Service"
#After=multi-user.target

[Service]
Type=simple
ExecStart=/usr/local/bin/autostart.sh
Restart=on-failure
#StartLimitInterval=10
RestartSec=10
KillMode=process
User=robotica

[Install]
WantedBy=multi-user.target
```

Una vez configurado el scrip nos dirijimos a la raiz de la terminal y modificaremos el archivo autostart

```
sudo gedit /usr/local/bin/autostart.sh
```
se copian las siguientes lineas

```
#!/bin/bash
passord='robotica'
source /opt/ros/melodic/setup.bash
source ~/Turtlebot2/devel/setup.bash
export ROS_MASTER_URI=http://192.168.43.178:11311
export ROS_IP=192.168.43.178
sleep 15

echo $passwor | sudo -S chmod 777 /dev/ttyUSB0 

sleep 15

roscore &
sleep 15

roslaunch rplidar_ros rplidar_a1.launch &
sleep 15

roslaunch turtlebot_bringup minimal.launch & 
```
Se aguardan los cambios y se ejecutan los siguientes comandos

```
sudo systemctl enable autostart.service 
sudo systemctl start  autostart.service 
sudo systemctl status  autostart.service 
sudo systemctl daemon-reload
sudo systemctl restart  autostart.service

```
Se cierra la ventana de comenandos se apaga la Jetson para realizar la prueba del autostart.

## Prueba de funcionamiento

Inicialmente se tiene al Turtleboot apagado, la Jetson desconectada 

1. Se conecta el Turtlebbot
2. Se conecta la Jetson
3. Se debe de escuchar un tono tres veces consecutivas indicando que este levanto el roscore(ESO NOS INDICA QUE EL AUTOSATART SE CONFIGURO CORRECTAMENTE)



# Observaciones durante el proceso 

Para conectar la pantalla a la Jetson(conectar sin conversores, Direccto a la HMI(1 cable) )


*********************************************
