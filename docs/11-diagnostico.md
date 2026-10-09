# Diagnóstico reproducible y troubleshooting

## Regla de oro

No empieces por una solución favorita. Define el síntoma, el alcance, el
momento en que comenzó y una condición observable que permita confirmar o
descartar hipótesis.

## Flujo general

1. **Describe:** qué falla, para quién y desde cuándo.
2. **Reproduce:** ejecuta una prueba pequeña y segura.
3. **Identifica:** sistema operativo, versión, kernel, usuario, servicio y
   entorno.
4. **Observa:** procesos, recursos, red, permisos, logs y dependencias.
5. **Formula:** una hipótesis comprobable.
6. **Cambia:** una sola variable, con respaldo si es persistente.
7. **Verifica:** prueba el resultado y el efecto secundario.
8. **Registra:** causa, solución, rollback y evidencia.

## Captura inicial segura

```bash
date --iso-8601=seconds
hostnamectl
cat /etc/os-release
uname -a
uptime
free -h
df -hT
ip -br address
systemctl --failed
journalctl -b -p warning..alert --no-pager
```

No compartas esta salida sin redactar identidad del equipo, IPs internas,
nombres de usuario, rutas privadas y mensajes que contengan credenciales.

## Árbol rápido de hipótesis

| Síntoma | Primeras comprobaciones |
| --- | --- |
| Comando no encontrado | `command -v`, paquete propietario, `$PATH`. |
| Permiso denegado | `id`, `namei -l`, ACL, AppArmor o SELinux. |
| Servicio caído | `systemctl status`, `journalctl -u`, dependencias, puerto. |
| Sistema lento | `uptime`, `top`, `free`, `vmstat`, `iostat`, logs. |
| Sin red | interfaz, dirección, ruta, DNS, firewall y destino. |
| Disco lleno | `df -h`, `df -i`, `du`, archivos borrados abiertos. |
| Arranque lento | `systemd-analyze`, `critical-chain`, journal del boot. |

## Arranque y kernel

```bash
systemd-analyze
systemd-analyze blame
systemd-analyze critical-chain
journalctl --list-boots
journalctl -k -b -1 --no-pager
```

Las órdenes de rendimiento muestran correlaciones, no una causa automática.
Un servicio que tarda en arrancar puede estar esperando red, disco o un
recurso externo.

## Qué registrar en un informe

- fecha y zona horaria;
- distribución, versión, kernel y arquitectura;
- comando exacto y código de salida;
- salida relevante sin secretos;
- cambios recientes;
- hipótesis probadas;
- acción aplicada y resultado;
- rollback disponible;
- fuente o página de manual consultada.

## Fuentes

- [Red Hat troubleshooting with log files](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_basic_system_settings/assembly_troubleshooting-problems-using-log-files_configuring-basic-system-settings)
- [Ubuntu server documentation](https://ubuntu.com/server/docs/)
- [Linux Kernel Administrator’s Guide](https://www.kernel.org/doc/html/latest/admin-guide/index.html)
