# Matriz de compatibilidad entre distribuciones

La sintaxis de muchas utilidades es común porque proviene de POSIX, GNU,
systemd o iproute2. La instalación de paquetes, el firewall, la configuración
de red y algunas rutas de configuración sí dependen de la distribución.

## Estado inicial

| Capacidad | Ubuntu/Debian | RHEL/Fedora | Linux común | Nota |
| --- | --- | --- | --- | --- |
| Paquetes | `apt`, `apt-get`, `dpkg` | `dnf`, `yum`, `rpm` | No universal | `apt` es interactivo; para scripts se prefiere `apt-get`. |
| Servicios | `systemctl` | `systemctl` | No universal | La unidad puede cambiar de nombre o no existir. |
| Logs | `journalctl`, `/var/log` | `journalctl`, `/var/log` | No universal | La persistencia y permisos del journal dependen de la configuración. |
| Red | Netplan, NetworkManager, `ip` | NetworkManager, `nmcli`, `ip` | `ip` suele estar disponible | No editar una ruta de red sin identificar el backend activo. |
| Firewall | `ufw`, nftables | `firewalld`, nftables | nftables o herramienta del sistema | No asumir que UFW existe fuera de Ubuntu. |
| MAC | AppArmor | SELinux | Depende de la distribución | No trasladar reglas de AppArmor a SELinux. |
| Swap | `swapon`, `swapoff`, `/etc/fstab` | `swapon`, `swapoff`, `/etc/fstab` | Kernel + util-linux | La activación persistente puede generar unidades `.swap`. |
| Shell | Bash, Dash y otras | Bash, otras | POSIX shell | Un script debe declarar qué shell necesita. |

## Ubuntu Desktop frente a Ubuntu Server

- Ambos pueden usar APT, systemd, Netplan, `ip`, `ss`, `journalctl` y las
  utilidades GNU.
- Desktop suele tener NetworkManager integrado con la interfaz gráfica; Server
  puede usar `systemd-networkd` según la instalación y el renderer de Netplan.
- Desktop puede tener servicios de sesión, audio, portal y entorno gráfico que
  no existen en una instalación Server.
- Una acción de servidor no debe suponer que hay una pantalla local, mientras
  que una acción de Desktop no debe asumir que existe un servicio de red
  dedicado.

## Regla para agregar otra distribución

Una adaptación nueva debe indicar:

1. distribución y versión probadas;
2. gestor de paquetes y equivalente del comando;
3. backend de red y firewall;
4. rutas de configuración relevantes;
5. diferencias de permisos o seguridad;
6. fuente oficial que confirma el comportamiento.

Hasta que una adaptación tenga evidencia suficiente, debe marcarse como
`pendiente` y no como compatible.
