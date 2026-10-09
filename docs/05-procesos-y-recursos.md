# Procesos, señales y recursos

## Qué es un proceso

Un proceso es una instancia de un programa en ejecución con PID, propietario,
estado, memoria, descriptores y relación con un proceso padre. Un servicio
puede supervisar uno o varios procesos, pero proceso y servicio no son
sinónimos.

```bash
ps -eo pid,ppid,user,stat,%cpu,%mem,etime,comm,args --sort=-%cpu | head -n 20
pgrep -a <nombre>
pstree -ap
```

Estados habituales:

- `R`: ejecutándose o listo para CPU.
- `S`: dormido interrumpible.
- `D`: espera no interrumpible, a menudo por I/O.
- `T`: detenido.
- `Z`: zombie; terminó, pero su padre aún no recogió el estado.

## CPU, memoria y Swap

```bash
uptime
free -h
vmstat 1 5
top
```

La memoria usada por el kernel para caché no implica automáticamente un
problema: parte puede recuperarse. Observa presión, Swap activa, I/O y tendencia
antes de matar procesos.

```bash
swapon --show
cat /proc/swaps
cat /proc/meminfo | grep -E 'MemAvailable|SwapTotal|SwapFree'
```

Swap es almacenamiento lento que puede evitar un fallo inmediato, pero no
convierte el disco en RAM. Uso constante de Swap junto con espera alta puede
indicar presión de memoria o thrashing.

## Señales y finalización

```bash
kill -TERM <pid>
kill -KILL <pid>
pkill -TERM -u <usuario> <nombre>
```

La práctica normal es `SIGTERM`: permite que el proceso cierre recursos. Usa
`SIGKILL` solo cuando el proceso no responde y hayas evaluado pérdida de datos.
No mates un PID sin volver a comprobar que pertenece al proceso esperado.

## Archivos abiertos y puertos

```bash
lsof -p <pid>
lsof +L1
ss -tulpn
fuser -v <puerto>/tcp
```

`lsof` puede no estar instalado y puede requerir permisos para mostrar procesos
de otros usuarios. `ss` muestra sockets y estado de escucha; una aplicación que
escucha en `127.0.0.1` no está expuesta igual que una que escucha en todas las
interfaces.

## Cgroups y límites

systemd agrupa servicios en cgroups y puede aplicar límites de CPU, memoria,
procesos o I/O. Para investigar un servicio, relaciona el PID con su unidad:

```bash
systemctl status <servicio>
systemctl show <servicio> -p MainPID -p MemoryCurrent -p CPUUsageNSec
systemd-cgls
```

No aumentes límites como primera respuesta. Primero confirma cuál recurso está
agotado y qué componente lo consume.

## Fuentes

- [GNU process control](https://www.gnu.org/software/coreutils/manual/html_node/Process-control.html)
- [Ubuntu performance documentation](https://ubuntu.com/server/docs/explanation/performance/)
- [Linux Kernel Administrator’s Guide](https://www.kernel.org/doc/html/latest/admin-guide/index.html)
