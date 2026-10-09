# Acción: crear usuario y conceder acceso sudo

## Objetivo

Crear una cuenta local para una persona y añadirla al grupo administrativo de
Ubuntu de forma explícita.

## Cuándo utilizarla

Cuando una persona necesita una cuenta propia. No compartas la cuenta root ni
la cuenta de otro usuario.

## Compatibilidad, privilegios y riesgo

- **Compatibilidad:** Ubuntu/Debian con `adduser`; RHEL/Fedora usa comandos y
  grupos administrativos distintos.
- **Privilegio:** root mediante `sudo`.
- **Riesgo:** cambio persistente de identidad y privilegios.

## Inspección previa

```bash
id
getent passwd <usuario>
getent group sudo
getent group wheel
```

Elige un nombre válido y confirma que no existe una cuenta con el mismo
propósito. Verifica si el sistema usa `sudo` o el grupo `wheel`.

## Respaldo y precauciones

Registra quién autorizó el acceso y qué nivel necesita. Para una cuenta
administrativa, acuerda cómo se revocará el acceso cuando ya no sea necesario.
No guardes contraseñas en la guía ni en el historial.

## Ejecución

```bash
sudo adduser <usuario>
sudo usermod -aG sudo <usuario>
id <usuario>
sudo -l -U <usuario>
```

La persona debe cerrar y abrir sesión para recibir el grupo suplementario. El
comando `adduser` solicitará datos interactivos; decide si los datos de nombre
completo, teléfono y otros campos son realmente necesarios.

## Verificación

Desde una nueva sesión del usuario:

```bash
id
sudo -v
sudo id
```

Debe aparecer el grupo `sudo` y `sudo id` debe mostrar UID 0 después de una
autenticación correcta.

## Rollback

Para retirar solo el privilegio administrativo:

```bash
sudo gpasswd -d <usuario> sudo
```

Comprueba la sesión nueva del usuario. Para eliminar una cuenta, detén el
procedimiento y respalda el home; `userdel -r` elimina datos locales y no debe
ser un rollback automático.

## Errores frecuentes

- El usuario no ve `sudo`: debe abrir una nueva sesión.
- `sudo` no existe: confirma la distribución y su mecanismo administrativo.
- `usermod` reemplazó grupos: revisa que se haya usado `-aG`.
- La cuenta puede usar sudo, pero una política adicional limita comandos:
  revisa `sudo -l -U` y `/etc/sudoers.d/` con `visudo`.

## Fuentes

- [Ubuntu user management](https://ubuntu.com/server/docs/how-to/security/user-management/)
- [Ubuntu welcome to the terminal](https://ubuntu.com/server/docs/tutorial/welcome-to-the-terminal/)
