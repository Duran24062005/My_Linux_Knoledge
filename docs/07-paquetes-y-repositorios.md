# Paquetes, repositorios y actualizaciones

## Qué resuelve un gestor de paquetes

Un gestor de paquetes mantiene un inventario de archivos, versiones,
dependencias, scripts de instalación y origen. No es lo mismo instalar un
paquete desde un repositorio firmado que ejecutar un instalador descargado de
una página desconocida.

## Ubuntu y Debian: APT y dpkg

```bash
apt policy <paquete>
apt show <paquete>
apt search <termino>
dpkg -l <paquete>
dpkg -L <paquete>
```

Para actualizar un Ubuntu de forma habitual:

```bash
sudo apt update
sudo apt upgrade
```

`apt update` actualiza el índice local. `apt upgrade` instala actualizaciones
disponibles sin retirar paquetes en el flujo habitual. Para scripts se suele
preferir `apt-get`, porque `apt` está pensado principalmente para uso
interactivo.

Operaciones frecuentes:

```bash
sudo apt install <paquete>
sudo apt remove <paquete>
sudo apt purge <paquete>
sudo apt autoremove
```

`purge` elimina también configuración administrada por el paquete y
`autoremove` puede retirar dependencias que ya no se consideran necesarias.
Revisa el resumen que APT muestra antes de aceptar.

## Repositorios y confianza

```bash
apt-cache policy
grep -R --line-number --no-filename '^\s*\(deb\|Types:\)' /etc/apt/sources.list.d /etc/apt/sources.list 2>/dev/null
```

Ubuntu moderno puede usar archivos deb822 en `/etc/apt/sources.list.d/`.
Añadir repositorios de terceros amplía el riesgo de incompatibilidad y cadena
de suministro. Registra origen, clave, propósito y cómo deshabilitarlo.

## RHEL y Fedora: DNF y RPM

```bash
dnf info <paquete>
dnf search <termino>
rpm -q <paquete>
rpm -ql <paquete>
sudo dnf install <paquete>
sudo dnf upgrade
sudo dnf remove <paquete>
```

Los nombres de paquetes y servicios no siempre coinciden con Ubuntu. Consulta
la [matriz de compatibilidad](distros/compatibilidad.md) antes de traducir una
acción.

## Después de actualizar

```bash
apt list --upgradable
systemctl --failed
needrestart -b 2>/dev/null || true
```

Un reinicio puede ser necesario cuando cambia el kernel o una biblioteca
fundamental. En servidores, consulta la política de mantenimiento antes de
reiniciar servicios automáticamente.

## Qué no hacer

- No mezcles repositorios de distintas versiones de Ubuntu.
- No uses `sudo pip install` para sustituir el gestor del sistema sin una razón
  documentada.
- No elimines paquetes base solo porque no reconoces su nombre.
- No aceptes una transacción sin leer paquetes que se instalarán, retirarán o
  actualizarán.

## Fuentes

- [Ubuntu package management](https://ubuntu.com/server/docs/package-management/)
- [Ubuntu managing software tutorial](https://ubuntu.com/server/docs/tutorial/managing-software/)
- [RHEL 9 documentation](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9)
