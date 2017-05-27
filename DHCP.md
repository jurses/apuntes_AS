# El protocolo DHCP _Dynamic Host Configuration Protocol_

Es un servidor que usa protocolo de red de tipo cliente/servidor en el que generalmente un servidor posee una lista de direcciones IP dinámicas y las va a signando a los clientes.

## Ventajas
*	Permite centralizar la configuración del protocolo **TCP/IP**.
	*	Dirección IP.
	*	Máscara de red.
	*	Puerta de enlace, DNS, ...
*	Reduce los problemas de gestión manual del protocolo.

## Principales características
*	DHCP permite controlar los parámetros de configuración de una red.
*	Los clientes DHCP son cofigurados de forma dinámica.
*	Múltiples servidores DHCP pueden dar servicio a una o más subredes.
*	DHCP proporciona una base de datos dinámica para la asignación de IP.
*	Al reiniciarse un cliente, puede seguir usando la IP asignada la última vez por DHCP, si no ha caducado.
*	DHCP usa los puertos UDP: 67 para peticiones (clientes) y 68 para respuestas (servidor).

## ¿Cómo funciona?
1.	El cliente manda una petición de concesión.
2.	El servidor oferta direcciones IP.
3.	El cliente selecciona la dirección IP.
4.	El servidor le confirma la concesión.
> Concesión: Acción de conceder algo que se pide o se desea.

*	La dirección asignada se reserva de modo que no se ofrece a ningún otro cliente.

## Renovación de la concesión del IP
Los clientes DHCP intentan renovar la concesión cuando ha transcurrido el 50% del tiempo -> Envían un paquete `DHCPREQUEST` dirigido al servidor DHCP que le concedió el IP.

1.	Si el servidor está disponible: renueva la concesión `DHCPACK`
2.	Si el servidor no está disponible: el cliente sigue usando la dirección que tenía
3.	El cliente intentará contactar con cualquier servidor DHCP cuando expire el 87.5% del tiempo `DHCPREQUEST`
	1.	Si contesta el servidor -> renovar IP
	2. Si no contesta, volvemos al paso 1.

## Gestión de DHCP
*	Consideraciones de diseño
	*	Al usar broadcasts, es necesario la presencia de un servidor DHCP por cada segmento -> Ámbitos (scopes)
	*	Puede utilizarse _DHCP relay agents_ para evitar las limitaciones anteriores.

> Ámbitos (scopes): un ámbito es una grupación administrativa de direcciones IP para equipos de una subred que usan el servicio DHCP (protocolo de configuración dinámica de host)

> Segmento: es sinónimo de LAN, que es una red de área local
