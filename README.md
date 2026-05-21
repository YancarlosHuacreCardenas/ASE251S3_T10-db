# 🗄️ ASE251S3 T10 - Base de Datos (SQL Server en Docker)

Este repositorio contiene la configuración de Docker para levantar la base de datos **SQL Server 2022 Express** en una instancia AWS EC2.

---

## 📁 Estructura del Proyecto

```
📦 ASE251S3_T10-db
 ┗ 📂 docker-compose-yancarlos
    ┣ 📄 docker-compose-db.yml   ← Configuración del contenedor SQL Server
    ┗ 📄 Dockerfile              ← Build multi-stage para el backend Spring Boot
```

---

## ⚙️ Requisitos Previos

- Acceso SSH a la instancia EC2 con la clave `bd.pem`
- Docker y Docker Compose instalados en la instancia
- Puerto **1433** abierto en el Security Group de AWS

---

## 🚀 Levantar la Base de Datos en AWS EC2

### 1. Conectarse a la instancia EC2

```bash
ssh -i "bd.pem" ubuntu@ec2-52-204-14-191.compute-1.amazonaws.com
```

### 2. Ir a la carpeta del proyecto

```bash
cd ~/docker-compose-yancarlos
```

### 3. Levantar el contenedor de SQL Server

```bash
sudo docker compose -f docker-compose-db.yml up -d
```

Esto hará lo siguiente:
- Descargará la imagen `mcr.microsoft.com/mssql/server:2022-latest` (si no está disponible localmente)
- Creará la red compartida `my-network`
- Iniciará el contenedor `sql-server` en segundo plano en el puerto `1433`

---

## 🛑 Detener la Base de Datos

```bash
sudo docker compose -f docker-compose-db.yml down
```

---

## 📋 Comandos Útiles para Gestionar el Contenedor

| Comando | Descripción |
|---|---|
| `sudo docker ps` | Ver contenedores corriendo |
| `sudo docker logs -f sql-server` | Ver logs en tiempo real del SQL Server |
| `sudo docker restart sql-server` | Reiniciar el contenedor |
| `sudo docker exec -it sql-server bash` | Entrar al contenedor interactivamente |
| `sudo docker network ls` | Listar redes Docker disponibles |
| `sudo docker inspect my-network` | Ver detalles de la red compartida |

---

## 🔌 Datos de Conexión a la Base de Datos

| Parámetro | Valor |
|---|---|
| **Host** | `ec2-52-204-14-191.compute-1.amazonaws.com` |
| **Puerto** | `1433` |
| **Usuario** | `SA` |
| **Contraseña** | `Yancarlos123!` |
| **Edición** | SQL Server 2022 Express |

---

## 🌐 Red Compartida entre Servicios

Este `docker-compose` crea la red `my-network` con el driver `bridge`. Esta red es compartida y puede ser utilizada por el servicio del backend (Spring Boot) para conectarse a la base de datos sin necesidad de exponer el puerto externamente entre contenedores.

---

## 🐳 Construir la Imagen del Backend (Spring Boot)

Si necesitas construir la imagen de la aplicación Spring Boot:

```bash
# Desde la carpeta que contiene el Dockerfile del backend
sudo docker build -t yancarlos18/springboot-sqlserver:1.0 .
```

---

## 👤 Autor

**Yancarlos Huacre Cardenas** — ASE251S3 Grupo T10
