Aquí tienes una versión **ampliada, detallada y mucho más profesional**.

Esta versión no solo incluye los comandos, sino que explica **qué hace cada flag**, añade una sección de **comandos útiles** para el día a día y consejos para encontrar la IP de tu Raspberry. Es perfecta para documentar un repositorio serio de HomeLab.

-----

# 🐳 Ultimate Guide: Docker & Portainer en Raspberry Pi

Esta guía documenta el proceso paso a paso para transformar una Raspberry Pi en un servidor de contenedores robusto utilizando **Docker Engine** y **Portainer CE** como interfaz gráfica de gestión.

-----

## 📋 Índice de Contenidos

1.  📝 Requisitos Previos.
2.  🐋 Instalación de Docker.
3.  🔐 Gestión de Permisos de Usuario.
4.  🧪 Verificación de la Instalación.
5.  🚢 Despliegue de Portainer CE.
6.  🖥️ Acceso y Configuración Inicial.
7.  ⚡ Cheatsheet: Comandos Útiles.

-----

## 📝 Requisitos Previos

  * Una **Raspberry Pi** (3B+ o 4/5 recomendada) con Raspberry Pi OS instalado.
  * Conexión a internet.
  * Acceso a la terminal (vía SSH o monitor directo).

-----

## 🐋 Paso 1: Instalación de Docker y Docker Compose

Docker es la plataforma que nos permitirá empaquetar y ejecutar aplicaciones en entornos aislados llamados contenedores. Usaremos el script oficial de instalación automatizada.

### 1.1 Descargar el script de instalación

```bash
sudo curl -fsSL https://get.docker.com/ -o get-docker.sh
```

### 1.2 Ejecutar la instalación

Este comando instalará automáticamente las dependencias necesarias, Docker Engine y el plugin de Docker Compose.

```bash
sudo sh get-docker.sh
```

> ⏳ **Nota:** Este proceso puede tardar unos minutos dependiendo de tu conexión a internet y modelo de Raspberry Pi.

-----

## 🔐 Paso 2: Gestión de Permisos de Usuario

Por defecto, Docker requiere privilegios de `root` (usar `sudo` constantemente). Para evitar esto y mejorar la seguridad, añadimos nuestro usuario actual al grupo `docker`.

### 2.1 Añadir usuario al grupo

```bash
sudo usermod -aG docker ${USER}
```

### 2.2 Aplicar los cambios

Es estrictamente necesario reiniciar la sesión o el sistema para que los cambios de grupos surtan efecto.

```bash
sudo reboot
```

-----

## 🧪 Paso 3: Verificación de la Instalación

Una vez reiniciado el sistema, verificamos que el daemon de Docker está corriendo correctamente ejecutando el contenedor de prueba oficial.

```bash
docker run hello-world
```

**Resultado esperado:**
Deberías ver un mensaje que comienza con:

> *Hello from Docker\! This message shows that your installation appears to be working correctly.*

-----

## 🚢 Paso 4: Despliegue de Portainer CE

**Portainer** es una herramienta esencial que proporciona una interfaz web amigable para gestionar contenedores, imágenes, redes y volúmenes sin necesidad de recordar comandos complejos de terminal.

### 4.1 Crear volumen persistente

Creamos un volumen dedicado para asegurar que la configuración de Portainer (usuarios, contraseñas, entornos) no se pierda si reiniciamos el contenedor.

```bash
docker volume create portainer_data
```

### 4.2 Descargar e iniciar el contenedor

Ejecuta el siguiente comando en bloque:

```bash
docker run -d \
  -p 8000:8000 \
  -p 9443:9443 \
  --name portainer \
  --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:latest
```

**🔍 Desglose del comando:**

  * `-d`: Ejecuta en segundo plano (Detached mode).
  * `-p 9443:9443`: Expone el puerto HTTPS de la interfaz web.
  * `--restart=always`: Si la Raspberry se reinicia, Portainer arrancará automáticamente.
  * `-v /var/run/docker.sock...`: Permite a Portainer comunicarse con el motor de Docker.

### 4.3 Comprobar estado

Verifica que Portainer está en la lista de procesos activos:

```bash
docker ps
```

-----

## 🖥️ Paso 5: Acceso y Configuración Inicial

### 5.1 Encontrar tu IP

Si no conoces la dirección IP de tu Raspberry Pi, ejecuta:

```bash
hostname -I
```

### 5.2 Acceder vía Navegador

Abre tu navegador web y ve a:
`https://[TU-IP]:9443`

> ⚠️ **Aviso de Seguridad:** Al usar HTTPS con un certificado autofirmado, tu navegador te mostrará una advertencia de "La conexión no es privada". Debes hacer clic en **Configuración avanzada** -\> **Continuar a... (inseguro)**. Esto es normal y seguro en tu red local.

### 5.3 Primer Login

1.  Crea el usuario `admin` y establece una contraseña segura.
2.  Selecciona el entorno **"Get Started"** (Local).
3.  ¡Listo\! Ya tienes control total sobre tus contenedores.

-----

## ⚡ Extra: Cheatsheet Comandos Útiles

Aquí tienes una lista rápida de comandos que usarás frecuentemente:

| Acción | Comando |
| :--- | :--- |
| **Listar contenedores activos** | `docker ps` |
| **Listar TODOS los contenedores** | `docker ps -a` |
| **Ver logs de un contenedor** | `docker logs -f [nombre_contenedor]` |
| **Detener un contenedor** | `docker stop [nombre_contenedor]` |
| **Eliminar un contenedor** | `docker rm [nombre_contenedor]` |
| **Descargar actualización imagen** | `docker pull [imagen:tag]` |
| **Limpieza de sistema** | `docker system prune` |

-----
