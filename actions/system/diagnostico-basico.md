# Acción: capturar diagnóstico básico

## Objetivo

Obtener una fotografía inicial del sistema sin reiniciar, instalar paquetes ni
modificar configuración.

## Cuándo utilizarla

Antes de investigar lentitud, errores de arranque, falta de espacio, pérdida de
red o un servicio que no responde.

## Compatibilidad, privilegios y riesgo

- **Compatibilidad:** Linux con utilidades habituales; algunos comandos usan
  systemd o herramientas específicas de Ubuntu.
- **Privilegio:** empieza como usuario normal; usa `sudo` solo si una salida lo
  requiere.
- **Riesgo:** lectura. La salida puede contener datos sensibles.

## Inspección previa

Confirma el equipo y la sesión:

```bash
hostname
whoami
pwd
date --iso-8601=seconds
```

No envíes la salida a terceros sin revisar nombres, IPs, rutas privadas,
usuarios, dominios internos y mensajes de log.

## Respaldo y precauciones

La acción no modifica el sistema, pero si guardas la salida en un archivo,
revisa y protege ese archivo porque puede contener datos del equipo, usuarios,
direcciones internas y nombres de servicios.

## Ejecución

```bash
cat /etc/os-release
uname -a
uptime
free -h
df -hT
df -ih
ip -br address
ip route
systemctl --failed --no-pager
journalctl -b -p warning..alert --no-pager
```

Si existe una sospecha específica, amplía sin recopilar todo el journal:

```bash
systemctl status <servicio> --no-pager
journalctl -u <servicio> --since '30 minutes ago' --no-pager
ps -eo pid,ppid,user,stat,%cpu,%mem,etime,comm,args --sort=-%cpu | head -n 20
```

## Verificación

La captura es útil si contiene, como mínimo, distribución, kernel, uptime,
memoria, espacio, red, unidades fallidas y advertencias recientes. Anota el
momento de captura y el síntoma que estabas investigando.

## Rollback

No hay cambios del sistema. Elimina el archivo de captura si lo guardaste y ya
no es necesario; revisa su contenido antes de compartirlo.

## Errores frecuentes

- `systemctl` o `journalctl` no existen: el sistema puede no usar systemd.
- No aparecen todos los logs: faltan privilegios o el journal no es persistente.
- `df` parece normal, pero no se crean archivos: revisa inodos con `df -ih`.
- El problema no aparece en el journal: revisa logs de la aplicación o del
  proveedor externo.

## Fuentes

- [Ubuntu Server Documentation](https://ubuntu.com/server/docs/)
- [Red Hat troubleshooting logs](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_basic_system_settings/assembly_troubleshooting-problems-using-log-files_configuring-basic-system-settings)
