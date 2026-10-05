# ¿Cuales son las diferencias entre Memory y Swap?

La **Memory (Memoria RAM)** es el componente de hardware físico de acceso ultrarrápido donde el procesador almacena los datos y las instrucciones de las aplicaciones que se están ejecutando activamente. En la captura proporcionada, esta memoria física corresponde a dos módulos DDR4 en formato SODIMM funcionando a 3200 MT/s. Su lectura y escritura son inmediatas, pero su capacidad es limitada y su contenido se borra al apagar el equipo.

El **Swap (Espacio de Intercambio)** es una porción del disco de almacenamiento (ya sea un archivo o una partición en tu SSD/HDD) que el sistema operativo utiliza como una extensión virtual de la RAM. Cuando la RAM física empieza a llenarse, el kernel de Linux traslada los datos de las aplicaciones en segundo plano o menos activas hacia el Swap para liberar espacio rápido para los procesos prioritarios. Debido a que el Swap reside en el disco, su velocidad es drásticamente inferior a la de la RAM.

**Análisis del estado actual en `image_14989a.png`:**

* **Saturación del sistema:** La gráfica muestra un pico repentino donde el Swap alcanzó rápidamente su límite máximo, quedando al 100% de uso (4.29 GB ocupados de 4.29 GB disponibles).


* **Alta demanda física:** Simultáneamente, la memoria RAM se encuentra operando al 84% de su capacidad (13.80 GB en uso de un total de 16.52 GB).


* **Impacto en el rendimiento:** Tener el Swap al 100% indica que el sistema se ha quedado sin "válvula de escape" para la memoria virtual. En un entorno de desarrollo Linux, levantar múltiples contenedores Docker, bases de datos o procesos de compilación en este estado generará ralentizaciones severas (thrashing), ya que el sistema pierde ciclos de CPU moviendo datos entre el disco y la RAM de forma constante. Si el consumo de memoria de los procesos continúa aumentando, el *OOM Killer* (Out of Memory Killer) del sistema operativo intervendrá para cerrar forzosamente las aplicaciones más pesadas y evitar un colapso total del sistema.

---

## ¿Cómo puedo asignarle mas espacio a la memoria swap de mi laptop?

La forma más segura y rápida de agregar espacio Swap en Linux sin arriesgarte a modificar particiones de disco es creando un archivo de intercambio adicional (swapfile) y sumándolo al que ya tienes.

Sigue estos pasos en tu terminal para añadir 8 GB extra de Swap:

1. **Crear el archivo de intercambio:**
Asignaremos un bloque de 8 GB en la raíz del sistema.
```bash
sudo fallocate -l 8G /swapfile_extra

```


*Verificación: Ejecuta `ls -lh /swapfile_extra` para confirmar que el archivo se creó con un peso de 8.0G.*
2. **Ajustar los permisos de seguridad:**
Solo el sistema (root) debe tener acceso de lectura y escritura a este archivo para evitar vulnerabilidades.
```bash
sudo chmod 600 /swapfile_extra

```


*Verificación: Ejecuta `ls -l /swapfile_extra` y asegúrate de que los permisos al inicio de la línea sean exactamente `-rw-------`.*
3. **Formatear el archivo como espacio Swap:**
Prepara el archivo para que el kernel lo reconozca como memoria virtual.
```bash
sudo mkswap /swapfile_extra

```


*Verificación: La terminal imprimirá un mensaje confirmando que se configuró el swapspace y te mostrará un UUID generado.*
4. **Activar el nuevo Swap:**
Enciende el archivo para que el sistema comience a utilizarlo de inmediato.
```bash
sudo swapon /swapfile_extra

```


5. **Confirmar la capacidad total:**
Revisa que el sistema ahora sume los 4.29 GB originales más los nuevos 8 GB.
```bash
free -h

```


*Verificación: La fila "Swap:" debería mostrar un total cercano a los 12 GB. También puedes ejecutar `swapon --show` para ver ambos orígenes trabajando en paralelo.*
6. **Hacer el cambio permanente (Recomendado):**
Si reinicias tu equipo ahora, este swap adicional desaparecerá. Para que se monte automáticamente en cada inicio, debes registrarlo en la tabla del sistema de archivos.
```bash
echo '/swapfile_extra none swap sw 0 0' | sudo tee -a /etc/fstab

```


*Verificación: Ejecuta `cat /etc/fstab` y revisa que la línea `/swapfile_extra none swap sw 0 0` aparezca al final del archivo.*