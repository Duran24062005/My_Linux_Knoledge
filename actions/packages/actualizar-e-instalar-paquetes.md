# Acción: actualizar e instalar paquetes

## Objetivo

Actualizar el índice de paquetes de Ubuntu, revisar cambios e instalar o
retirar un paquete sin usar fuentes no verificadas.

## Cuándo utilizarla

Para mantener un Ubuntu LTS actualizado o instalar una herramienta disponible en
los repositorios configurados.

## Compatibilidad, privilegios y riesgo

- **Compatibilidad:** los comandos principales son Ubuntu/Debian con APT.
- **Privilegio:** `sudo` para actualizar e instalar; consulta puede ser como
  usuario normal.
- **Riesgo:** cambio persistente; la eliminación o actualización puede afectar
  servicios.

## Inspección previa

```bash
cat /etc/os-release
apt-cache policy
apt-mark showhold
systemctl --failed --no-pager
df -hT /
```

Confirma que el sistema es Ubuntu/Debian, que hay espacio en `/`, que no hay
paquetes retenidos inesperados y que conoces la ventana de mantenimiento.

## Respaldo y precauciones

Para una actualización importante, registra la lista de paquetes y respalda la
configuración de los servicios afectados:

```bash
dpkg-query -W -f='${binary:Package}\n' > "paquetes-$(date +%Y%m%d).txt"
```

No incluyas ese archivo en un repositorio si contiene información que no deba
publicarse.

## Ejecución

```bash
sudo apt update
apt list --upgradable
sudo apt upgrade
```

Para instalar:

```bash
apt-cache policy <paquete>
sudo apt install <paquete>
```

Para retirar, lee el resumen de APT antes de confirmar:

```bash
sudo apt remove <paquete>
```

Usa `purge` solo si también quieres eliminar configuración administrada por el
paquete y has revisado sus consecuencias.

## Verificación

```bash
apt list --upgradable
dpkg -s <paquete>
systemctl --failed --no-pager
```

Si instalaste un servicio, revisa su estado, puerto, logs y configuración.

## Rollback

APT no ofrece un rollback universal. Puedes reinstalar la versión disponible,
restaurar la configuración respaldada o retirar el paquete si la transacción lo
permite. Para una regresión de versión consulta `apt-cache policy` y no fuerces
un downgrade sin confirmar dependencias.

## Errores frecuentes

- `Could not get lock`: otra operación APT está activa; no borres el lock.
- Dependencias incumplidas: termina una configuración pendiente con cuidado y
  revisa la transacción propuesta.
- Repositorio sin firma: no desactives la verificación para avanzar.
- Poco espacio: resuelve almacenamiento antes de repetir la actualización.

## Fuentes

- [Ubuntu package management](https://ubuntu.com/server/docs/package-management/)
- [Ubuntu managing software](https://ubuntu.com/server/docs/tutorial/managing-software/)
