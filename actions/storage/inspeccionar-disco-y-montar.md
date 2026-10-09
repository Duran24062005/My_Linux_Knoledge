# Acción: inspeccionar un disco y montar un filesystem existente

## Objetivo

Identificar un dispositivo o partición y montarlo en un directorio sin
formatearlo ni modificar la tabla de particiones.

## Cuándo utilizarla

Cuando un filesystem existente no está montado o necesitas consultar su
contenido en una ruta controlada.

## Compatibilidad, privilegios y riesgo

- **Compatibilidad:** Linux con `util-linux`; rutas y tipos de filesystem pueden
  variar.
- **Privilegio:** lectura parcial como usuario normal; montaje con `sudo`.
- **Riesgo:** bajo si solo se monta; medio si se escribe `fstab`.

## Inspección previa

```bash
lsblk -o NAME,TYPE,FSTYPE,SIZE,FSAVAIL,FSUSE%,MOUNTPOINTS,UUID
findmnt
df -hT
```

Identifica el dispositivo por nombre, tamaño, filesystem y UUID. Si la salida no
permite distinguirlo con seguridad, detente. No ejecutes `mkfs`, `wipefs`,
`fdisk` ni `parted` como parte de esta acción.

## Respaldo y precauciones

Si el montaje será persistente, respalda y revisa `/etc/fstab`:

```bash
sudo cp -a /etc/fstab "/etc/fstab.bak.$(date +%Y%m%d-%H%M%S)"
```

Usa un directorio vacío y no montes encima de una ruta que ya contenga datos
sin confirmar el estado actual.

```bash
findmnt /ruta/de/montaje
sudo mkdir -p /ruta/de/montaje
```

## Ejecución

### Montaje temporal

```bash
sudo mount <dispositivo-o-uuid> /ruta/de/montaje
findmnt /ruta/de/montaje
df -hT /ruta/de/montaje
```

Puedes usar UUID explícito con `UUID=<uuid>` si el sistema lo admite y el
filesystem es conocido.

### Persistencia controlada

Obtén una línea adecuada con la documentación del filesystem y añade una sola
entrada coherente a `fstab`. Después valida sin reiniciar:

```bash
sudoedit /etc/fstab
findmnt --verify
sudo mount -a
findmnt /ruta/de/montaje
```

No continúes si `mount -a` informa errores.

## Verificación

```bash
findmnt -no SOURCE,FSTYPE,OPTIONS,TARGET /ruta/de/montaje
df -hT /ruta/de/montaje
touch /ruta/de/montaje/.prueba-montaje
rm /ruta/de/montaje/.prueba-montaje
```

La prueba de escritura solo debe hacerse si tienes autorización y sabes que el
filesystem debe ser escribible.

## Rollback

Para un montaje temporal:

```bash
sudo umount /ruta/de/montaje
```

Antes de desmontar, sal de la ruta y revisa procesos abiertos:

```bash
findmnt /ruta/de/montaje
sudo fuser -vm /ruta/de/montaje
```

Para un montaje persistente, elimina o comenta únicamente la entrada agregada,
valida `fstab` y desmonta cuando ningún proceso la use.

## Errores frecuentes

- `wrong fs type`: filesystem incorrecto, módulo ausente o dispositivo equivocado.
- `target is busy`: una shell o proceso mantiene abierta la ruta.
- Montaje duplicado: ya existe una unidad o entrada en `fstab`.
- El dispositivo no aparece: revisa kernel, cableado, VM o almacenamiento.

## Fuentes

- [Ubuntu storage documentation](https://ubuntu.com/server/docs/how-to/storage/)
- [Linux Kernel Administrator’s Guide](https://www.kernel.org/doc/html/latest/admin-guide/index.html)
