# Acción: corregir permisos y propietarios

## Objetivo

Diagnosticar un `Permission denied` y corregir únicamente el recurso que debe
ser accesible, sin abrir el sistema completo.

## Cuándo utilizarla

Cuando un usuario o servicio no puede leer, escribir o atravesar una ruta y ya
se confirmó que el recurso existe.

## Compatibilidad, privilegios y riesgo

- **Compatibilidad:** Linux/GNU; ACL, AppArmor y SELinux dependen del sistema.
- **Privilegio:** lectura como usuario normal; cambios con `sudo`.
- **Riesgo:** cambio persistente; recursividad puede afectar muchos archivos.

## Inspección previa

```bash
id <usuario>
namei -l /ruta/al/recurso
stat /ruta/al/recurso
getfacl -p /ruta/al/recurso 2>/dev/null
findmnt -no TARGET,OPTIONS /ruta/al/recurso
```

Si el proceso es un servicio:

```bash
systemctl show <servicio> -p User -p Group -p DynamicUser
```

Identifica el usuario efectivo, cada directorio padre, propietario, grupo,
ACL, opciones de montaje y política MAC antes de cambiar un modo.

## Respaldo y precauciones

Para una ruta pequeña, registra metadatos antes del cambio:

```bash
stat /ruta/al/recurso > permisos-antes.txt
getfacl -R -p /ruta/limitada > acl-antes.txt 2>/dev/null
```

No uses `chmod -R 777`. No uses `chown -R` sobre `/`, `/usr`, `/var` o un
directorio que contenga datos de varios servicios sin un plan específico.

## Ejecución

Para un archivo puntual:

```bash
sudo chown <usuario>:<grupo> /ruta/al/recurso
sudo chmod 640 /ruta/al/recurso
```

Para un directorio de trabajo bien delimitado:

```bash
sudo chown <usuario>:<grupo> /ruta/limitada
sudo chmod u+rwx,g+rx,o-rwx /ruta/limitada
```

Añade permisos de escritura al grupo solo si el flujo lo necesita. En un
directorio, `x` significa poder atravesarlo y no ejecutar el directorio.

## Verificación

```bash
namei -l /ruta/al/recurso
sudo -u <usuario> test -r /ruta/al/recurso
sudo -u <usuario> test -w /ruta/al/recurso
sudo -u <usuario> test -x /ruta/al/recurso
```

Repite la operación real del servicio o usuario y revisa su log.

## Rollback

Restaura propietario, grupo y modo usando los valores registrados. Si la ruta
contiene muchos archivos y no existe un respaldo de metadatos completo,
detente: cambiar permisos recursivos sin un estado anterior verificable no
permite un rollback seguro.

## Errores frecuentes

- El archivo tiene permisos correctos, pero un directorio padre no tiene `x`.
- Una ACL concede o niega acceso adicional.
- AppArmor o SELinux bloquea la operación aunque el modo parezca correcto.
- El filesystem está montado como `ro`.
- El servicio corre con un usuario distinto al que se estaba probando.

## Fuentes

- [GNU file permissions](https://www.gnu.org/s/coreutils/manual/html_node/File-permissions.html)
- [GNU setting permissions](https://www.gnu.org/s/coreutils/manual/html_node/Setting-Permissions.html)
