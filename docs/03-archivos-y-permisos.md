# Archivos, directorios, enlaces y permisos

## Inspección básica

```bash
ls -lah
stat archivo.txt
file archivo.txt
readlink -f ruta
```

`ls -l` muestra tipo, permisos, enlaces, propietario, grupo, tamaño y fecha.
`stat` ofrece metadatos más completos. `file` identifica el contenido mediante
firmas y no solo por la extensión.

## Crear, copiar, mover y eliminar

```bash
mkdir -p proyecto/{src,docs,tests}
touch proyecto/README.md
cp -a origen destino
mv archivo.txt proyecto/
rm archivo.txt
rmdir directorio-vacio
```

`rm` no tiene papelera por defecto. Antes de usar `rm -r`, lista el objetivo y
confirma la ruta absoluta. Nunca combines un patrón amplio con una variable no
validada.

## Permisos tradicionales

Un modo como `-rwxr-x---` se divide en:

1. tipo de archivo;
2. permisos del propietario;
3. permisos del grupo;
4. permisos de otros.

En archivos, `r` permite leer, `w` modificar y `x` ejecutar. En directorios,
`r` permite listar, `w` crear o eliminar entradas y `x` atravesar el directorio.
Tener `w` sin `x` en un directorio rara vez produce el acceso esperado.

```bash
ls -ld proyecto
chmod u+x script.sh
chmod g-w archivo.txt
chmod 640 privado.txt
```

El modo octal representa `r=4`, `w=2` y `x=1`. `640` significa `rw-` para el
propietario, `r--` para el grupo y ningún permiso para otros.

## Propietario, grupo y umask

```bash
id
namei -l /ruta/al/archivo
chown usuario:grupo archivo.txt
chgrp grupo archivo.txt
umask
```

`namei -l` ayuda a encontrar qué directorio padre bloquea el acceso. `chown`
requiere privilegios cuando no eres propietario. Evita `chown -R` o `chmod -R`
en rutas del sistema hasta haber inspeccionado el alcance.

`umask` resta permisos al crear archivos y directorios. No es una máscara que
se aplique a archivos existentes.

## Enlaces

```bash
ln archivo.txt copia-dura.txt
ln -s archivo.txt acceso-simbolico.txt
ls -li archivo.txt copia-dura.txt acceso-simbolico.txt
```

Un enlace duro apunta al mismo inode y normalmente no cruza filesystems. Un
enlace simbólico guarda una ruta y puede quedar roto si el destino desaparece.
`readlink -f` resuelve la ruta cuando los componentes existen.

## ACL y atributos especiales

Cuando los nueve bits no son suficientes, consulta si existen ACL:

```bash
getfacl archivo.txt
getfattr -d archivo.txt
```

En sistemas con el paquete correspondiente, `setfacl` permite permisos más
específicos. No elimines ACL o atributos extendidos sin entender qué servicio
los utiliza.

## Fuentes

- [GNU file permissions](https://www.gnu.org/s/coreutils/manual/html_node/File-permissions.html)
- [GNU setting permissions](https://www.gnu.org/s/coreutils/manual/html_node/Setting-Permissions.html)
- [Ubuntu user management](https://ubuntu.com/server/docs/how-to/security/user-management/)
