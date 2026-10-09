# Acción: configurar una dirección estática con Netplan

## Objetivo

Configurar una dirección IPv4 estática en Ubuntu mediante Netplan, con prueba
temporal y posibilidad de rollback.

## Cuándo utilizarla

En equipos donde una interfaz necesita una dirección fija, gateway y DNS
conocidos. No la uses sin autorización de la red.

## Compatibilidad, privilegios y riesgo

- **Compatibilidad:** Ubuntu con Netplan; renderer `networkd` o
  NetworkManager.
- **Privilegio:** root mediante `sudo`.
- **Riesgo:** puede cortar la conectividad, especialmente por SSH.

## Inspección previa

```bash
ip -br link
ip -br address
ip route
sudo netplan get
systemctl is-active systemd-networkd NetworkManager
```

Confirma nombre de interfaz, prefijo, gateway, DNS, renderer y que la
dirección no esté siendo usada por otro equipo. Mantén consola local o una
segunda sesión antes de continuar.

## Respaldo y precauciones

```bash
sudo cp -a /etc/netplan "/etc/netplan.bak.$(date +%Y%m%d-%H%M%S)"
ls -l /etc/netplan
```

Crea una configuración con nombre posterior a las existentes para entender su
precedencia. El YAML exige espacios, no tabuladores.

## Ejecución

Edita un archivo nuevo y adapta todos los valores:

```bash
sudoedit /etc/netplan/99-static.yaml
```

Contenido de ejemplo para `systemd-networkd`:

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    <interfaz>:
      addresses:
        - 192.0.2.10/24
      routes:
        - to: default
          via: 192.0.2.1
      nameservers:
        addresses:
          - 1.1.1.1
          - 9.9.9.9
```

Genera y prueba. Si estás remoto, no confirmes hasta comprobar la sesión:

```bash
sudo netplan generate
sudo netplan try
sudo netplan status
ip -br address
ip route
```

## Verificación

```bash
ping -c 3 192.0.2.1
getent hosts example.com
ip route get 1.1.1.1
```

Comprueba también la aplicación que necesitaba la dirección y el estado del
renderer seleccionado.

## Rollback

Si `netplan try` no funciona, no confirmes y espera la reversión. Si ya
confirmaste, restaura la copia del directorio que tomaste, revisa que solo
contenga archivos esperados y ejecuta de nuevo `netplan try` antes de `apply`.

```bash
sudo mv /etc/netplan/99-static.yaml /etc/netplan/99-static.yaml.disabled
sudo netplan generate
sudo netplan try
```

No borres todas las configuraciones de Netplan como rollback genérico.

## Errores frecuentes

- Interfaz incorrecta: nombres predecibles no son siempre `eth0`.
- YAML inválido: usa espacios y `netplan generate` antes de aplicar.
- Gateway fuera de la subred: confirma el diseño de red.
- DNS funciona, pero no hay conectividad: revisa rutas y firewall.
- Se perdió SSH: usa consola o la sesión alternativa y revierte el archivo.

## Fuentes

- [Ubuntu configuring networks](https://ubuntu.com/server/docs/explanation/networking/configuring-networks/)
- [Netplan CLI manpage](https://manpages.ubuntu.com/manpages/noble/man8/netplan.8.html)
