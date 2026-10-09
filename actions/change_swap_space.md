# Acción: crear, verificar y retirar un archivo Swap

## Objetivo

Aumentar temporalmente o de forma persistente el espacio Swap mediante un
archivo, sin modificar particiones. También explica cómo retirarlo de forma
segura.

## Cuándo utilizarla

Cuando la memoria RAM se agota con frecuencia y necesitas una válvula adicional
para cargas puntuales. Swap no sustituye RAM ni corrige por sí sola una fuga de
memoria o un proceso descontrolado.

## Compatibilidad, privilegios y riesgo

- **Compatibilidad:** Linux con `util-linux`, archivo en un filesystem adecuado
  y soporte de `swapon`.
- **Referencia:** Ubuntu LTS; las órdenes de activación son comunes en muchas
  distribuciones.
- **Privilegio:** root mediante `sudo`.
- **Riesgo:** cambio persistente en almacenamiento y `/etc/fstab`; retirar un
  archivo activo puede provocar un fallo.

## Distinción importante

- Una **partición Swap** es un dispositivo o partición formateada con
  `mkswap`.
- Un **archivo Swap** es un archivo regular reservado para el kernel.
- Una entrada en `/etc/fstab` hace persistente la activación. En sistemas con
  systemd puede aparecer una unidad `.swap` generada a partir de esa entrada.
- Swap usada al 100% no demuestra por sí sola que haya un fallo; revisa RAM,
  presión, I/O y procesos.

## Inspección previa

Elige una ruta y tamaño concretos. No continúes si el archivo ya existe:

```bash
free -h
swapon --show --output=NAME,TYPE,SIZE,USED,PRIO
cat /proc/swaps
grep -nE '(^|[[:space:]])swap([[:space:]]|$)' /etc/fstab
systemctl list-units --type=swap --all
df -hT /
test -e /swapfile_extra && printf 'La ruta ya existe\n'
```

Confirma que `/` tiene espacio, que no hay una entrada para
`/swapfile_extra` y que el filesystem permite el método escogido. En Btrfs,
NFS, contenedores o entornos administrados por un proveedor pueden existir
restricciones específicas.

## Respaldo y precauciones

Guarda una copia de `fstab` antes de editarla:

```bash
sudo cp -a /etc/fstab "/etc/fstab.bak.$(date +%Y%m%d-%H%M%S)"
```

Usa una segunda sesión si el equipo está bajo presión de memoria. No ejecutes
`swapoff` sobre el único Swap activo sin comprobar que la RAM disponible puede
absorber las páginas trasladadas.

## Ejecución

### Crear y activar el archivo

Este ejemplo crea 8 GiB en `/swapfile_extra`:

```bash
sudo fallocate -l 8G /swapfile_extra
sudo chmod 600 /swapfile_extra
sudo mkswap /swapfile_extra
sudo swapon /swapfile_extra
```

Si `fallocate` produce un archivo con agujeros que `swapon` rechaza, consulta
la documentación del filesystem y usa un método alternativo adecuado para ese
entorno. No improvises con un archivo que no haya sido verificado.

## Hacerlo persistente sin duplicar fstab

Comprueba la línea exacta y añádela solo si no existe:

```bash
grep -Fqx '/swapfile_extra none swap sw 0 0' /etc/fstab || \
  printf '%s\n' '/swapfile_extra none swap sw 0 0' | sudo tee -a /etc/fstab
sudo findmnt --verify
sudo systemctl daemon-reload
```

La orden no debe producir una segunda línea idéntica. Si `findmnt --verify`
informa errores, detén el procedimiento y corrige `fstab` antes de reiniciar.

## Verificación

```bash
ls -lh /swapfile_extra
stat -c '%A %a %U:%G %n' /swapfile_extra
swapon --show --output=NAME,TYPE,SIZE,USED,PRIO
free -h
systemctl list-units --type=swap --all
```

Debes ver el archivo activo y el total Swap incrementado. Los permisos deben
ser restrictivos, normalmente `-rw-------`, y el propietario debe ser root.

Para probar persistencia sin reiniciar, valida la configuración con
`findmnt --verify`; no reinicies un equipo productivo solo para probar una
línea no revisada de `fstab`.

### Retirar el archivo

Retíralo solo si confirmas que no está siendo usado y que existe otra capacidad
de memoria suficiente:

```bash
swapon --show
free -h
sudo swapoff /swapfile_extra
swapon --show
```

Después elimina únicamente la línea exacta de `fstab` y valida:

```bash
sudo sed -i '\|^/swapfile_extra none swap sw 0 0$|d' /etc/fstab
sudo findmnt --verify
sudo systemctl daemon-reload
sudo rm -- /swapfile_extra
```

Si `swapoff` falla por falta de memoria, no borres el archivo: libera memoria,
añade otra Swap o planifica una ventana de mantenimiento.

## Rollback

Si la activación falló antes de escribir en `fstab`, desactiva y elimina el
archivo solo si quedó creado:

```bash
sudo swapoff /swapfile_extra 2>/dev/null || true
```

Si el cambio persistente quedó mal, restaura la copia de `fstab` elegida tras
comparar su contenido con la versión actual:

```bash
sudo cp -a /etc/fstab.bak.<marca> /etc/fstab
sudo findmnt --verify
sudo systemctl daemon-reload
```

Sustituye `<marca>` por el nombre real del respaldo; no uses un comodín sin
confirmar el archivo exacto.

## Errores frecuentes

- `swapon: <archivo> is busy`: el archivo ya está activo; revisa `swapon --show`.
- `Invalid argument`: el filesystem o el archivo no cumple las condiciones de
  Swap; consulta el método apropiado.
- `Permission denied`: faltan privilegios o los permisos del archivo son
  inseguros.
- `No space left on device`: libera espacio o elige un tamaño menor; no llenes
  completamente `/`.
- Se duplica una unidad `.swap`: revisa `/etc/fstab`, `systemctl list-units`
  y las configuraciones generadas antes de volver a activar.

## Fuentes

- [Ubuntu storage documentation](https://ubuntu.com/server/docs/how-to/storage/)
- [Ubuntu swapon and swapoff manpage](https://manpages.ubuntu.com/manpages/noble/man8/swapon.8.html)
- [Ask Ubuntu swap discussions](https://askubuntu.com/questions/tagged/swap)
