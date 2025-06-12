PROYECTO: INSTALACIÓN Y TAREAS BÁSICAS EN SERVIDOR LDAP
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

RESUMEN

Este proyecto consiste en la instalación, configuración y prueba de un servidor LDAP (OpenLDAP) en una máquina Ubuntu/Zorin OS. Se crea una estructura organizativa básica, se agregan usuarios al directorio, se realizan búsquedas, modificaciones y eliminaciones, y se generan contraseñas cifradas. El objetivo es familiarizarse con tareas básicas de gestión de un servidor de directorio centralizado, útil para autenticar usuarios o centralizar información en una red empresarial.

OBJETIVOS

Instalar y configurar un servidor LDAP (slapd).

Crear una estructura organizativa dentro del árbol LDAP.

Añadir, modificar, buscar y eliminar usuarios.

Comprender la base de funcionamiento de un directorio y tareas básicas de administración.

Prepararse para responder preguntas de entrevistas sobre LDAP.

TECNOLOGÍAS UTILIZADAS

Sistemas Operativos: Ubuntu/Zorin OS

Servicios: slapd (OpenLDAP), ldap-utils

Redes: Comunicación local con IPs internas o localhost

Seguridad: Autenticación con contraseña y contraseñas cifradas (SSHA)

Archivos de configuración: LDIF para definición de entradas

PREPARACIÓN DEL ENTORNO

Crear una máquina virtual o servidor Ubuntu/Zorin OS.

Asignar IP estática o usar localhost.

Asegurar conectividad de red mínima.

INSTALACIÓN DEL SERVIDOR LDAP (SLAPD)

Ejecutar:
sudo apt update

sudo apt install slapd ldap-utils

Si no aparece el asistente, ejecutar:

sudo dpkg-reconfigure slapd

Respuestas sugeridas:

• ¿Omitir configuración? → No

• Nombre de dominio DNS → empresa.local

• Nombre de la organización → Empresa

• Contraseña de administrador LDAP → la que yo quiera

• ¿Borrar base al purgar slapd? → No

• ¿Permitir LDAPv2? → No

COMPROBAR FUNCIONAMIENTO BÁSICO

Ejecutar:

ldapsearch -x

Si devuelve “result: 32 No such object”, el servidor está activo pero sin entradas personalizadas.

CREAR LA ESTRUCTURA DEL ÁRBOL LDAP

Crear el archivo estructura.ldif con:

dn: ou=people,dc=empresa,dc=local

objectClass: organizationalUnit

ou: people

Añadirlo con:

ldapadd -x -D "cn=admin,dc=empresa,dc=local" -W -f estructura.ldif

CREAR UN USUARIO LDAP

Generar contraseña cifrada:

slappasswd

Copiar la cadena {SSHA}…

Crear usuario.ldif con:

dn: uid=juan,ou=people,dc=empresa,dc=local

objectClass: inetOrgPerson

uid: juan

sn: Pérez

givenName: Juan

cn: Juan Pérez

mail: juan@empresa.local

userPassword: {SSHA}contraseña_cifrada

Añadirlo con:

ldapadd -x -D "cn=admin,dc=empresa,dc=local" -W -f usuario.ldif

BUSCAR UN USUARIO

ldapsearch -x -b "dc=empresa,dc=local" "(uid=juan)"

MODIFICAR UN USUARIO

Crear modificacion.ldif con:

dn: uid=juan,ou=people,dc=empresa,dc=local

changetype: modify

replace: mail

mail: juan.perez@empresa.local

Aplicar con:

ldapmodify -x -D "cn=admin,dc=empresa,dc=local" -W -f modificacion.ldif

ELIMINAR UN USUARIO

Ejecutar:
ldapdelete -x -D "cn=admin,dc=empresa,dc=local" -W "uid=juan,ou=people,dc=empresa,dc=local"

LECCIONES APRENDIDAS
Automatización: Comprender cómo gestionar un directorio LDAP, crear y modificar entradas.

Autenticación centralizada: Saber que LDAP es base para acceso a múltiples sistemas.

Administración: Conocer comandos ldapadd, ldapmodify, ldapsearch y ldapdelete.
