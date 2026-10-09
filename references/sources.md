# Fuentes y política de referencia

Esta guía usa fuentes de distinto nivel, pero no las trata como equivalentes.
La documentación oficial y los manuales locales definen el comportamiento
esperado; los foros ayudan a reconocer síntomas y casos reales.

## Jerarquía de fuentes

1. Documentación oficial de la distribución y del proyecto que mantiene la
   herramienta.
2. Páginas `man` instaladas en el sistema y manuales GNU.
3. Documentación de proveedores o proyectos integrados, como Netplan,
   NetworkManager y systemd.
4. Foros y comunidades, utilizados para contexto, variantes y troubleshooting.

Cuando una recomendación comunitaria contradice a una fuente oficial, la guía
debe explicitar la diferencia y no presentar el caso del foro como regla
universal.

## Ubuntu y Debian

- [Ubuntu Server Documentation](https://ubuntu.com/server/docs/): índice de
  instalación, administración, software, red, seguridad y rendimiento.
- [Welcome to the terminal](https://ubuntu.com/server/docs/tutorial/welcome-to-the-terminal/):
  terminal, usuarios, grupos, permisos y `sudo`.
- [Install and manage packages](https://ubuntu.com/server/docs/package-management/):
  `apt`, `dpkg`, repositorios, actualizaciones y advertencias sobre fuentes de
  terceros.
- [User management](https://ubuntu.com/server/docs/how-to/security/user-management/):
  cuentas locales, grupos y permisos de directorios personales.
- [Firewalls](https://ubuntu.com/server/docs/how-to/security/firewalls/):
  UFW, reglas, perfiles de aplicaciones y comprobación del estado.
- [Networking](https://ubuntu.com/server/docs/how-to/networking/): índice de
  configuración, DNS y herramientas de red.
- [Configuring networks](https://ubuntu.com/server/docs/explanation/networking/configuring-networks/):
  Netplan, interfaces y diferencias entre Desktop y Server.
- [Netplan CLI manpage](https://manpages.ubuntu.com/manpages/noble/man8/netplan.8.html):
  `generate`, `apply`, `try`, `get`, `set` y `status`.
- [Ubuntu swapon and swapoff manpage](https://manpages.ubuntu.com/manpages/noble/man8/swapon.8.html):
  activación, desactivación, archivos Swap, prioridades y restricciones.
- [Ubuntu automatic updates](https://ubuntu.com/server/docs/how-to/software/automatic-updates/):
  timers, actualizaciones de seguridad y reinicios automáticos.

## Linux, GNU, kernel y systemd

- [GNU Coreutils manual](https://www.gnu.org/software/coreutils/manual/):
  archivos, permisos, procesos, redirecciones y utilidades fundamentales.
- [GNU file permissions](https://www.gnu.org/s/coreutils/manual/html_node/File-permissions.html):
  bits de modo, propietario, grupo y permisos especiales.
- [systemd journalctl](https://www.freedesktop.org/software/systemd/man/255/journalctl.html):
  consulta y filtrado del journal.
- [Linux Kernel Administrator’s Guide](https://www.kernel.org/doc/html/latest/admin-guide/index.html):
  administración del kernel, `/proc`, `sysfs` y parámetros del sistema.

## Red Hat Enterprise Linux y Fedora

- [RHEL 9 documentation](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9):
  referencia empresarial para administración, red, seguridad y almacenamiento.
- [Managing systemd](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_basic_system_settings/managing-systemd_configuring-basic-system-settings):
  unidades, arranque, start, stop, restart y enable.
- [Troubleshooting with log files](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_basic_system_settings/assembly_troubleshooting-problems-using-log-files_configuring-basic-system-settings):
  filtros de `journalctl`, boots, unidades y prioridades.
- [Systemd network targets and services](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_and_managing_networking/systemd-network-targets-and-services_configuring-and-managing-networking):
  `network.target`, `network-online.target` y dependencias de red.

## Comunidad

- [Ask Ubuntu](https://askubuntu.com/): casos prácticos y troubleshooting
  específico de Ubuntu.
- [Unix & Linux Stack Exchange](https://unix.stackexchange.com/): preguntas
  comparativas sobre Linux, Unix, shell y administración.

Las respuestas comunitarias deben contrastarse con `man`, documentación de la
versión instalada y documentación del proveedor antes de convertirlas en una
acción del repositorio.

## Cómo citar una fuente nueva

Cada módulo o acción debe enlazar la fuente junto a la explicación que sustenta
una decisión técnica. También debe añadirse aquí con:

- nombre del proyecto o institución;
- título de la página;
- URL directa;
- tema que respalda;
- fecha de consulta si el contenido es especialmente sensible a la versión.
