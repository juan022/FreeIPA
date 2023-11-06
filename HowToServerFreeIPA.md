# Guía de instalación y configuración de FreeIPA en CentOs

Previo a la insrtalación debemos asegurarnos que tenemos el sistema actualizado y la direccion IP estatica correctamente configurada.

# Paso 1: Instalación de FreeIPA

## Instalamos el paquete de FreeIPA
    sudo yum install ipa-server ipa-server-dns

## Configuramos el nombre de host para coincidir con nuestro dominio de FreeIPA
    sudo hostnamectl set-hostname ipa.jpsanchez.com

## Nos aseguramos de que el nombre de host está en /etc/hosts
    echo "192.168.1.100 ipa.jpsanchez.com ipa" | sudo tee -a /etc/hosts

## Instalamos FreeIPA con un servidor DNS integrado
    sudo ipa-server-install --setup-dns --noforwarders

Durante la instalación, se nos pedirá que proporcionemos una contraseña pa el administrador de IPA y otra para el Directory Manager.

# Paso 2: Configuración de Kerberos

Kerberos ya está integrado en FreeIPA, por lo que no se necesitan pasos para su configuración, más que seguir la guia que se nos presenta al configurar FreeIPA.

# Paso 3: Instalación y configuración de NFS

## Instalamos los paquetes necesarios para NFS
    sudo yum install nfs-utils

## Habilitamos y arrancamos el servicio de NFS
    sudo systemctl enable --now nfs-server rpcbind

## Configuramos los directorios de exportación
    sudo mkdir /home/gpfs
    echo "/home/gpfs *(rw,sync,no_subtree_check,sec=krb5p)" | sudo tee /etc/exports

## Exportamos los directorios y reiniciamos el servicio NFS
    sudo exportfs -rav
    sudo systemctl restart nfs-server

# Paso 4: Configuramos el Autofs

Autofs es un servicio que permite montar automáticamente sistemas de archivos bajo demanda, muy útil para montar directorios de incio de usuarios en un entorno como el que estamos preparando.

## Instalamos Autofs
    sudo yum install autofs

## Configuramos el archivo auto.master
    echo "/home /etc/auto.home" | sudo tee -a /etc/auto.master

## Creamos y configuramos el archivo auto.home
    echo "* -fstype=nfs,rw,sec=krb5p ipa.jpsanchez.com:home/gpfs/&" sudo tee /etc/auto.home

## Reinciamos el servicio autofs
    sudo systemctl enable --now autofs

# Paso 5: Verifición y pruebas

## Verificamos que FreeIPA está funcionando:
    kinit admin
    ipa user-show admin

## Creamos un nuevo usuario en FreeIPA y probamos el montaje automático:
    ipa user-add newuser --first=juan --last=sanchez --password

* Debemos configurar el cliente FreeIPA (HowToClientFreeIPA)

* Deberia funcionar, resumen:

## Desde nuestro cliente Linux (falta cliente Windows)
    sudo yum install ipa-client

## Configuramos el cliente FreeIPA uniendolo al dominio (servidor)
    sudo ipa-client-install --mkhomedir

## Verificamos el montaje del directorio home en el cliente
    su - juansanchez

Deberiamos ver que el directorio home del usuario se monta automáticamente, en caso contrario comproabar permisos.

Puntos ha tener en cuenta:

    firewalls

    Windows --> Active directory (¿SAMBA?)