# FreeIPA: Componentes y Funcionamiento

FreeIPA combina múltiples componentes de seguridad y directorio bajo una única interfaz de gestión. Los principales son LDAP, kerberos y NFS.

(A continuación se explica como "deberia" funcionar por lo bajo en el proyecto)

## LDAP (Lightweight Directory Access Protocol)

### Funcionamiento:

- **Estructura Jerárquica:** 
  Los datos en LDAP se organizan en un árbol jerárquico, facilitando la búsqueda y el acceso.

- **Autenticación y Autorización:** 
  Los clientes consultan al servidor LDAP para verificar credenciales y derechos de acceso.

## Kerberos

### Funcionamiento:

- **Tickets de Seguridad:** 
  Kerberos utiliza tickets para comunicaciones seguras en redes no seguras y es esencial para la autenticación en FreeIPA.

- **Proceso de Autenticación:** 
  1. **AS Request:** El usuario solicita un ticket al Servicio de Autenticación (AS).
  2. **TGT:** Se recibe un Ticket de Concesión de Tickets (TGT) tras la verificación de identidad.
  3. **TGS Request:** Con el TGT, se solicita un ticket de servicio al TGS para acceder a servicios como NFS.
  4. **Service Ticket:** El TGS emite un ticket que permite al usuario autenticarse ante el servicio deseado.

## NFS (Network File System)

### Funcionamiento:

- **Compartición de Archivos en Red:** 
  NFS permite el acceso a archivos sobre una red de manera similar a los almacenados localmente.

- **Directorios Home en Servidores Remotos:**
  1. **Montaje:** El sistema cliente monta el directorio home del usuario desde el servidor NFS al iniciar sesión.
  2. **Autofs:** Autofs monta automáticamente el directorio home y lo desmonta al finalizar la sesión.

- **Integración con Kerberos:** 
  NFS usa Kerberos para asegurar que solo los usuarios autenticados accedan a sus archivos.

## Integración y Flujo de Trabajo

1. **Solicitud de Inicio de Sesión:** 
   El cliente envía una solicitud de inicio de sesión al servidor FreeIPA.

2. **Autenticación LDAP:** 
   FreeIPA verifica al usuario a través de LDAP.

3. **Autenticación Kerberos:** 
   Kerberos autentica al usuario y emite los tickets necesarios.

4. **Acceso a NFS:** 
   El cliente monta el directorio home del usuario con permisos de Kerberos.

5. **Montaje Automático con Autofs:** 
   Autofs se encarga del montaje y desmontaje del directorio home.

La integración de estos servicios facilita la administración de la seguridad y las políticas en la organización.