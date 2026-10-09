# Usuarios, grupos y privilegios

## Identidad efectiva

```bash
whoami
id
groups
getent passwd "$USER"
getent group sudo
```

El usuario efectivo es el que determina muchos permisos de una orden. Los
grupos suplementarios amplían los recursos accesibles, por ejemplo `sudo`,
`adm` o grupos específicos de dispositivos.

## Crear y modificar usuarios

En Ubuntu se suele preferir `adduser` para la interacción guiada:

```bash
sudo adduser <usuario>
sudo usermod -aG <grupo> <usuario>
sudo passwd -S <usuario>
```

`usermod -aG` debe conservar `-a`: omitirlo puede reemplazar los grupos
suplementarios existentes. La sesión del usuario puede necesitar cerrarse y
abrirse de nuevo para recibir el grupo actualizado.

Para una cuenta técnica, define explícitamente si necesita login interactivo,
home, shell y expiración. No habilites más privilegios de los necesarios.

## Grupos y directorios compartidos

```bash
getent group <grupo>
sudo groupadd <grupo>
sudo gpasswd -a <usuario> <grupo>
id <usuario>
```

Para un directorio compartido se debe decidir propietario, grupo, permisos de
escritura y si el bit setgid debe conservar el grupo en nuevos archivos. La
decisión depende del flujo de trabajo y no debe resolverse con `chmod 777`.

## sudo

Comprueba si una cuenta puede elevar privilegios:

```bash
sudo -l -U <usuario>
```

Las reglas persistentes deben editarse con `visudo` o con un archivo controlado
en `/etc/sudoers.d/`, porque valida sintaxis antes de instalar la configuración.
El archivo debe tener propietario root y permisos restrictivos.

```bash
sudo visudo
sudo visudo -f /etc/sudoers.d/<regla>
```

No concedas `NOPASSWD`, acceso a una shell completa o comodines amplios sin
documentar el riesgo. Un programa aparentemente inocuo puede permitir ejecutar
otra orden con privilegios.

## Bloqueo, expiración y eliminación

```bash
sudo passwd -l <usuario>
sudo passwd -u <usuario>
sudo chage -l <usuario>
sudo userdel -r <usuario>
```

`passwd -l` bloquea la autenticación por contraseña, pero no necesariamente
termina sesiones existentes. `userdel -r` elimina el home y el spool local:
confirma primero que no existan datos que deban conservarse.

## Diagnóstico de permisos

Cuando una aplicación no puede leer un archivo, separa:

1. usuario y grupos del proceso;
2. permisos del archivo;
3. permisos de cada directorio padre;
4. ACL o atributos extendidos;
5. AppArmor, SELinux u otra política MAC;
6. montaje con opciones como `ro`, `noexec` o `nosuid`.

```bash
ps -o user,group,egroup,comm -p <pid>
namei -l /ruta/al/recurso
ls -l /ruta/al/recurso
```

## Fuentes

- [Ubuntu user management](https://ubuntu.com/server/docs/how-to/security/user-management/)
- [Ubuntu welcome to the terminal](https://ubuntu.com/server/docs/tutorial/welcome-to-the-terminal/)
- [Red Hat managing users](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_basic_system_settings/managing-users-and-groups_configuring-basic-system-settings)
