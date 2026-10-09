# Ruta de aprendizaje

Esta ruta prioriza comprender el sistema antes de automatizar cambios. Puedes
leerla completa o saltar a una acción concreta cuando ya conozcas el concepto.

## Nivel 1: orientarse

1. Lee [fundamentos](01-fundamentos.md).
2. Practica [terminal y shell](02-terminal-y-shell.md).
3. Consulta la [tabla de comandos](tabla-de-comandos.md) mientras trabajas.

Objetivo: saber en qué distribución, kernel, usuario, shell y ruta estás
trabajando.

## Nivel 2: administrar datos y acceso

1. Estudia [archivos y permisos](03-archivos-y-permisos.md).
2. Continúa con [usuarios y grupos](04-usuarios-y-grupos.md).
3. Ejecuta la acción de [corregir permisos](../actions/permissions/corregir-permisos.md)
   solo en una ruta de prueba.

Objetivo: poder explicar quién puede leer, modificar o ejecutar cada recurso.

## Nivel 3: entender lo que está ejecutándose

1. Lee [procesos y recursos](05-procesos-y-recursos.md).
2. Estudia [servicios y logs](06-servicios-y-logs.md).
3. Practica [inspeccionar procesos](../actions/processes/inspeccionar-finalizar-proceso.md)
   y [gestionar un servicio](../actions/services/gestionar-servicio-logs.md).

Objetivo: diferenciar proceso, servicio, unidad, sesión, PID y log.

## Nivel 4: instalar y conectar

1. Estudia [paquetes y repositorios](07-paquetes-y-repositorios.md).
2. Lee [redes y DNS](08-redes-y-dns.md).
3. Consulta la [matriz de compatibilidad](distros/compatibilidad.md).

Objetivo: instalar software desde una fuente confiable y diagnosticar una red
sin cambiar varias capas al mismo tiempo.

## Nivel 5: operar con seguridad

1. Lee [almacenamiento y Swap](09-almacenamiento-y-swap.md).
2. Revisa [seguridad y firewall](10-seguridad-y-firewall.md).
3. Usa [diagnóstico](11-diagnostico.md) como procedimiento transversal.
4. Automatiza únicamente después de entender [Bash](12-automatizacion-bash.md).

Objetivo: hacer cambios persistentes con respaldo, verificación y rollback.

## Cómo estudiar cada tema

Para cada comando, responde:

- ¿Qué recurso consulta o modifica?
- ¿Con qué usuario se ejecuta?
- ¿Qué salida confirma el estado esperado?
- ¿Qué puede salir mal?
- ¿Cómo se deshace el cambio?
- ¿Depende de Ubuntu, de una versión o de una herramienta opcional?
