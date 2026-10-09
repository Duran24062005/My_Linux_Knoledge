# Servicios, systemd y logs

## Unidades y estados

systemd administra unidades como servicios, sockets, timers, mounts y swaps.
Los estados de una unidad y los estados del proceso que inició deben
interpretarse juntos.

```bash
systemctl status <servicio>
systemctl is-active <servicio>
systemctl is-enabled <servicio>
systemctl list-units --type=service --state=failed
```

- `active`: la unidad está activa según systemd.
- `inactive`: no está activa; puede ser normal.
- `failed`: una activación terminó con error o quedó marcada como fallida.
- `enabled`: está configurada para iniciar mediante uno o más targets.
- `masked`: su inicio está bloqueado mediante un enlace a `/dev/null`.

## Ciclo de operación

```bash
sudo systemctl start <servicio>
sudo systemctl stop <servicio>
sudo systemctl restart <servicio>
sudo systemctl reload <servicio>
sudo systemctl enable --now <servicio>
sudo systemctl disable <servicio>
```

`start` y `stop` cambian la sesión actual. `enable` cambia el arranque futuro.
`enable --now` hace ambas cosas. `reload` solo funciona si el servicio soporta
recarga de configuración.

## Leer una unidad sin editarla

```bash
systemctl cat <servicio>
systemctl show <servicio>
systemctl list-dependencies <servicio>
systemctl list-unit-files --state=enabled
```

Los archivos proporcionados por paquetes suelen estar bajo `/usr/lib` o
`/lib`, mientras los overrides administrados localmente deben vivir en
`/etc/systemd/system`. No edites directamente el archivo entregado por el
paquete si puedes usar un drop-in:

```bash
sudo systemctl edit <servicio>
sudo systemctl daemon-reload
```

## journalctl

```bash
journalctl -b
journalctl -b -1
journalctl -u <servicio> --since '30 minutes ago'
journalctl -p warning..alert -b
journalctl -k -b
journalctl -f
```

Usa `--no-pager` en diagnósticos automatizados. El acceso a logs de todo el
sistema puede requerir root o pertenecer a grupos como `adm` o
`systemd-journal`.

## Flujo ante un servicio fallido

1. `systemctl status` para obtener estado, unidad, PID y últimas líneas.
2. `journalctl -u` para ampliar el periodo y conservar el contexto.
3. `systemctl cat` para conocer comando, usuario, entorno y dependencias.
4. Verificar puertos, rutas, permisos, variables y archivos de configuración.
5. Cambiar una sola causa probable.
6. Recargar o reiniciar y volver a verificar.

No borres logs ni reinicies repetidamente antes de capturar evidencia.

## Fuentes

- [Ubuntu changing package files and systemd](https://ubuntu.com/server/docs/changing-package-files/)
- [systemd journalctl](https://www.freedesktop.org/software/systemd/man/255/journalctl.html)
- [Red Hat managing systemd](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_basic_system_settings/managing-systemd_configuring-basic-system-settings)
- [Red Hat troubleshooting logs](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_basic_system_settings/assembly_troubleshooting-problems-using-log-files_configuring-basic-system-settings)
