# Fundamentos de Linux

## Linux, distribución y sistema operativo

Linux es el kernel: administra CPU, memoria, dispositivos, procesos y la
frontera entre software y hardware. Una distribución combina el kernel con
herramientas de usuario, bibliotecas, instalador, sistema de paquetes,
configuración y una política de mantenimiento.

Ubuntu y Debian usan paquetes `.deb` y APT. RHEL, Fedora y derivados usan
paquetes RPM y normalmente DNF. La presencia de systemd o Bash es frecuente,
pero no debe confundirse con una garantía universal.

Consulta tu sistema antes de asumir:

```bash
cat /etc/os-release
uname -r
uname -m
hostnamectl
```

`/etc/os-release` identifica la distribución y su versión. `uname -r` muestra
la versión del kernel en ejecución y `uname -m` la arquitectura. `hostnamectl`
resume nombre del equipo, kernel, arquitectura y, en sistemas compatibles,
información del entorno de virtualización.

## Kernel, espacio de usuario y shell

- **Kernel:** ejecuta con privilegios altos y controla recursos.
- **Espacio de usuario:** contiene aplicaciones, servicios, bibliotecas y
  herramientas administrativas.
- **Shell:** interpreta comandos y conecta programas mediante pipes,
  redirecciones, variables y expansión de rutas.
- **Terminal:** interfaz que permite interactuar con una shell; no es la shell
  en sí misma.

Comprueba la shell actual y la shell de inicio configurada para tu usuario:

```bash
printf 'shell actual: %s\n' "$SHELL"
ps -p $$ -o pid,ppid,comm,args
getent passwd "$USER"
```

La variable `SHELL` expresa la shell de inicio declarada, pero el proceso que
está ejecutándose puede ser otra shell iniciada manualmente. `getent` consulta
la base de cuentas configurada por NSS y no solo un archivo local.

## Usuarios, root y sudo

Linux asigna un UID a cada usuario y un GID a cada grupo. `root` tiene UID 0 y
puede modificar casi cualquier recurso, por lo que una orden root equivocada
puede dañar el sistema rápidamente.

`sudo` ejecuta una orden concreta con privilegios elevados según una política.
No convierte automáticamente todas las órdenes siguientes en root. Revisa la
identidad efectiva cuando una acción dependa de privilegios:

```bash
id
whoami
sudo -v
sudo id
```

## El filesystem

Linux presenta dispositivos, pseudo-filesystems y archivos regulares mediante
una jerarquía única que comienza en `/`. Algunas rutas frecuentes son:

| Ruta | Propósito habitual |
| --- | --- |
| `/` | Raíz de la jerarquía. |
| `/home` | Directorios de usuarios. |
| `/etc` | Configuración del sistema y servicios. |
| `/var` | Datos variables, cachés, colas y logs. |
| `/usr` | Programas, bibliotecas y datos compartidos. |
| `/tmp` | Temporales, normalmente con limpieza automática. |
| `/dev` | Dispositivos expuestos al espacio de usuario. |
| `/proc` | Información dinámica de procesos y kernel. |
| `/sys` | Información y configuración de dispositivos y kernel. |
| `/run` | Estado efímero desde el arranque actual. |

No todas las rutas son un directorio normal en disco. `/proc`, `/sys` y `/run`
pueden mostrar información generada por el kernel o por servicios.

## Estados de un sistema

Al investigar una incidencia, separa estas preguntas:

1. ¿El hardware o la máquina virtual está disponible?
2. ¿El kernel arrancó y detectó el recurso?
3. ¿El proceso existe?
4. ¿El servicio está activo y habilitado?
5. ¿El usuario tiene permiso?
6. ¿La red, el almacenamiento o una dependencia externa funciona?

Esta separación evita reiniciar o reinstalar antes de conocer la causa.

## Fuentes

- [Ubuntu Server Documentation](https://ubuntu.com/server/docs/)
- [Linux Kernel Administrator’s Guide](https://www.kernel.org/doc/html/latest/admin-guide/index.html)
- [GNU Coreutils manual](https://www.gnu.org/software/coreutils/manual/)
