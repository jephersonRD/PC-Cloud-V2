# 🖥️ **PC-Free**: Linux Desktop en Docker via GitHub Codespace 🚀

💡 Ejecuta un entorno de escritorio **Linux completo** directamente desde la nube, sin necesidad de hardware potente. 🐳 Basado en **Docker** + **GitHub Codespace**.

---

## ⚠️ Requisitos Importantes

🔐 **¡ATENCIÓN!** Para que este proyecto funcione correctamente, **debes seguir esta cuenta de GitHub**: 👉 [@jephersonRD](https://github.com/jephersonRD)

Algunas funcionalidades no se activarán si no cumples este requisito.

---

## ✅ Requisitos Previos

Antes de comenzar, asegúrate de tener:

* 🧑‍💻 **GitHub Codespace habilitado**
* 🐋 **Docker instalado y funcionando**

---

## ⚙️ Instalación y Configuración

### 1️⃣ Verifica tu espacio en disco

```bash
df -h
```

🔎 Selecciona la partición con **más espacio libre**.

### 2️⃣ Crea un directorio de datos para Docker

```bash
sudo mkdir -p /tmp/docker-data
```

### 3️⃣ Configura Docker para usar ese directorio

Edita el archivo de configuración:

```bash
sudo nano /etc/docker/daemon.json
```

Agrega lo siguiente:

```json
{
  "data-root": "/tmp/docker-data"
}
```

### 4️⃣ Reinicia tu Codespace

🔁 Reinicia para aplicar los cambios de configuración.

### 5️⃣ Verifica la configuración de Docker

```bash
docker info
```

✅ Asegúrate de que `Docker Root Dir` apunte a `/tmp/docker-data`.

---

## 🧱 Configuración del Contenedor

### 📄 Archivo `linux-desktop.yml`

```yaml
# ⚠️ Antes de ejecutar docker-compose up, asegúrate de seguir:
# https://github.com/jephersonRD
# Ejecuta este script para comprobarlo:
# bash check_github_follow.sh || exit 1

version: '3.8'

services:
  linux-desktop:
    image: linuxserver/webtop:ubuntu-kde
    container_name: linux-desktop
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/New_York
      - KEYBOARD=en-us-qwerty
      - WEBPAGE_TITLE=Linux Desktop
      - RESOLUTION=1920x1080
      - SUDO_PASSWORD=123456
      - NOVNC_ENABLE=true
      - VNC_PASSWORD=vnc123456
    ports:
      - "3000:3000"
      - "5900:5900"
    volumes:
      - linux-data:/config
      - /dev/shm:/dev/shm
    cap_add:
      - SYS_ADMIN
    devices:
      - /dev/fuse
    security_opt:
      - seccomp:unconfined
    restart: unless-stopped

volumes:
  linux-data:
```

---

## ▶️ Ejecución

### 🔌 Inicia el contenedor

```bash
docker-compose -f linux-desktop.yml up -d
```

### 🌐 Accede al escritorio

* **Desde el navegador**: `http://localhost:3000`
* **Desde un cliente VNC**: `localhost:5900` 🔑 Contraseña VNC: `vnc123456`

---

## 🛠️ Gestión del Contenedor

```bash
# 📄 Ver logs
docker-compose -f linux-desktop.yml logs -f

# ⏹️ Detener
docker-compose -f linux-desktop.yml down

# 🔄 Reiniciar
docker-compose -f linux-desktop.yml restart

# 📋 Ver estado
docker ps
```

---

## 🔐 Seguridad

🔒 **Recomendaciones importantes**:

* Cambia las contraseñas por defecto si usas este entorno en producción
* No subas credenciales al repositorio
* Usa variables de entorno para datos sensibles
* Considera agregar autenticación adicional

---

## 🧯 Resolución de Problemas

### 🚫 El contenedor no inicia

```bash
docker-compose -f linux-desktop.yml logs
df -h
```

### 🐢 Problemas de rendimiento

```bash
htop
docker stats
```

### ❌ Fallos al conectar por VNC

* Verifica el puerto `5900`
* Confirma la contraseña VNC
* Revisa configuración del firewall

---

## 🧰 Buenas Prácticas

✅ **Consejos útiles**:

* Usa volúmenes Docker para mantener tus datos
* Haz backups regularmente
* Monitorea el uso de CPU/RAM
* Mantén la imagen Docker actualizada

---

## 🤝 Contribuir

¡Tu ayuda es bienvenida! Para contribuir:

1. Haz un **fork** del repositorio
2. Crea una rama:
   ```bash
   git checkout -b feature/tu-mejora
   ```
3. Haz commits con tus cambios:
   ```bash
   git commit -am "Agrega nueva funcionalidad"
   ```
4. Haz push:
   ```bash
   git push origin feature/tu-mejora
   ```
5. Abre un **Pull Request**

---

## 📄 Licencia

Distribuido bajo la licencia **MIT**. Consulta el archivo `LICENSE` para más detalles.

---

## 💬 Soporte

¿Tienes dudas o problemas? 👉 Abre un issue en el repositorio.

---

🧑‍💻 **Diseñado para desarrolladores que necesitan un entorno Linux completo, sin depender de su hardware físico.**
