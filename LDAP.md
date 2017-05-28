# LDAP y openLDAP Lightweight Directory Access Protocol

Protocolo que permite el acceso a un servicio de directorio ordenado y distribuido para buscar diversa información en un entorno de red.

*	Los objetos almacenados en el repositorio son generalmente pequeños y la información está especificada mediante atributos.
*	Los datos están replicados en distintos servidores.
*	Sigue el diseño **WORM** *write once, read many*.
*	Los datos se estructuran de forma jerárquica y no se atiende al concepto de tabla, duplicidad o redundancia.

## Terminología

*   CN      commonName
*   L       localityName
*   ST      stateOrProvinceName
*   O       organizationName
*   OU      organizationalUnitName
*   C       countryName
*   STREET  streetAddress
*   DC      domainComponent
*   UID     userid

## Tipos de modelos

*	Modelo de información: determina la estructura de la información que es almacenada en el directorio.
*	Modelo de nombres: define cómo se organiza la información en el directorio y como se identifica.
*	Modelo funcional: define las operaciones que pueden realizarse sobre el directorio.
*	Modelo de seguridad: defie como proteger la información contenida en el directorio.

## Configuración básica de LDAP

### Servidor:

*   Vamos a definir un administrador para el administrador de LDAP, este **no** tiene porqué ser **root** o algún usuario de `/etc/passwd`
*   Ciframos el password con `slappasswd`.
*   Una vez cifrada copiamos la clave y añadimos la línea en nuestro `
/etc/openldap/slapd.d/cn=config/olcDatabase={2}hdb.ldif` `olcRootPW: <clave-cifrada>`
*   Especificamos el DN del administrador en `lcDatabase={1}monitor.ldif`.
*   Para buscar usamos el `ldapsearch`
    ```
    ldapsearch -x -s base namingContexts

    namingContexts: dc=<domain_component>, dc=<domain_component>
    ```
*   ¿Cómo añadir algunos elementos al directorio?
    ```ldif
    dn: dc=cotufa,dc=com
    dc: cotufa
    objectClass: top
    objectClass: domain
    dn: ou=miOU,dc=cotufa,dc=com
    objectClass: organizationalUnit
    ou: miOU

    dn: cn=Pepe Lopez,ou=miOU,dc=cotufa,dc=com
    cn: Pepe Lopez
    objectClass: inetOrgPerson
    mail: pepel@cotufa.org
    givenName: Pepe
    surname:Lopez
    displayName: Pepe Lopez
    telephoneNumber: 666 444 555
    ```

    `ldapadd -xD "cn=Manager,dc=cotufa,dc=com" -W -f prueba.ldif`

*   Para buscar: `ldapsearch ‐x ‐b “dc=cotufa,dc=com” “surname=Lop*”`, devuelve la entrada correspondiente a `Pepe Lopez`
*   Para eliminar:  `ldapdelete ‐xD “cn=Manager,dc=cotufa,dc=com” ‐W \ “cn=Pepe Lopez,ou=miOU,dc=cotufa,dc=com”`.

### Cliente

*   Podemos realizar búsquedas al servidor como: `ldapsearch ‐h <ip-servidor> ‐x ‐b “dc=cotufa,dc=com” “surname=Lop*”`
