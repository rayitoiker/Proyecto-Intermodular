# LuxDrive — Sistema de Gestión y Alquiler de Vehículos de Alta Gama

**Autor:** Iker Raimundo Cantó Alarcón  
**NIA:** 10886882  
**Ciclo Formativo:** 2.º C.F.G.S. Administración de Sistemas Informáticos en Red (ASIR)  
**Módulo:** Proyecto Intermodular (Código: IFC_ASIR_2_01_IRCA)  

---

## Descripción del Proyecto
LuxDrive es una propuesta de empresa dedicada al alquiler de vehículos de lujo y deportivos (Porsche, Audi, BMW, etc.) que cuenta con una plataforma tecnológica propia para centralizar la gestión del negocio. 

El proyecto resuelve la problemática de la información dispersa en hojas de cálculo y la falta de control sobre la flota, ofreciendo un portal web público para reservas online y un panel de administración privado para empleados. Todo ello se respalda mediante una infraestructura informática propia, virtualizada, segura y de alta disponibilidad.

---

## Objetivo de la Infraestructura
El objetivo principal de esta infraestructura es proporcionar un entorno de red hiperconvergente, aislado y resiliente para dar soporte al modelo de negocio de LuxDrive:

- **Segmentación de Red y Aislamiento:** Red dividida en 4 VLANs controladas mediante un cortafuegos pfSense en DMZ para evitar accesos directos desde Internet a la base de datos interna.
- **Virtualización Híbrida Eficiente:** Despliegue de servicios sobre Proxmox VE utilizando máquinas virtuales (KVM) para servicios críticos y contenedores (LXC) para aplicaciones ligeras.
- **Acceso Administrativo Seguro:** Administración centralizada desde un Bastion Host en VLAN de Gestión mediante claves SSH cifradas y conexión remota obligatoria por VPN WireGuard.
- **Resiliencia y Redundancia:** Política de copias de seguridad inmutables mediante Proxmox Backup Server y almacenamiento en espejo ZFS, garantizando RTO <= 1 hora y RPO <= 15 minutos.
- **Automatización e Idempotencia:** Despliegue y configuración declarativa de la infraestructura mediante Ansible, manteniendo la custodia segura de secretos con Ansible Vault.

---

## Tecnologías Utilizadas

| Categoría | Tecnología Seleccionada | Uso en LuxDrive |
| :--- | :--- | :--- |
| **Virtualización** | Proxmox VE (KVM / LXC) | Orquestación de nodos y gestión de recursos del CPD local. |
| **Sistemas Operativos** | FreeBSD (pfSense) / Debian 12 / Ubuntu 24.04 | Sistema operativo del firewall perimetral y nodos de servicio. |
| **Servidor Web** | Nginx | Proxy inverso, servidor web para la aplicación de reservas y terminación SSL/TLS. |
| **Base de Datos** | MariaDB | Sistema Gestor de Bases de Datos relacional para catálogo, clientes y reservas. |
| **Automatización** | Ansible / Docker | Infraestructura como Código (IaC) e inventarios para despliegues idempotentes. |
| **Seguridad y Red** | pfSense / Suricata / WireGuard | Cortafuegos perimetral, sistema IDS/IPS y VPN para gestión remota. |
| **Monitorización** | Prometheus + Grafana | Cuadro de mando en tiempo real para métricas de servidores y PUE energético. |
| **Copias de Seguridad**| Proxmox Backup Server (PBS) + ZFS | Copias de seguridad inmutables, deduplicadas y réplicas off-site. |
| **Control de Versiones**| Git / GitHub | Control de versiones del código de automatización y documentación. |

---

## Estructura del Repositorio

```text
LuxDrive-IaC/
├── playbooks/              # Libros de jugadas de Ansible para ejecución automatizada
│   ├── site.yml            # Playbook maestro del sistema
│   ├── desplegar_web.yml   # Despliegue del frontal Nginx
│   └── configurar_bd.yml   # Aprovisionamiento de MariaDB
├── roles/                  # Módulos de configuración reutilizables
│   ├── firewall/           # Reglas de pfSense y segmentación VLAN/DMZ
│   ├── servicios_web/      # Configuración del servidor Nginx y portal web
│   ├── base_datos/         # Estructura e instalación del SGBD
│   ├── monitorizacion/     # Agentes de métricas Prometheus y dashboards Grafana
│   └── seguridad/          # Bastion host, hardening SSH y configuración VPN WireGuard
├── inventario/             # Definición de hosts y direcciones IP de los nodos
│   ├── hosts.ini           # Listado de IPs del laboratorio
│   └── group_vars/         # Variables aplicadas a grupos de máquinas
│       └── all.yml
├── plantillas/             # Archivos de configuración dinámicos (Jinja2)
│   ├── nginx_luxdrive.conf.j2
│   └── backup_script.sh.j2
├── variables/              # Custodia de credenciales y contraseñas
│   └── vault.yml           # Variables cifradas mediante Ansible Vault
├── documentacion/          # Esfuerzo técnico y blueprints del proyecto
│   └── arquitectura.md
└── README.md               # Archivo de presentación del proyecto
