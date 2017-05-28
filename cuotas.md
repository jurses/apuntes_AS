# Gestión de cuotas

## Introducción

*   Los sistemas Linux permiten limitar el espacio que cada usuario puede utlizar.
*   Las cuotas se establecen por sistema de ficheros.
*   Las cuotas establecen dos tipos de límites:
    *	Límite duro: no se puede sobrepasar.
    *	Límite blando:	puede sobrepasar el límite durante un período de tiempo limitado quedando advertido de esto.
*   Los límites se establecen, de forma independiente, para bloques y para inodos.

## Aplicación

1.  Añadir la opción `usrquota` en el `/etc/fstab`

```
...
/dev/sda3   /	ext3	defaults,usrquota   1	1
...
```

2.  Remontar la partición para que se active la opción `usrquota` con `
mount -o remount /`

3.  `quotacheck`: crea el fichero de control de cuotas, `aquota.user`, en el directorio ráiz del SF al que se le asignan las cuotas: `quotacheck -nm /home`

Crea `/home/aquota.user`.

4.  `quotaon -a` Activa las cuotas.
5.  `edquota username` activar la cuota de usuario/grupo
6.  `edquota -t` establece el período de gracia.
