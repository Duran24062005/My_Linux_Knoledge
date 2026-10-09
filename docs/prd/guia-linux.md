# PRD: Guía jerárquica de Linux enfocada en Ubuntu

## Problema

El repositorio contiene una explicación aislada sobre Swap, pero no ofrece una
ruta para aprender Linux ni un sistema consistente para registrar comandos y
procedimientos operativos. Las instrucciones pueden perder contexto si no
explican privilegios, compatibilidad, verificación y rollback.

## Objetivo

Construir una base de conocimiento en español que permita:

- aprender Linux desde fundamentos hasta administración cotidiana;
- encontrar rápidamente un comando o una acción completa;
- ejecutar cambios con inspección previa y evidencia verificable;
- distinguir lo común a Linux de lo específico de Ubuntu, RHEL/Fedora u otra
  distribución;
- incorporar nuevos temas sin convertir el README en un documento monolítico.

## Alcance de la primera versión

Incluye fundamentos, terminal, archivos, permisos, usuarios, procesos,
recursos, servicios, logs, paquetes, red, almacenamiento, Swap, seguridad,
diagnóstico y Bash. También incluye diez acciones operativas y una matriz de
compatibilidad inicial.

Quedan fuera de esta versión los tutoriales exhaustivos de Kubernetes,
virtualización, administración avanzada de bases de datos, SELinux avanzado,
LVM avanzado y redes empresariales. Podrán añadirse como módulos separados.

## Usuarios y casos de uso

- **Aprendiz:** sigue la ruta en orden y consulta el glosario.
- **Usuario de Ubuntu Desktop:** resuelve tareas locales sin asumir que todos
  los servicios de servidor están instalados.
- **Operador de Ubuntu Server:** utiliza las acciones de paquetes, servicios,
  red, almacenamiento y diagnóstico.
- **Colaborador futuro:** agrega una acción o un módulo usando las plantillas y
  la política de fuentes existentes.

## Estructura documental

- `README.md`: entrada, reglas de seguridad y navegación.
- `docs/`: conceptos, ruta, tabla de comandos, glosario y diagnóstico.
- `actions/`: procedimientos orientados a un objetivo.
- `references/`: fuentes, alcance y diferencias de herramientas.
- `docs/prd/`: decisiones permanentes sobre la guía.

Cada acción seguirá: objetivo, cuándo usarla, requisitos, inspección,
precauciones, ejecución, verificación, rollback, errores y fuentes.

## Reglas técnicas y editoriales

1. Ubuntu LTS es la referencia principal; se marcarán diferencias entre
   Desktop y Server y no se fijará una versión cuando el comportamiento sea
   común entre LTS.
2. Las órdenes comunes deben indicar si dependen de GNU, Bash, systemd,
   NetworkManager u otra herramienta concreta.
3. Los comandos que modifiquen el sistema deben incluir una precondición, un
   resultado esperado y un método de comprobación.
4. Las operaciones destructivas deben advertir el riesgo y describir una
   alternativa o respaldo cuando sea razonable.
5. No se documentarán secretos reales ni rutas dependientes de una máquina
   personal sin explicarlas.
6. Las referencias se colocarán cerca del tema y también se registrarán en
   `references/sources.md`.

## Impacto en datos e integraciones

No se modifican configuraciones del sistema operativo ni servicios externos.
El impacto está limitado a archivos Markdown del repositorio. Las acciones
describen comandos que el lector puede ejecutar posteriormente en su propio
equipo.

## Riesgos y mitigaciones

| Riesgo | Mitigación |
| --- | --- |
| Copiar una orden destructiva sobre el disco incorrecto | Inspección previa, variables explícitas y advertencia visible. |
| Aplicar una instrucción de otra distribución | Etiquetas de compatibilidad y matriz de equivalencias. |
| Perder conectividad al editar red | Usar `netplan try`, mantener sesión alternativa y documentar rollback. |
| Duplicar configuración de Swap o `fstab` | Buscar entradas existentes antes de escribir y verificar unidades activas. |
| Confundir servicio detenido con servicio fallido | Consultar `systemctl status`, código de salida y `journalctl`. |
| Quedarse con una guía obsoleta | Registrar fuentes oficiales y notas de versión. |

## Evolución futura

Las siguientes ampliaciones pueden añadirse sin cambiar la estructura:

- perfiles de Debian, Fedora, RHEL, Arch y openSUSE;
- LVM, RAID, cifrado y recuperación;
- SSH y hardening de servidores;
- contenedores y virtualización;
- pruebas automatizadas de enlaces y consistencia documental;
- ejercicios prácticos con entornos virtuales o contenedores.
