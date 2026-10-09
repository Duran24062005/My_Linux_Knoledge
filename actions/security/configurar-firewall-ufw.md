# Acción: configurar firewall básico con UFW

## Objetivo

Aplicar una política mínima de firewall en Ubuntu y permitir únicamente los
servicios necesarios.

## Cuándo utilizarla

En Ubuntu donde UFW está instalado y no existe otra herramienta administrando
las mismas reglas. No la uses en RHEL/Fedora como sustituto de firewalld.

## Compatibilidad, privilegios y riesgo

- **Compatibilidad:** Ubuntu/Debian con UFW.
- **Privilegio:** root mediante `sudo`.
- **Riesgo:** puede bloquear conexiones legítimas o dejar una administración
  remota inaccesible.

## Inspección previa

```bash
sudo ufw status verbose
sudo ufw app list
sudo ss -lntup
```

Si estás conectado por SSH, identifica el puerto real y el perfil de acceso.
No actives el firewall hasta haber permitido esa administración.

## Respaldo y precauciones

Registra la política actual:

```bash
sudo ufw status numbered | tee ufw-antes.txt
```

No publiques el archivo si incluye direcciones internas. Define qué tráfico
entrante debe existir y qué servicios están expuestos intencionalmente.

## Ejecución

Aplica una política conservadora y permite primero la administración:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow OpenSSH
sudo ufw enable
```

Si el servicio usa otro puerto, sustituye `OpenSSH` por una regla específica
solo después de confirmarlo:

```bash
sudo ufw allow <puerto>/tcp
sudo ufw allow from <red-autorizada> to any port <puerto> proto tcp
```

## Verificación

```bash
sudo ufw status verbose
sudo ufw status numbered
sudo ss -lntup
```

Prueba desde un origen autorizado. Un estado `allow` no prueba que haya un
proceso escuchando ni que el servicio acepte autenticación.

## Rollback

Elimina una regla por número después de volver a listar las reglas:

```bash
sudo ufw status numbered
sudo ufw delete <numero>
```

Para desactivar temporalmente toda la política, solo si tienes acceso
alternativo y una razón clara:

```bash
sudo ufw disable
```

Documenta y restaura las reglas necesarias después de resolver la incidencia.

## Errores frecuentes

- SSH bloqueado: usa consola local o proveedor fuera de banda.
- Regla duplicada: revisa `status numbered` antes de borrar.
- UFW no controla el firewall: identifica nftables, firewalld u otra capa.
- Puerto cerrado: confirma servicio, dirección de escucha, ruta y firewall
  remoto.

## Fuentes

- [Ubuntu firewall documentation](https://ubuntu.com/server/docs/how-to/security/firewalls/)
- [Ubuntu UFW documentation](https://help.ubuntu.com/community/UFW)
