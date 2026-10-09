# Terminal, shell y composición de comandos

## Sintaxis básica

Una orden suele tener esta forma:

```text
comando [opciones] [argumentos]
```

Las opciones cambian el comportamiento del comando y los argumentos identifican
recursos. Consulta la ayuda local antes de copiar una opción desde Internet:

```bash
comando --help
man comando
info comando
apropos palabra-clave
```

`man` es la referencia más cercana a la versión instalada. `apropos` busca en
los nombres y descripciones de páginas de manual.

## Rutas y expansión

- Una ruta que empieza por `/` es absoluta.
- Una ruta que no empieza por `/` es relativa al directorio actual.
- `.` representa el directorio actual.
- `..` representa el directorio padre.
- `~` suele expandirse al home del usuario actual.
- `*`, `?` y `[]` son patrones de expansión de la shell.

Practica sin modificar datos:

```bash
pwd
printf '%s\n' . .. "$HOME"
printf '%s\n' ./*
find . -maxdepth 1 -type f -print
```

Usa comillas cuando una ruta pueda contener espacios o caracteres especiales:

```bash
archivo='informe de enero.txt'
printf '%s\n' "$archivo"
```

## Variables y código de salida

Las variables de shell no son iguales a variables de entorno hasta exportarlas.
`$?` contiene el código de salida de la última orden; cero suele indicar éxito.

```bash
nombre='Linux'
export EDITOR=vim
printf 'Hola, %s\n' "$nombre"
true
printf 'exit status: %s\n' "$?"
```

No uses `echo` para datos que necesiten formato exacto; `printf` permite
controlar saltos de línea y caracteres especiales.

## Pipes y redirecciones

```bash
command > salida.txt
command >> salida.txt
command 2> errores.txt
command > salida-y-errores.txt 2>&1
command-a | command-b
```

`>` reemplaza el archivo, `>>` agrega contenido y `2>` redirige el error
estándar. Un pipe conecta la salida estándar de un proceso con la entrada del
siguiente. Verifica la salida antes de encadenar una orden que modifique datos.

Ejemplos de consulta:

```bash
ps aux | less
systemctl list-units --type=service --state=running | less
find /var/log -type f -print 2>/dev/null | sort | head -n 20
```

## Comandos de consulta y transformación

| Herramienta | Uso |
| --- | --- |
| `less` | Leer salida larga sin cargarla toda en un editor. |
| `head` / `tail` | Ver inicio o final de un resultado. |
| `grep` | Filtrar líneas según un patrón. |
| `sort` / `uniq` | Ordenar y eliminar duplicados consecutivos. |
| `cut` / `awk` | Extraer campos estructurados. |
| `sed` | Transformar texto por reglas. |
| `wc` | Contar líneas, palabras o bytes. |
| `xargs` | Construir órdenes a partir de entrada; usar con cuidado. |

Para nombres de archivo arbitrarios, prefiere `find -print0` junto con una
herramienta que entienda separadores nulos. No uses `xargs` sin `-0` cuando los
nombres puedan contener espacios o saltos de línea.

## Historial y edición segura

```bash
history | tail -n 20
fc -l -10
type -a ls
command -v systemctl
```

`type -a` y `command -v` ayudan a detectar alias, funciones o rutas inesperadas.
Antes de repetir una orden privilegiada desde el historial, léela completa y
confirma que los argumentos siguen siendo correctos.

## Fuentes

- [GNU Coreutils manual](https://www.gnu.org/software/coreutils/manual/)
- [Ubuntu welcome to the terminal](https://ubuntu.com/server/docs/tutorial/welcome-to-the-terminal/)
