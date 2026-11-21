# 🛡️ Configuración Inicial, Hardening y Acceso Remoto Seguro (SSH) en Switch Cisco

![Cisco Packet Tracer](https://shields.io)
![CCNA](https://shields.io)
![Security](https://shields.io)

## 📋 Descripción del Proyecto
Este laboratorio práctico forma parte de mi ruta de estudio autodidacta hacia la certificación **CCNA**. El objetivo principal es simular el aprovisionamiento inicial de un switch empresarial **Cisco Catalyst 2960**, aplicando medidas de seguridad básica (*hardening*), segmentación de la interfaz de administración y acceso remoto cifrado mediante **SSHv2**.

---

## 🗺️ Topología de Red
La infraestructura consta de un switch de capa 2 (`SW1`), una estación de administración inicial conectada por puerto de consola (`CONSOLE`) y un equipo cliente (`USER-VLAN5`) conectado al puerto `FastEthernet0/1` para validar la gestión remota a través de la red.

![Topología del Laboratorio](docs/images/01-topologia.png)

### 📊 Tabla de Direccionamiento

| Dispositivo | Interfaz | Dirección IP | Máscara de Subred | VLAN | Función |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **SW1** | `Vlan5` (SVI) | `172.16.50.1` | `255.255.255.0` | VLAN 5 | Interfaz virtual de administración |
| **SW1** | `Vlan1` (SVI) | *Sin asignar* | *N/A* | VLAN 1 | Deshabilitada por seguridad (`shutdown`) |
| **USER-VLAN5** | `FastEthernet0` | `172.16.50.10` | `255.255.255.0` | VLAN 5 | Host cliente para pruebas SSH |

---

## 🛠️ Implementaciones Técnicas Realizadas

1. **Seguridad de Acceso Físico y Privilegiado:**
   * Asignación de identidad del dispositivo (`hostname SW1`) y bloqueo de búsquedas DNS erróneas (`no ip domain-lookup`).
   * Protección del modo privilegiado mediante hash MD5 (`enable secret`) y cifrado de contraseñas en texto plano (`service password-encryption`).
   * Configuración de contraseña y sincronización de mensajes en consola (`line con 0`).
   * Aviso legal de acceso no autorizado (`banner motd`).

2. **Segmentación de Administración:**
   * Creación de la **VLAN 5** dedicada exclusivamente a tareas de gestión.
   * Desactivación administrativa de la VLAN 1 por defecto (`interface vlan 1` -> `shutdown`).
   * Configuración del puerto `FastEthernet0/1` en modo acceso asignado a la VLAN 5.

3. **Administración Remota Segura (SSHv2):**
   * Definición del nombre de dominio local (`://lab.com`) y generación de par de llaves criptográficas **RSA de 2048 bits**.
   * Habilitación exclusiva de **SSH versión 2** (`ip ssh version 2`), bloqueando protocolos inseguros como Telnet (`transport input ssh`) y exigiendo autenticación contra la base de datos local (`login local`).

---

## ✅ Validación y Evidencias

### 1. Prueba de Conexión Remota SSH y Estado de Interfaces
Desde el host cliente `USER-VLAN5`, se estableció sesión remota exitosa hacia `172.16.50.1` verificando el despliegue del banner legal y el estado `up/up` de la interfaz `Vlan5` mediante `show ip interface brief`.

![Validación SSH](docs/images/02-validacion-ssh.png)

### 2. Verificación de Configuración Activa (`show running-config`)
Revisión de contraseñas cifradas, parámetros SSHv2, asignación de puertos de acceso y restricciones en líneas virtuales (`vty 0 15`).

![Running Config Parte 1](docs/images/03-running-config-1.png)
![Running Config Parte 2](docs/images/04-running-config-2.png)

---

## 📂 Contenido del Repositorio
* [`configs/sw1-running-config.txt`](configs/sw1-running-config.txt): Archivo de texto con la configuración completa aplicada en `SW1`.
* [`topology/switch-ssh-lab.pkt`](topology/switch-ssh-lab.pkt): Archivo fuente editable para abrir y probar en **Cisco Packet Tracer**.
* [`docs/images/`](docs/images/): Capturas de pantalla con las evidencias de configuración y pruebas.
