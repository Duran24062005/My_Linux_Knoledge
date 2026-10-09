# Acción: inspeccionar y finalizar un proceso

## Objetivo

Identificar un proceso que consume recursos o no responde y terminarlo con la
señal menos agresiva posible.

## Cuándo utilizarla

Cuando una aplicación bloquea una sesión, consume CPU o memoria de forma
anómala, o mantiene abierto un recurso que necesitas liberar.

## Compatibilidad, privilegios y riesgo

- **Compatibilidad:** Linux con procps; `lsof` es opcional.
- **Privilegio:** consulta como usuario normal; otros usuarios pueden requerir
  `sudo`.
- **Riesgo:** enviar señales puede perder datos; `SIGKILL` es irreversible para
  ese proceso.

## Inspección previa

```bash
ps -eo pid,ppid,user,stat,%cpu,%mem,etime,comm,args --sort=-%cpu | head -n 25
pgrep -a <nombre>
free -h
swapon --show
```

Captura PID, usuario, comando completo, padre, estado y tiempo de ejecución.
Confirma el PID justo antes de actuar; los PIDs se reutilizan.

## Respaldo y precauciones

Si es una aplicación con datos sin guardar, intenta su cierre normal desde la
propia aplicación. Si es un servicio administrado por systemd, utiliza la
acción de [gestionar servicios](../services/gestionar-servicio-logs.md) para
que el supervisor conozca el estado.

## Ejecución

Primero envía `SIGTERM`:

```bash
ps -p <pid> -o pid,ppid,user,stat,etime,comm,args
kill -TERM <pid>
sleep 2
ps -p <pid> -o pid,ppid,user,stat,etime,comm,args
```

Si el proceso pertenece a una cuenta propia y hay varios procesos del mismo
programa, usa `pkill` solo después de revisar la lista:

```bash
pgrep -a -u "$USER" <nombre>
pkill -TERM -u "$USER" -x <nombre>
```

Usa `SIGKILL` únicamente si el proceso sigue bloqueado y aceptas perder su
limpieza normal:

```bash
kill -KILL <pid>
```

## Verificación

```bash
ps -p <pid> -o pid,stat,comm,args
pgrep -a <nombre>
ss -lntup
lsof -p <pid> 2>/dev/null
```

Si era parte de un servicio, comprueba si systemd lo reinició y revisa logs.

## Rollback

Una señal de terminación no puede deshacerse. El rollback consiste en reabrir la
aplicación o iniciar el servicio después de verificar su configuración y datos.
No reinicies automáticamente un proceso que está dañando datos.

## Errores frecuentes

- `No such process`: el proceso terminó o el PID cambió.
- `Operation not permitted`: no eres propietario o el proceso está protegido.
- Estado `D`: puede estar bloqueado esperando I/O; matar no siempre libera la
  causa.
- Estado `Z`: el zombie requiere que el padre recoja su estado.

## Fuentes

- [GNU process control](https://www.gnu.org/software/coreutils/manual/html_node/Process-control.html)
- [procps man pages](https://manpages.ubuntu.com/manpages/noble/en/man1/ps.1.html)
