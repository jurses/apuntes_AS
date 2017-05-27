# Kerberos

## ¿Cómo funciona?

AS = Servidor de autenticación
SS = Servicio del servidor

*	El usuario se loguea en el cliente
	*	Le dan un TGT
	*	La máquina guarda el TGT y lo usa para mostrar las credenciales del usuario

1.	Envío de la solicitud de pre-autenticación
	*	AS\_REQ

2.	Cuando el controlador de dominio comprueba la información envía el TGT
	*	El cliente lo almacena pero no puede leer el contenido TGT.

3.	El cliente envía un paquete TGS\_REQ
4.	El servidor comprueba que existe el SPN y que el TGT es correcto. Envía una TGS\_REP
5.	El cliente ya puede enviar el "ticket de servicio" al servidor
6.	El servidor "puede" enviar un paquete para confirmar que es el servidor que dice ser: AP\_REP
