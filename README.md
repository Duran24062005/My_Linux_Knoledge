# My_Linux_Knowledge

Base de conocimiento personal sobre Linux, con foco en Ubuntu LTS y con espacio
para comparar otras distribuciones. El repositorio combina explicaciones para
aprender, referencias de comandos y guías completas de acción.

## Cómo usar este repositorio

1. Sigue la [ruta de aprendizaje](docs/00-ruta-de-aprendizaje.md) si estás
   construyendo fundamentos.
2. Consulta los módulos de [conceptos](docs/) cuando necesites entender el
   sistema antes de modificarlo.
3. Usa una guía de [acciones](actions/) cuando tengas un objetivo operativo.
4. Revisa la [tabla de comandos](docs/tabla-de-comandos.md), el
   [glosario](docs/glosario.md) y la [matriz de distribuciones](docs/distros/compatibilidad.md)
   cuando no recuerdes una herramienta o exista una diferencia entre sistemas.
5. Comprueba las [fuentes](references/sources.md) antes de aplicar una
   instrucción en un equipo importante.

## Reglas de seguridad

- Lee la sección de inspección previa antes de ejecutar cambios.
- Confirma siempre el equipo, usuario, ruta, disco, interfaz o servicio sobre
  el que vas a trabajar.
- Trata como potencialmente destructivos `rm`, `dd`, `mkfs`, `fdisk`,
  `parted`, `wipefs`, cambios de red, cambios de permisos recursivos y
  operaciones sobre `/etc`, `/boot` o `/var`.
- No pegues contraseñas, tokens, claves privadas ni datos personales en
  comandos, capturas o documentación.
- Prefiere `sudo` para una orden concreta en lugar de abrir una shell root
  permanente.
- Después de cada cambio, verifica el estado y registra cómo volver atrás.
- Una orden documentada no sustituye la copia de seguridad ni la revisión de
  la documentación de la versión instalada.

## Estructura

```text
.
├── actions/       Procedimientos ejecutables con verificación y rollback
├── docs/          Conceptos, ruta de aprendizaje, comandos y troubleshooting
├── references/    Fuentes y política de referencia
└── README.md      Índice y reglas de uso
```

El diseño y los criterios de la guía están registrados en
[`docs/prd/guia-linux.md`](docs/prd/guia-linux.md).

## Ruta de aprendizaje

| Etapa | Tema | Resultado esperado |
| --- | --- | --- |
| 1 | [Fundamentos](docs/01-fundamentos.md) | Entender kernel, distribución, shell, usuarios y filesystem. |
| 2 | [Terminal y shell](docs/02-terminal-y-shell.md) | Navegar, consultar ayuda, combinar comandos y redirigir salida. |
| 3 | [Archivos y permisos](docs/03-archivos-y-permisos.md) | Administrar rutas, propietarios, permisos y enlaces. |
| 4 | [Usuarios y grupos](docs/04-usuarios-y-grupos.md) | Gestionar cuentas y privilegios con criterio. |
| 5 | [Procesos y recursos](docs/05-procesos-y-recursos.md) | Diagnosticar CPU, memoria, Swap y procesos. |
| 6 | [Servicios y logs](docs/06-servicios-y-logs.md) | Operar `systemd` y encontrar evidencia en el journal. |
| 7 | [Paquetes](docs/07-paquetes-y-repositorios.md) | Instalar, actualizar y retirar software sin romper dependencias. |
| 8 | [Redes](docs/08-redes-y-dns.md) | Inspeccionar interfaces, rutas, DNS y Netplan. |
| 9 | [Almacenamiento](docs/09-almacenamiento-y-swap.md) | Entender discos, montajes, filesystem y Swap. |
| 10 | [Seguridad](docs/10-seguridad-y-firewall.md) | Reducir exposición y aplicar cambios reversibles. |
| 11 | [Diagnóstico](docs/11-diagnostico.md) | Seguir un flujo reproducible desde el síntoma hasta la causa. |
| 12 | [Automatización](docs/12-automatizacion-bash.md) | Escribir scripts Bash claros, verificables e idempotentes. |

## Convenciones de las guías

Cada acción debe declarar:

- **Compatibilidad:** `Linux/GNU`, `Ubuntu/Debian`, `RHEL/Fedora` o una
  variante concreta.
- **Privilegio:** usuario normal, `sudo` o root.
- **Riesgo:** lectura, reversible, cambio persistente o destructivo.
- **Entrada:** valores que debes reemplazar, como `<usuario>` o `<servicio>`.
- **Verificación:** qué observar para saber si funcionó.
- **Rollback:** cómo deshacer el cambio o detenerse de forma segura.

Los bloques de comandos no incluyen el prompt `$` o `#`, para que puedan
copiarse sin arrastrar el indicador de la shell. Los valores entre `<angulares>`
son ejemplos que deben sustituirse.

## Acciones disponibles

- [Diagnóstico básico](actions/system/diagnostico-basico.md)
- [Instalar y actualizar paquetes](actions/packages/actualizar-e-instalar-paquetes.md)
- [Crear usuario y acceso sudo](actions/users/crear-usuario-sudo.md)
- [Corregir permisos y propietarios](actions/permissions/corregir-permisos.md)
- [Inspeccionar y finalizar procesos](actions/processes/inspeccionar-finalizar-proceso.md)
- [Gestionar servicios y logs](actions/services/gestionar-servicio-logs.md)
- [Inspeccionar disco y montar filesystem](actions/storage/inspeccionar-disco-y-montar.md)
- [Configurar una red estática](actions/network/configurar-red-estatica.md)
- [Configurar firewall con UFW](actions/security/configurar-firewall-ufw.md)
- [Cambiar Swap](actions/change_swap_space.md)

## Fuentes

La [política de fuentes](references/sources.md) prioriza documentación oficial,
páginas `man` y manuales de proyectos. Los foros se usan para encontrar casos
prácticos y síntomas, no como única autoridad para una operación sensible.
