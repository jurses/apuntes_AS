# Cosas del directorio activo y organización lógica

*	Active Directory (AD)

*	Componentes de la estructura lógica
	*	Dominios
	*	Unidades organizativas
	*	Árboles
	*	Bosques
	*	Catálogo global

## Dominios

*	Es una agrupación lógica de ordenadores y otros recursos que comparten la base de datos del directorio.
*	Cada dominio está gestionado por un administrador.
*	Cada dominio incluye uno o más controladores de dominios-

>Controlador de dominio: ordenador que ejecuta Windows Server y almacena una copia completa en el AD.

*	No existe una relación maestro-esclavo entre los controladores del dominio:
	*	Las actualizaciones se pueden realizar en cualqueira de ellos.
	*	El sistema de replicación los mantiene sincronizados
	*	Excepciones a estas reglas: maestros de operaciones.


