# Almacenamiento, filesystems, montajes y Swap

## Capas del almacenamiento

Conviene separar:

1. dispositivo físico o virtual, como `/dev/nvme0n1`;
2. partición, como `/dev/nvme0n1p2`;
3. volumen lógico o RAID, si existe;
4. filesystem, como ext4, XFS o Btrfs;
5. punto de montaje;
6. permisos y opciones de montaje.

Modificar una capa no siempre corrige un problema de otra. Identifica primero
la relación completa:

```bash
lsblk -o NAME,TYPE,FSTYPE,SIZE,FSAVAIL,FSUSE%,MOUNTPOINTS,UUID
findmnt
df -hT
df -ih
```

`df -hT` muestra espacio de filesystems montados; `df -ih` muestra inodos.
Un disco puede tener bytes libres y, a la vez, no poder crear archivos por
agotamiento de inodos.

## Montajes

```bash
findmnt /ruta
mount | column -t
cat /etc/fstab
```

`/etc/fstab` describe montajes persistentes. Usa UUID o etiquetas estables en
lugar de asumir que el nombre `/dev/sdX` siempre será igual. Antes de editarlo,
haz una copia y valida con una herramienta apropiada para la versión instalada.

```bash
sudo cp -a /etc/fstab "/etc/fstab.bak.$(date +%Y%m%d-%H%M%S)"
sudo mount -a
findmnt --verify
```

No desmontes un filesystem que tenga procesos o tu directorio actual abierto.
Comprueba dependencias antes de usar `umount`.

## Uso de espacio

```bash
du -xhd1 /var 2>/dev/null | sort -h
du -xhd1 "$HOME" 2>/dev/null | sort -h
lsof +L1
```

Archivos borrados pero aún abiertos siguen ocupando espacio hasta que el
proceso cierre el descriptor. Reiniciar no debe ser la primera acción si puedes
identificar y corregir el proceso responsable.

## Swap

Swap puede ser una partición o un archivo. Inspecciona antes de cambiarla:

```bash
free -h
swapon --show --output=NAME,TYPE,SIZE,USED,PRIO
cat /proc/swaps
grep -nE '(^|[[:space:]])swap([[:space:]]|$)' /etc/fstab
systemctl list-units --type=swap
```

Una entrada persistente en `fstab` puede ser convertida en una unidad `.swap`
por systemd. No añadas líneas duplicadas ni elimines un archivo antes de
desactivarlo con `swapoff`.

## Filesystem lleno o en solo lectura

```bash
findmnt -no TARGET,SOURCE,FSTYPE,OPTIONS /ruta
dmesg --level=err,warn | tail -n 50
journalctl -k -b -p warning..alert
```

Un filesystem en modo `ro` puede ser una protección ante errores de I/O. No
fuerces `rw` sin revisar logs, estado del dispositivo y procedimiento de
recuperación del filesystem.

## Fuentes

- [Ubuntu storage documentation](https://ubuntu.com/server/docs/how-to/storage/)
- [Ubuntu swapon and swapoff manpage](https://manpages.ubuntu.com/manpages/noble/man8/swapon.8.html)
- [Linux Kernel Administrator’s Guide](https://www.kernel.org/doc/html/latest/admin-guide/index.html)
