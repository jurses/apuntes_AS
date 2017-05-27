# Windows - Active Directory - DNS

## DNS y AD
*	Resolución de nombres: la resolución de nombres de las distintas máquinas e el dominio se realiza usando DNS
*	Esquema de nombres en el dominio: el AD sigue el mismo esquema de nombres que el servicio DNS.

## Localización de servicios
*	Windows usa DNS para localizar servicios en la red.
	*	Ejemplo: para encontrar las distintas impresoras -> catálogo global.
	*	Ejemplo: cada controlador de dominio registraba el nombre dominio con _<1C>_ en el carácter 16, en el servidor WINS correspondiente.

## Registro de recursos
Se utilizan para almacenar datos sobre nombres de dominio y direcciones IP.
La base de datos de zona de DNS está formada por una serie de registros de recursos. Cada registro de recurso especifica la información pertinente sobre un objeto determinado.

## Proceso de inicion sesión
1.	Logon or AD search
2.	Net Logon collects client information
3.	Sends dns query with client info ->
4.	DNS queries SRV records for match
5.	Return lost of IP addresses <-
6.	Client contacts domain controllers ->
7.	Domain controllers respond <-
8.	Client sends request to a domain controller

## Zonas integradas en Actuve Directory
Una de las principales ventajas de DNS en W2K es la posibildad de itegrar DNS con el AD
*	Ventajas:
	*	LA información de la zona no se almacena en el fichero dNS sino en la base de datos de AD -> seguridad.
	* El prceso de transferencia de zona se sustituye por la __replicación__ de AD.
		*	Sólo componentes modificados.
		*	Replicación entre sitios optimizada.
	*	Ofrece la posibilidad de configuración __multimaster__ de servidores.
		*	Evita tener un solo servidor primario.
		*	Los cambios pueden realizarseen cualquier controlador del dominio.
	*	Opción de actualización segura
		*	Se controla quién puede hacer modificaciones
