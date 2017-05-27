# Cosas del directorio activo y organización lógica

*	Active Directory (AD)

*	Componentes de la estructura lógica
	*	Dominios
	*	Unidades organizativas
	*	Árboles
	*	Bosques
	*	Catálogo global

## Antes de nada

*   Schema: es un componente del AD que define todos los objetos y atributos que un servicio de directorio usa para almacenar datos.
*   Schema común: lo mismo que antes pero es un schema replicado en todos los dominios de un mismo bosque y guarda información solamente de los principales objetos y atributos.
*   Catálogo global: es un almacén de información que contiene un subconjunto de todos los objetos del AD.
    *	Almacena aquellos atributos más frecuentes.
    *	Su función consiste en ayudarnos con la búsqueda de información en todo el bosque.
    *	Utilización en grupos universales, permitiendo a los usuarios entrar en cualquier dominio del bosque.

## Estructura lógica

### Dominios

*	Es una agrupación lógica de ordenadores y otros recursos que comparten la base de datos del directorio.
*	Cada dominio está gestionado por un administrador.
*	Cada dominio incluye uno o más controladores de dominios-

>Controlador de dominio: ordenador que ejecuta Windows Server y almacena una copia completa en el AD.

*	No existe una relación maestro-esclavo entre los controladores del dominio:
	*	Las actualizaciones se pueden realizar en cualqueira de ellos.
	*	El sistema de replicación los mantiene sincronizados
	*	Excepciones a estas reglas: maestros de operaciones.

### Unidades organizativas (OU)

*   Son contenedores que permiten agrupar los objetos de un domnio en grupos administrativos lógicos.
*   Pueden contener cuentas de usuarios, grupos o recursos...
*   No hay límite en la profundidad de la estructura.
*   Se usan para agrupar objetos de la forma que mejor se ajuste a las necesidades de la organización.
*   Permite delegar el control administrativo de forma controlada.
    *	Es posible delegar el control total sobre todos los objetos de la OU.
    *	Es posible delegar control parcial de la OU.

### Árboles

*   Un árbol es una agrupación jerárquica de uno o más dominios Windows que comparten un espacio de nombres contiguo. Ejemplo peladilla.kek `<-> {casa.peladilla.kek, bar.peladilla.kek}`
*   Añadido un nuevo dominio al árbol, éste pasa a ser el hijo y se combina con el dominio del padre para formar su nombre DNS.
*   Confianza bidireccional y transitiva con el dominio padre.

### Bosques

*   Es una agrupación jerárquica de árboles
*   Los árboles en el bosque no comparten un espacio de nombres contiguos.
*   Los árboles en el bosque comparte un **schema común** y el **catálogo global**.
*   El primer dominio creado será el **dominio raíz del bosque**

### ¿Qué se comparte en el bosque?

*   **schema común** y el **catálogo global**
*   Administradores
    *	Administradores de schema: pueden hacer cambios que afectan a todo el bosque.
    *	Administradores de la organización
*   Configuración de confianza compartida:
    *	Todos los dominos del bosque se configuran para que confíen en los restantes dominos del bosque.

### Reflexiones sobre dominios múltiples

*   Se crea múltiples dominios cuando:
    *	Queremos reducir el tráfico de replicación. Una organización con muchos dominios, sólo se replican los cambios realizados en el servidor de catálogo, el schema y la información de configuración.
    *	AD proporciona seguridad entre múltiples dominios a través de las relaciones de confianza.
    *	Una relación de confianza es una relación que se establece ente dos DC (controladores de dominio), permiten la autenticación de usuarios entre dominios.
    *	Tipos de relaciones de confianza:
	*   Transitivas: es bidireccional, hay confianza entre todos los dominios de un árbol, relación padre-hijo.
	*   Unidireccionales: relaciones manuales entre dominios de un bosque, siven para crear atajos y mejorar prestaciones.
	*   Bosque: integra organizaciones con sistemas ya instalados, permite a los usuarios acceder a los recursos del otro bosque.
	    *	**No son relaciones transitivas**.
	    *	Cada bosque tiene su catálogo global y schema.
	*   Realms: permite tener relaciones de confianza entre AD y otras implementaciones de Kerberos.

## Estructura física

### Estructura física del AD

Al contrario que la estrucutra lógica que permite organizar los recursos de la red, la estructura física se encarga de configurar y gestionar el tráfico de la red.

Define cómo y cuándo se produce el proceso de replicación y el proceso de login.

La estructura física viene delimitada por los **Controladores de dominio** y los **sites**

### Controladores de dominio

*	Es un ordenadore que ejecuta Windows Server
*	Almacena la base de datos de AD.
*	Gestiona los cambios en el directorio y propaga estos cambios a otros controladores del mismo dominio.
*	LLeva a cabo los proceso de login, autenticación y búsqueda en el directorio.
*	Por cada dominio puede haber más de un controlador de domino, mejora la tolerancia a fallos.

#### CD y catálogo global

*	El primer DC se cibvuerte en el servidor del catálogo global
*	Gestiona las peticiones para el catálogo global.
*	Conviene añadir más servidores para el balanceo de carga.
*	Solo tiene permisos de lectura para el catálogo global.
*	Los usuarios universales se almacenan en el catálogo global.

### Replicas del directorio Activo

*	Los DC en una red W2K siguen el modelo multimaste: todos los servidores tienen una base de datos donde se puede actualizar la información.
*	Existen réplicas para sincronizar las bases de datos de lo DC.
