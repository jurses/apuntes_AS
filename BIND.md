# BIND _Berkely Internet Name Domain_

## ¿Qué es BIND?

> BIND es el servidor de DNS más comúnmente usado en Internet, especialmente en sistemas Unix.

## BIND, cliente Linux

*	Fichero /etc/resolv.conf
	*	Dominio al que pertenece la máquina
	*	Servidores de nombre a utilizar
	```
	domain prueba.com
	nameserver 192.168.50.2
	nameserver 192.168.50.100
	```
*	En el fichero /etc/nsswitch.conf se puede configurar el orden de base de datos a consultar para la resolución de nombres (host, DNS, ...)

## BIND, servidor Linux

*	Fichero /etc/named.conf **Configuración**
	*	Un archivo típico de named.conf está organizado de forma similar al ejemplo siguiente:
	```
	Sentencia1["<nombre>"]{
		opcion1;	//comentarios
		opcion2;
	};
	Sentencia1["<nombre>"]{
		opcion1;
		opcion2;
	};
	...
	```
Donde sentencia indica una faceta particular de la operación de BIND y opciones son el conjunto de opciones aplicables a esa sentencia en particular.

## named.conf *sentencias I*

*	Las sentencias más usuales son:
	*	ACL: Listas de control de acceso. Permite limitar el acceso a ciertas máquinas.
	Ejemplo:
	```
	acl black-hats{	10.0.2.0/24;
					192.168.0.0/24; };
	acl red-hats{	10.0.1.0/24;	};
	options{
		blackhole{black-hats;};
		allow-query{red-hats;};
		allow-recursion{red-hats;};
	}
	```

## named.conf *sentencias II*

*	Include: permite incluir otro archivo y que se trate como parte de este archivo.
*	Options: define opciones de configuración de servidor globales y configura otras declaraciones por defecto.

Las opciones más habituales son:
*	allow-query: especifica cuáles hosts tienen permitido consultar este servidor de nombres.
*	allow-recursion: parecido a allow-query, salvo que se aplica a las peticiones recursivas.
*	blackhole: especifica cuáles hosts no tienen permitido consultar al servidor de nombres.
*	directory: especifica el directorio de trabajo `named` si es diferente del valor predeterminado `/var/named`.
*	forwaders: especifica una lista de direcciones IP válidas para los servidores de nombres donde las peticiones se pueden reenviar para ser resueltas.

## named.conf *sentencias III*

*	Logging: especifica sobre qué información se guardarán los logs y cuál se ignorará. También permite especificar dónde se guardará esa información
*	Server: configura opciones específicas del servidor
*	Zone: define una zona DNS

El formato de la sentencia es:

## sentencia "zone"

*	allow.query especifica los clientes que se autorizan para pedir información sobre una zona.
*	allow-transfer especifica los servidores esclavos que están autorizados para pedir una transferencia de información de la zona.
*	allow-update especifica los hosts que están autorizados para actualizar dinámicamente la información en sus zonas.

```
zone <zone-name> <zone-class>{
	<zone-options>;
	[<zone-options>;...]
};
```
