#Prerrequisitos
- Instalar ubuntu server con IP estatica
- Instalar openssh durante la instalacion de ubuntu server
-------
- Una vez instalado ubuntu server configurar zora horaria, en mi caso: Europe/Madrid
	timedatectl
	sudo timedatectl set-timezone Europe/Madrid
------- (opcional)
- Si tienes el servidor montado en un portatil y quieres mantenerlo con la tapa cerrada:
	sudo nano /etc/systemd/logind.conf
- Descomentar las siguientes lineas:
	HandleLidSwitch=ignore
	HandleLidSwitchDocked=ignore
- Guardar y ejecutar:
	sudo systemctl restart systemd-logind
-------- 
- Pasamos a habilitar ssh para poder acceder desde tu pc personal al server
	- Primero comprobamos el estado de ssh en nuestro servidor, lo habilitamos y runeamos
		sudo systemctl status ssh
		sudo systemctl enable ssh
		sudo systemctl start ssh
	- Ahora habilitamos el firewall y abrimos el puerto 22 que es el de conexion ssh
		sudo ufw status
		sudo ufw enable
		sudo ufw allow 22/tcp
		sudo ufw status
	- Posteriormente, hacemos ping a ip del servidor desde nuestro pc para ver que tenemos conexion
		ping 192.168.100.151
	- Generamos una key ssh
		ssh-keygen -t rsa -b 4096 -C "tu_correo@example.com"
	- Le asignamos un nombre a la clave para poder copiarla 
		Enter file in which to save the key (C:\Users\xxxxxxx/.ssh/id_rsa): example
	- La copiamos:
		type example.pub | ssh nameserver@192.168.100.151 "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
	- Y entramos en nuestro servidor:
		ssh -i example nameserver@192.168.100.151
	- Aseguramos los permisos
		chmod 700 ~/.ssh
		chmod 600 ~/.ssh/authorized_keys
--------
- Verificar en que usuario estamos
	whoami
- Configurar sudo sin Contraseña
	sudo visudo
- Agrega esta línea al final del archivo:
    %sudo   ALL=(ALL:ALL) NOPASSWD:ALL
- Configurar SSH para Permitir Solo Tu Usuario
	echo "AllowUsers nameuser" | sudo tee -a /etc/ssh/sshd_config
- Para aplicar los cambios, reinicia el servicio SSH:
	sudo systemctl enable ssh
	sudo systemctl start ssh
- Instalar paquetes basicos
	sudo apt-get update && sudo apt-get install -y \
     apt-transport-https \
     ca-certificates \
     curl \
     gnupg2 \
     software-properties-common \
     vim \
     fail2ban \
     ntfs-3g
--------
- Instalar docker
	curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/docker.gpg
	gpg --show-keys --with-fingerprint /etc/apt/trusted.gpg.d/docker.gpg
	echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
	sudo apt-get update
	sudo apt-get install -y --no-install-recommends docker-ce docker-compose
- Agregar tu usuario al grupo docker
	sudo usermod -a -G docker nameuser
--------
- Pasamos a crear los contenedores
	mkdir contenedores
	cd contenedores
- Primero creamos la carpeta duckdns para configurar nuestro dns
	- Vamos a la web de duckdns.org y seguimos los pasos de instalacion despues de haber creado una cuenta


 -------------  continuará...
	
