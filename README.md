# TP Integrador – Laboratorio de Redes I

Infraestructura de red virtualizada para una **pizzería**, armada con VirtualBox y Debian 13. Incluye una aplicación web con login y ABM de productos, base de datos, DHCP, servidor de archivos (Samba), FTP y backups automáticos.

- **Materia:** Laboratorio de Redes I
- **Fecha de entrega:** 11/11/2026

## Arquitectura

Red host-only con 4 servidores Debian y un cliente Windows 10.

| Servidor | Hostname | IP | Función |
|----------|----------|----|---------|
| DHCP | dhcp | 192.168.10.10 | Asignación de direcciones IP |
| Web | web | 192.168.10.11 | Aplicación web de la pizzería (PHP) |
| Base de datos | db | 192.168.10.12 | MariaDB, base `pizzeria` |
| Archivos | files | 192.168.10.13 | Samba (file server) y FTP |
| Cliente | – | DHCP | Windows 10, mapea la unidad de red de Samba |


## Seguridad

- Todos los servidores aceptan SSH únicamente por el puerto **22035**.
- El servidor web tiene `ufw` configurado para permitir solo los puertos **80** (HTTP) y **22035** (SSH); el resto está bloqueado.

## Aplicación web

Aplicación en PHP para una pizzería:

- Tienda pública con los productos
- Login de administrador
- ABM de productos

Las credenciales de la base de datos **no** están en el repo.

## Backups

Un cron diario genera backups de la web y de la base de datos, con la fecha en el nombre del archivo. Se conservan los últimos 5 (el sexto sobreescribe al primero) y se suben por FTP al servidor de archivos, cerrando la conexión al terminar.

Correr backup: /usr/local/bin/backup_pizzeria.sh
Ver resultado: ls -l /var/backups/pizzeria

## Estructura del repositorio

```
.
├── README.md
├── .gitignore
├── web/        Código de la aplicación web
├── configs/    Archivos de configuración de cada servidor
├── scripts/    Scripts de backup y cron
└── docs/       Documentación técnica y funcional, diagrama de red, Excel del modelo de negocio
```
