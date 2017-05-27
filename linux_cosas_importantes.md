### Tipos de sistemas de archivo
*	Journaling: registros diarios donde se guardan los cambios a aplicar en el disco. Permite restablecer los datos afectados por la transacción en caso de que ésta falle.

### Directorios a partir de raiz '/'

*	/bin: binarios
*	/sbin: binarios del sistema para uso del administrador
*	/etc: archivos de configuración
*	/usr: donde residen las aplicaciones y librerías básicas
*	/sys: datos de configuración del núcleo del sistema.
*	/dev: dispositivos lógicos
*	/home: directorios de los usuarios
*	/root: directorio de superusuario
*	/mnt: punto de montaje
*	/proc: información sobre los recursos del sistema
*	/var: almacenamiento para los registros, colas de correo o impresión.

### Montando y desmontando cosas
Montar una unidad.
```sh
mount /dev/sdb1 /home
```

Desmontarla
```sh
umount /home
```

El mount es de forma temporal, si queremos que se mantenga montada durante los próximios inicios de sesióne tenemos que configurar el `etc/fstab`

```sh
# <file system>	<dir>	<type>	<options>	<dump>	<pass>
# /dev/sda3
UUID=5d3cecfe-3518-490f-8eb6-7a072ac543f1	/         	ext4      	rw,relatime,data=ordered	0 1
```

En orden:

1.	Dispositivo
2.	Directorio de montaje
3.	Tipo de sistema de archivos
4.	Opciones
5.	Frecuencia del dump: frecuencia con la que se hará el backup, 0 nunca, 1 diario, 2 cada 2 días.
6.	`fsck` revisa la consistencia y repara los sistemas de archivo. Determina el orden.

### Administrador de volúmenes lógicos LVM
Es una serie de dispositivos físicos o RAID que ofrecen memoria pero que a la hora del usuario interpretarlo le es abstraído por una capa que lo hace ver como un sólo dispositivo.

El **LVM** se encarga de fragmentar los volúmenes físicos y reagruparlos de la forma que resulte más convenientes.

Ventajas:
1.	Permite expandir el volúmen facilmente.
2.	El disco puede ser definido a un único grupo lógico y paralmacer directorios como /opt y /home y poder redimensionarlo.
3.	Se le puede añadir permisos y engañar al usuario
4.	Facilita administrar un sistema con muchos discos

### Arranque
*	BIOS: dispositiv de arranque que espera leer en el MBR para arrancar el sistema operativos correspondiente.
*	UEFI: busca gestores de arranque localizados en una partición especial EFI, que usa un sistema de archivos FAT.
*	Grub y Grub2: gestor de arranque múltiple, que nos permite elegir qué sistema operativo arrancar.

### Super/usuarios

*	root es el usuario que tiene acceso a todo en el sistema, realiza tareas de administración.
*	los usuarios normales son usuarios sometidos a permisos que disfrutan de los recursos que les son otorgados.

Se puede acceder a super usuario desde cualquier usuario con el programa `su -` *substitute user identity*. Aunque puede estar bloqueado este modo de acceso

El fichero sudoers es un fichero que controla quién puede ejecutar determinados comandos, qué usuarios o qué máquina darle permisos.
**ESTE ARCHIVO SE EDITA CON visudo**

### Cuentas de usuario
`/etc/passwd`
```
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/usr/bin/nologin
jurses:x:1000:1000:Raul:/home/jurses:/bin/bash
```
Sigue el siguiente formato:
`usuario:x:UID:GID:informacion-usuario:directorio-home:shell`

Antiguamente las claves se almacenaban donde se encontraba la 'x' pero actualmente las claves se encuentran de forma cifrada en `/etc/shadow`
`marta:$6$fbzJWfo6$aduPGl9grxhhzPbVMhMbh9MDWjXD3kv1dOkodsueRXDSv9OHbAKZA6YKlOqSe5LBPEqek0t5KyrSiTwuGZ
8DK0:16474:0:60:3:2::`
Sigue el siguiente formato:
* usuario
* contraseña-cifrada($tipo de cifrado$)
* última modificación de la contraseña 
* numero de días que tiene que pasar para cambiar la contraseña
* numero de días que se puede tener la misma contraseña
* días de aviso antes de que la cuente expire
* indica cuantos días transcurren desde que caduca la contraseña
* fecha de expiración de la cuenta
* campo no usado
Si se quiere desactivar la cuenta basta con poner un '\*' o un '!' delante, como comentándola.

### Grupos de usuario
```
/etc/group
asesores:x:525:marta,juan,manolo
laura:x:1002:
lidia:x:1003:
```
*	Nombre de grupo
*	Contraseña: se encuentra en `/etc/gshadow`
*	GID
*	Usuarios pertenecientes al grupo

```
/etc/gshadow
taller:$6$8pYeH/iZ44$YRfR2Rz.r3XEC3hcVVvbaK.bj/dz5c0NTlU/nPIdQ9GxUe3z89NgDMCihaQ8EH6mJH4BAyAw4G37hf
EKMMn8x.:marta:juan,manolo,maria
```

### Tipos de archivo
Podemos verlo haciendo un `ls -l`.

*	'-' archivo común
*	'd' de tipo directorio
*	'l' de tipo enlace
*	'p' de tipo tubería
*	's' de tipo socket
*	'b' asociado a un dispositivo de bloques
*	'c' archivo asociado a un dispositivo

### Permisos

| Acceso	| Fichero	| Directorio	| 
| ---------:| ---------:| -------------:|
| r	| Ver contenido	| Ver contenido	| 
| w	| Modificar fichero	| Crear, borrar, renombrar	| 
| x	| Ejecución	| Entrar en el directorio (cd)	| 

### Permisos especiales

*	't': sticky bit, solamente el propietario del directorio puede borrarlo.
*	'T': lo mismo que el anterior pero sin permisos de ejecución.
*	's': setGID, se usa para que los usuarios del sistema puedan ejecutar binarios con privilegios elevados. En los directorios se usan para que subarchivos o subdirectorios mantengan el GID del dueño del directorio.
*	'S': lo mismo pero sin permisos de ejecución.

### Listas de control de acceso
```
$ ls -l
rwxrwxr‐x+ ...
```
Son una serie de listas que conviven con los permisos "ugo" anteriores para asignar permisos con mayor complejidad.

Para añadir una entrada:
`setfacl -m <ugo>:<usuario/grupo>:<permisos> archivo`

Para ver como quedaría:
`getfacl archivo`

* mask: indica los permisos máximos de todos los grupos y usuarios sin incluir al usuario propietario y los "otros". La máscara limita.

* default: solo se puede se puede aplicar a directorios, indica que va a heredar esos permisos.

* effective: cálculo que realiza el acl diciéndote los permisos efectivos del directorio o fichero para cierta entrada.

### Control de acceso obligatorio (MAC - Mandatory Access Control)

Procedimiento para restringir el acceso a los objetos de un sistema.

Los sistemas MAC al contrario que DAC proporcionan al administrador control total sobre lo que es o no permitido en el sistema. Funciona dependiendo de las políticas establecidas por el administrador.

### LDAP y openLDAP Lightweight Directory Access Protocol

Protocolo que permite el acceso a un servicio de directorio ordenado y distribuido para buscar diversa información en un entorno de red.

*	Los objetos almacenados en el repositorio son generalmente pequeños y la información está especificada mediante atributos.
*	Los datos están replicados en distintos servidores.
*	Sigue el diseño **WORM** *write once, read many*.
*	Los datos se estructuran de forma jerárquica y no se atiende al concepto de tabla, duplicidad o redundancia.

Tipos de modelos:
*	Modelo de información: determina la estructura de la información que es almacenada en el directorio.
*	Modelo de nombres: define cómo se organiza la información en el directorio y como se identifica.
*	Modelo funcional: define las operaciones que pueden realizarse sobre el directorio.
*	Modelo de seguridad: defie como proteger la información contenida en el directorio.


