# Acción: gestionar un servicio y revisar sus logs

## Objetivo

Inspeccionar, iniciar, detener o reiniciar un servicio systemd con evidencia
antes y después del cambio.

## Cuándo utilizarla

Cuando una unidad está fallida, una configuración acaba de cambiar o un
servicio debe habilitarse o deshabilitarse durante una ventana controlada.

## Compatibilidad, privilegios y riesgo

- **Compatibilidad:** sistemas con systemd, como Ubuntu y RHEL/Fedora actuales.
- **Privilegio:** consulta como usuario normal; cambios con `sudo`.
- **Riesgo:** reiniciar puede interrumpir usuarios; deshabilitar puede afectar
  el arranque.

## Inspección previa

```bash
systemctl status <servicio> --no-pager
systemctl is-enabled <servicio>
systemctl show <servicio> -p FragmentPath -p User -p Group -p MainPID
systemctl cat <servicio>
journalctl -u <servicio> -b --no-pager -n 100
```

Confirma el nombre exacto de la unidad y si la acción debe afectar solo la
sesión actual o también futuros arranques.

## Respaldo y precauciones

Si editarás configuración, copia el archivo que administra el paquete o crea un
drop-in con `systemctl edit`. No edites directamente una unidad entregada por
el paquete si un override resuelve el cambio.

```bash
sudo systemctl edit <servicio>
sudo systemctl daemon-reload
```

## Ejecución

Para una acción temporal:

```bash
sudo systemctl start <servicio>
sudo systemctl stop <servicio>
sudo systemctl restart <servicio>
```

Para modificar el arranque:

```bash
sudo systemctl enable <servicio>
sudo systemctl disable <servicio>
```

Usa `enable --now` solo cuando quieras habilitar e iniciar al mismo tiempo.

## Verificación

```bash
systemctl is-active <servicio>
systemctl is-enabled <servicio>
systemctl status <servicio> --no-pager
journalctl -u <servicio> --since '5 minutes ago' --no-pager
```

Una unidad `active` no demuestra que la aplicación atienda correctamente.
Comprueba el puerto, endpoint o función real si corresponde.

## Rollback

Si el cambio fue un reinicio, inicia o reinicia de nuevo solo después de
corregir la causa. Si fue `enable` o `disable`, invierte la orden. Para un
override, elimina únicamente el archivo creado y ejecuta `daemon-reload`:

```bash
sudo systemctl revert <servicio>
sudo systemctl daemon-reload
```

Confirma qué archivos revertirá `systemctl revert` antes de aceptarlo.

## Errores frecuentes

- `Unit not found`: el paquete no está instalado o el nombre es incorrecto.
- `failed`: consulta `journalctl -u`; no repitas reinicios sin analizar.
- `address already in use`: otro proceso ocupa el puerto.
- El servicio arranca manualmente, pero no en boot: revisa `enable`, usuario,
  rutas absolutas y dependencias.

## Fuentes

- [Ubuntu systemd package files](https://ubuntu.com/server/docs/changing-package-files/)
- [systemd journalctl](https://www.freedesktop.org/software/systemd/man/255/journalctl.html)
- [Red Hat managing systemd](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_basic_system_settings/managing-systemd_configuring-basic-system-settings)
