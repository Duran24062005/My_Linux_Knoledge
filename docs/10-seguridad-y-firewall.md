# Seguridad básica, sudo, actualizaciones y firewall

## Principio de mínimo privilegio

Concede a cada usuario, proceso y servicio únicamente lo que necesita. La
seguridad no consiste en ejecutar todo como root ni en poner `chmod 777` para
evitar un error de permisos.

Revisa la superficie de exposición:

```bash
id
sudo -l
ss -lntup
systemctl list-unit-files --state=enabled
```

Un puerto escuchando en `0.0.0.0` o `::` puede ser accesible desde la red,
mientras que uno ligado a `127.0.0.1` normalmente queda local. Confirma IPv4 e
IPv6 por separado.

## Actualizaciones

Mantén actualizado el índice y revisa la transacción antes de aceptarla:

```bash
sudo apt update
apt list --upgradable
sudo apt upgrade
```

En servidores, documenta ventana de mantenimiento, servicios afectados,
necesidad de reinicio y método de validación posterior.

## UFW en Ubuntu

```bash
sudo ufw status verbose
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow OpenSSH
sudo ufw enable
```

Antes de habilitar UFW en una sesión SSH, permite el puerto y servicio de
administración que realmente utilizas. Una regla amplia puede dejar el equipo
expuesto; una regla ausente puede cortar tu acceso.

Consulta reglas numeradas:

```bash
sudo ufw status numbered
sudo ufw delete <numero>
sudo ufw delete allow 22/tcp
```

No borres una regla sin confirmar que no sea la única vía de administración.

## Firewalld y SELinux en RHEL/Fedora

```bash
sudo firewall-cmd --state
sudo firewall-cmd --list-all
getenforce
ausearch -m AVC -ts recent 2>/dev/null
```

RHEL/Fedora no debe administrarse con UFW como si fuera Ubuntu. `firewalld`
gestiona zonas y servicios; SELinux aplica una política de control de acceso
obligatorio. Si una aplicación recibe `Permission denied`, revisa permisos
tradicionales y la política MAC.

## Logs y cambios de seguridad

Registra qué cambiaste, por qué, cuándo y cómo revertirlo. No publiques la
salida de logs sin revisar direcciones, nombres, tokens, rutas privadas y
datos de usuarios.

## Fuentes

- [Ubuntu firewall documentation](https://ubuntu.com/server/docs/how-to/security/firewalls/)
- [Ubuntu automatic updates](https://ubuntu.com/server/docs/how-to/software/automatic-updates/)
- [RHEL security hardening](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html-single/security_hardening/index)
- [RHEL SELinux](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/using_selinux/index)
