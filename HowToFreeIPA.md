# HowTo: Manejo Básico de FreeIPA

## Introducción
How To guía para la administración básica de un servidor FreeIPA. Cubrirá cómo manejar usuarios, grupos...

## Requisitos Previos

Tener acceso al servidor FreeIPA con privilegios de administración.

Tener instalada la línea de comandos ipa.

## A. Gestión de Usuarios
### A.1 Crear un Nuevo Usuario
    ipa user-add username --first=nombre --last=apellido --password

Este comando crea un nuevo usuario en FreeIPA con un nombre de usuario username y establece su nombre y apellido. Se solicitará establecer una contraseña después de ejecutar el comando.

### A.2 Modificar un Usuario Existente
    ipa user-mod username --shell=/bin/bash

Modifica un usuario existente, por ejemplo, para cambiar su shell por defecto a /bin/bash.

### A.3 Deshabilitar un Usuario
    ipa user-disable username

Deshabilita la cuenta de usuario especificada.

### A.4 Habilitar un Usuario
    ipa user-enable username

Habilita la cuenta de usuario especificada.

### A.5 Establecer Contraseña de Usuario
    ipa passwd username

Cambia la contraseña del usuario. Se pedirá que introduzca la nueva contraseña dos veces.

## B. Gestión de Grupos
### B.1 Crear un Nuevo Grupo
    ipa group-add groupname --desc="Descripción del grupo"

Crea un nuevo grupo con una descripción.

### B.2 Añadir Usuario a un Grupo
    ipa group-add-member groupname --users=username

Añade un usuario existente a un grupo.

### B.3 Eliminar Usuario de un Grupo
    ipa group-remove-member groupname --users=username

Elimina un usuario de un grupo.

### B.4 Modificar un Grupo Existente
    ipa group-mod groupname --desc="Nueva descripción"

Modifica la descripción de un grupo existente.

## C. Gestión de Políticas de Contraseñas
### C.1 Crear una Nueva Política de Contraseñas
    ipa pwpolicy-add --group=groupname --minlen=8 --maxfail=3

Crea una nueva política de contraseñas para un grupo específico con una longitud mínima y un número máximo de intentos fallidos.

### C.2 Modificar una Política de Contraseñas
    ipa pwpolicy-mod --group=groupname --minlife=1 --maxlife=365

Modifica la política de contraseñas de un grupo para establecer la vida mínima y máxima de las contraseñas en días.

### C.3 Mostrar Política de Contraseñas
    ipa pwpolicy-show --group=groupname

Muestra la política de contraseñas actual para un grupo.

## D. Gestión de politicas de host
    ipa host-add hostname --description="descripcion del host"

Registra un nuevo host en el dominio de FreeIPA.