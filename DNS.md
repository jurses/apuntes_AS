# DNS

## ¿Qué es la DNS?
La DNS _Domain Name Service_ es un servidor encargado de traducir las direcciones IP a cadenas que correspondan con el servicio para que sean entendibles por el ser humano.

*	Son un sistema jerárquico y descentralizado.
*	Localización: ¿dónde se encuentra?
	*	Ordenadores
	*	Servicios
	*	Cualquier recurso conectado a una red de datos
*	Internet y redes privadas
*	Usualmente traduce nombres de domino en direcciones numéricas IP
	*	Los humanos recordamos mejor los nombres
	*	Las máquinas trabajn mejor con números

## Distribución y jerarquía
*	Cada servidor DNS se encarga de una porción de todos los nombres existentes de la que es responsable.

Ejemplo
```
*	'.' raíz
	*	.es
		*	ull.es
			*	etsii.ull.es
		*	ulpgc.es
	*	.fr
	*	.com
	*	.org
```
## Dominios y zonas
*	Dominio: parte del espacio de nombres DNS _subtree_ ejemplo: `www.ull.es`
*	Zona: estructura en la que se almacena la información de gestión.
	*	Contiene información de un dominio o una parte del mismo.
	*	Si no hay subdominios que constituyen zonas diferentes, la zona corresponde al dominio completo.
	*	Conjunto de servidores que se utilizan para almacenar todos los registros de ese espacio de nombres -> **Ficheros de zona**

*	Destinos de ficheros de zonas ¿?

## ¿Qué almacenan los servidores DNS?
*	Registros de recursos (RRs, resource records):
	*	Start of Authority __SOA__: indica qué servidor DNS es el autorizado para gestionar una zona en particular.
	*	Host __A__: son utlizados para las resoluciones directas, en las que se busca la IP a partir del nombre.
	*	Pointer __PTR__: son los contrarios a los anteriores, son usados para las resoluciones inversasm en las que se proporciona una IP ara obtener el nombre de un recurso.
	*	Name Server __NS__: indica los servidores DNS disponibles para una zona. Aunque sólo exista un SOA, pueden existir carios servidores en una xona por balanceo de carga y disponibilidad del servicio.
	*	Service __SRV__: indican qué recursos tenen disponible un determinado servicio. Este tipo de registros es muy importante para Active Directory.
	*	Mail Exchanger __MX__: Indica los servidores disponibles en el dominio para recibir mensajes de correo electrónico.
	*	Canonical Name __CNAME__: representa un alias para un recurso particular, de modo que varios nombres puedan apuntar a la misma dirección ip.

## Proceso de la resolución de nombres.
Se basa en una consulta recursiva donde el cliente pregunta por una dirección como `www.ull.es` y los servidores DNS se van preguntando unos a otros si conocen la dirección.

1.	El cliente pregunta por `www.ull.es` a su servidor __A__ DNS.
2.	El servidor DNS __A__ pregunta a otro servidor __B__ si conoce la ruta `.es.`, este servidor __B__ le manda una lista de servidores a __A__ con aquellos servidores que conocen a `.es.`.
3.	__A__ prueba con un servidor cualquiera envíado por __B__ a ver si conocen a `ull.es.` si lo conoce se repite el proceso anterior
4. Y cuando se haya generado la ruta completa el servidor DNS que conoce la dirección de `www.ull.es.` le mandará la dirección IP a __A__ y este al cliente.

## Tipos de servidores de DNS
*	Servidor Primario
	*	Es el único que contiene la copia original del fichero de la zona
	*	Es donde se realizan las modificaciones de la zona.
	*	Después de modificar estos datos, se replican a los servidores secundarios -> transferencia de zonas.
*	Servidores Secundarios
	*	Contienen una copia (read only) del fichero de zona.
	*	Se actualizan a través de transferencias de zona (completas o incrementales).
	*	Proporcionan balanceo de carga (no tolerancia a fallos)
*	Servidores caché
	*	No contienen copia de ningún fichero.
	*	Almacenan temporalmente las resoluciones anteriores.

## Resolución inversa
La búsqueda DNS inversa es la determinación de un nombre de dominio que está asociado a auna determinada dirección IP utilizando el DNS de internet.

## Zonas de autoridad
Es una parte del espacio de nombre de dominios sobre la que es responsable un servidor DNS, que puede tener autoridad sobre varias zonas.

## Zonas delegadas
*	Permite mantener la estructura jerárquica de DNS.
*	Se implementa a través de registros de delegación.

## Forwarders y root hints _Promotores y sugerencias de raíz_
*	Estos mecanismos se usan normalmente para conectar con los niveles superiores en la jerarquía.
*	Los *Forwarders* se establecen en sercidores DNS que no son responsables de la resolución externa.

> _Root hints_: base de datos con las IP de los servidores autorizados del dominio '.'.

## DNS dinámico
*	Algunas implementaciones ya incluyeb DNS dinámico.
*	Los clientes se registran automáticamente en el DNS
*	Los servidores también pueden registrar los servicios que proporcionan.
*	Inconvenientes: seguridad
	*	Cualquier equipo puede registrarse en el DNS de una red.
*	Solución: secure updates
	*	Sólo los equipos autorizados pueden registrarse.
