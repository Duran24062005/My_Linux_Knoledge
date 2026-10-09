# Automatización con Bash

## Cuándo automatizar

Automatiza una tarea cuando el procedimiento manual ya está entendido, sus
entradas están definidas y puedes comprobar el resultado. Un script que oculta
la lógica o no maneja errores puede ser más peligroso que una ejecución manual.

## Plantilla segura

```bash
#!/usr/bin/env bash
set -Eeuo pipefail

usage() {
    printf 'Uso: %s <directorio>\n' "$0" >&2
}

if [[ $# -ne 1 ]]; then
    usage
    exit 64
fi

directory=$1

if [[ ! -d $directory ]]; then
    printf 'No existe el directorio: %s\n' "$directory" >&2
    exit 66
fi

find "$directory" -maxdepth 1 -type f -print
```

`set -e` detiene fallos no gestionados, `-u` detecta variables no definidas y
`pipefail` conserva un fallo dentro de un pipe. `-E` permite que los traps
heredados conserven contexto. Aun así, no reemplazan la validación explícita.

## Variables y quoting

Siempre cita variables que representen rutas o texto arbitrario:

```bash
printf 'Ruta: %s\n' "$directory"
if [[ -f $file ]]; then
    printf 'Archivo regular: %s\n' "$file"
fi
```

Usa `[[ condición ]]` para condiciones Bash y arrays cuando una lista pueda contener
espacios:

```bash
files=(/var/log/*.log)
for file in "${files[@]}"; do
    printf '%s\n' "$file"
done
```

## Idempotencia

Una acción idempotente produce el mismo estado si se ejecuta una segunda vez.
Antes de añadir una línea a una configuración, busca si ya existe. Antes de
crear un usuario, comprueba si existe. Antes de habilitar un servicio, consulta
su estado.

```bash
if ! getent group <grupo> >/dev/null; then
    sudo groupadd <grupo>
fi
```

## Logs y códigos de salida

```bash
log() {
    printf '[%s] %s\n' "$(date --iso-8601=seconds)" "$*"
}

if ! command -v systemctl >/dev/null 2>&1; then
    printf 'systemctl no está disponible\n' >&2
    exit 69
fi
```

Define códigos de salida útiles, escribe errores en stderr y no incluyas
secretos en logs. Para tareas destructivas, añade modo de simulación o una
confirmación explícita.

## Bash no es universal

Un script con `[[`, arrays o `mapfile` necesita Bash. Si el script debe correr
en `/bin/sh`, escribe sintaxis POSIX y prueba en el shell objetivo. Declara el
intérprete con shebang y valida:

```bash
bash -n script.sh
shellcheck script.sh
```

`shellcheck` es una herramienta externa y debe instalarse según la distribución.

## Fuentes

- [GNU Bash Reference Manual](https://www.gnu.org/software/bash/manual/bash.html)
- [GNU Coreutils manual](https://www.gnu.org/software/coreutils/manual/)
- [Ubuntu command line documentation](https://ubuntu.com/server/docs/tutorial/)
