
# Práctica de Configuración de DNS Maestro-Esclavo con Subdominios
## Descripción General

Esta práctica configura un entorno de DNS con un servidor maestro (`DNSA`) y un servidor esclavo (`DNSB`) utilizando `bind9` en máquinas virtuales gestionadas con Vagrant. La configuración incluye el dominio principal `ies.test` y varios subdominios (`informatica`, `aulas`, `departamentos`). También se ha configurado una zona inversa para la red `192.168.57.0/24`.

### Estructura del Proyecto

- `Vagrantfile`: Define la configuración de las máquinas virtuales `DNSA` y `DNSB`.
- `config_dns/`: Carpeta que contiene los archivos de configuración de zonas DNS.
  - `named.conf.local.dnsa`: Archivo de configuración de Bind para `DNSA` (maestro).
  - `named.conf.local.dnsb`: Archivo de configuración de Bind para `DNSB` (esclavo).
  - Archivos de zonas (`db.ies.test`, `db.informatica.ies.test`, `db.aulas.ies.test`, `db.departamentos.ies.test`, `db.192.168.57`).

### Configuración Realizada

1. **Dominio Principal y Subdominios**:
   - `DNSA` actúa como servidor maestro para el dominio `ies.test` y los subdominios `informatica`, `aulas` y `departamentos`.
   - Se han configurado los archivos de zona en `DNSA` para resolver las direcciones IP de los equipos especificados.

2. **Servidor DNS Esclavo**:
   - `DNSB` se ha configurado como un servidor esclavo para los dominios `informatica.ies.test`, `aulas.ies.test`, y `57.168.192.in-addr.arpa`.
   - `DNSB` recibe la información de estas zonas desde `DNSA` mediante transferencia de zona.

3. **Zona Inversa**:
   - Se ha configurado una zona inversa para la red `192.168.57.0/24`, permitiendo la resolución inversa (IP a nombre de host) para los equipos en la red.

4. **Transferencia de Zona**:
   - `DNSA` transfiere las zonas configuradas a `DNSB` para que `DNSB` pueda resolver las consultas de los subdominios y la zona inversa.

### Archivos Configurados

- **`named.conf.local.dnsa`**: Configura las zonas maestras en `DNSA` para los dominios y la zona inversa.
- **`named.conf.local.dnsb`**: Configura las zonas esclavas en `DNSB` apuntando al servidor maestro `DNSA`.
- **Archivos de Zona (`db.ies.test`, `db.informatica.ies.test`, etc.)**: Contienen los registros A y PTR necesarios para la resolución de nombres directos e inversos.

### Ejecución del Proyecto

Para ejecutar el proyecto:

1. Clona el repositorio y navega a la carpeta del proyecto.
2. Ejecuta `vagrant up` para iniciar y configurar las máquinas `DNSA` y `DNSB`.
3. Las configuraciones de `bind9` se aplican automáticamente en cada máquina.

### Comprobaciones

- Prueba la resolución de nombres y la resolución inversa en ambas máquinas (`DNSA` y `DNSB`) usando `dig` y `nslookup`.
- Verifica la transferencia de zona revisando que `DNSB` tiene archivos de zona en `/var/cache/bind`.

