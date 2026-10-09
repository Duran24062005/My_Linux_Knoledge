# Tabla de comandos

Consulta primero la compatibilidad en la [matriz de distribuciones](distros/compatibilidad.md)
y luego la página `man` del sistema instalado.

## Orientación y ayuda

| Comando | Propósito | Compatibilidad |
| --- | --- | --- |
| `pwd` | Mostrar directorio actual. | POSIX/GNU |
| `ls` | Listar entradas. | POSIX/GNU |
| `man` | Leer manual local. | Linux frecuente |
| `type` | Identificar alias, función o binario. | Bash |
| `command -v` | Localizar una orden. | POSIX/Bash |
| `history` | Consultar historial de shell. | Bash y otras shells |

## Archivos y permisos

| Comando | Propósito | Riesgo |
| --- | --- | --- |
| `find` | Buscar por nombre, tipo, tiempo o permiso. | Lectura; acciones pueden ser destructivas |
| `stat` | Consultar metadatos. | Lectura |
| `cp` | Copiar archivos o directorios. | Puede sobrescribir |
| `mv` | Mover o renombrar. | Puede reemplazar destinos |
| `rm` | Eliminar. | Destructivo |
| `chmod` | Cambiar permisos. | Puede bloquear acceso |
| `chown` | Cambiar propietario y grupo. | Puede romper servicios |
| `ln` | Crear enlaces. | Puede crear referencias equivocadas |

## Procesos y servicios

| Comando | Propósito | Compatibilidad |
| --- | --- | --- |
| `ps` | Capturar procesos. | Linux/procps |
| `top` | Observar procesos y recursos. | Linux frecuente |
| `free` | Mostrar memoria y Swap. | procps |
| `kill` | Enviar señales a un PID. | POSIX/Linux |
| `pgrep` | Buscar procesos por atributos. | procps |
| `systemctl` | Administrar unidades systemd. | systemd |
| `journalctl` | Consultar logs del journal. | systemd |
| `systemd-analyze` | Analizar arranque y unidades. | systemd |

## Paquetes

| Ubuntu/Debian | RHEL/Fedora | Función |
| --- | --- | --- |
| `apt search` | `dnf search` | Buscar paquetes. |
| `apt show` | `dnf info` | Consultar metadatos. |
| `apt install` | `dnf install` | Instalar. |
| `apt remove` | `dnf remove` | Retirar. |
| `apt update` | `dnf makecache` | Actualizar metadatos. |
| `apt upgrade` | `dnf upgrade` | Actualizar paquetes. |
| `dpkg -L` | `rpm -ql` | Listar archivos instalados. |

## Red y almacenamiento

| Comando | Propósito |
| --- | --- |
| `ip` | Direcciones, interfaces y rutas. |
| `ss` | Sockets y puertos. |
| `nmcli` | NetworkManager desde CLI. |
| `netplan` | Configuración de red de Ubuntu. |
| `resolvectl` | Estado y consultas de systemd-resolved. |
| `lsblk` | Dispositivos y filesystems. |
| `findmnt` | Montajes activos. |
| `df` | Espacio e inodos de filesystems. |
| `du` | Uso de espacio por directorio. |
| `swapon` | Activar o inspeccionar Swap. |
| `swapoff` | Desactivar Swap. |

## Seguridad

| Comando | Propósito | Nota |
| --- | --- | --- |
| `sudo` | Elevar una orden según política. | Revisar el comando completo. |
| `ufw` | Firewall sencillo de Ubuntu. | No asumir en RHEL/Fedora. |
| `firewall-cmd` | Firewall de firewalld. | Común en RHEL/Fedora. |
| `getenforce` | Estado de SELinux. | RHEL/Fedora. |
| `aa-status` | Estado de AppArmor. | Ubuntu cuando está instalado. |
