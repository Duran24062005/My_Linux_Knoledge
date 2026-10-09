# Redes, interfaces, rutas y DNS

## Modelo de diagnóstico

Una conexión útil depende de varias capas:

1. interfaz y enlace físico o virtual;
2. dirección IP y máscara;
3. ruta por defecto;
4. resolución DNS;
5. conectividad al destino;
6. servicio remoto, puerto y autenticación.

No concluyas que "Internet está caído" solo porque un nombre no resuelve.

## Inspección con iproute2

```bash
ip -br link
ip -br address
ip route
ip route get 1.1.1.1
ss -tulpen
```

`ip -br` produce un resumen legible. `ip route get` muestra qué interfaz,
dirección y gateway usaría el kernel para un destino.

## Netplan en Ubuntu

Netplan usa YAML y genera configuración para `systemd-networkd` o
NetworkManager. Antes de editar:

```bash
sudo netplan get
ls -l /etc/netplan/
systemctl is-active systemd-networkd NetworkManager
```

Tras un cambio, usa `netplan try` cuando esté disponible:

```bash
sudo netplan try
sudo netplan status
```

`netplan try` aplica temporalmente y permite confirmar; si no confirmas dentro
del plazo, revierte la configuración. En una conexión remota, conserva una
sesión alternativa o acceso local antes de cambiar la única interfaz.

## NetworkManager y nmcli

```bash
nmcli general status
nmcli device status
nmcli connection show
nmcli device show <interfaz>
```

NetworkManager puede administrar conexiones gráficas y de servidor. No mezcles
edición manual de perfiles, Netplan y `nmcli` sin identificar cuál es la fuente
de verdad en ese equipo.

## Pruebas de conectividad

```bash
ping -c 4 <gateway>
ping -c 4 1.1.1.1
getent hosts example.com
resolvectl status
curl --head --max-time 10 https://example.com
```

`ping` puede estar bloqueado por el destino; su fallo no prueba por sí solo que
HTTPS no funcione. `getent` prueba la resolución usada por las aplicaciones
mediante NSS. `resolvectl` depende de systemd-resolved.

## Puertos y firewall

```bash
sudo ss -lntup
sudo ufw status verbose
```

Primero confirma que el proceso escucha y en qué dirección; luego revisa el
firewall. Abrir un puerto que no tiene servicio detrás no resuelve la causa.

## Fuentes

- [Ubuntu networking](https://ubuntu.com/server/docs/how-to/networking/)
- [Ubuntu configuring networks](https://ubuntu.com/server/docs/explanation/networking/configuring-networks/)
- [Netplan CLI manpage](https://manpages.ubuntu.com/manpages/noble/man8/netplan.8.html)
- [Red Hat systemd network targets](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_and_managing_networking/systemd-network-targets-and-services_configuring-and-managing-networking)
