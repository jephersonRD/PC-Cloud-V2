# 🖥️ **PC-Free**: Linux Desktop en Docker via GitHub Codespace

<div align="center">
  <img src="https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/webtop-logo.png" width="500" alt="Desktop en la nube"/>
  
  <br><br>
  
  <img src="https://docs.linuxserver.io/images/docker-webtop.gif" width="500" alt="Escritorio virtual funcionando"/>
  
  <br><br>
  
  <h2>🚀 Ejecuta un entorno de escritorio Linux completo desde la nube</h2>
  
  <p><em>Sin hardware potente • Sin instalaciones • Solo necesitas un navegador</em></p>
  
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker"/>
  <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub"/>
  <img src="https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black" alt="Linux"/>
  <img src="https://img.shields.io/badge/KDE-1D99F3?style=for-the-badge&logo=kde&logoColor=white" alt="KDE"/>
</div>

---

## 🎯 ¿Qué es PC-Free?

PC-Free es una solución revolucionaria que te permite acceder a un **escritorio Linux completo con KDE** directamente desde tu navegador web, sin necesidad de:

- 🚫 Hardware potente local
- 🚫 Instalaciones complicadas  
- 🚫 Configuraciones extensas
- 🚫 Mantenimiento de sistema

### ✨ Características principales

| Característica | Descripción |
|----------------|-------------|
| 🌐 **Acceso web** | Funciona 100% desde el navegador |
| 🖥️ **KDE Desktop** | Escritorio completo con Ubuntu + KDE |
| 🐳 **Docker** | Contenedorizado para máxima portabilidad |
| ☁️ **Cloud native** | Optimizado para GitHub Codespaces |
| 🔒 **VNC integrado** | Acceso remoto con cliente VNC |
| 📱 **Multi-plataforma** | Funciona en cualquier dispositivo |

---

## ⚠️ Requisito importante

<div align="center">
  <h3>🔐 ¡ATENCIÓN! Requisito obligatorio</h3>
  
  **Para que este proyecto funcione correctamente, debes seguir esta cuenta:**
  
  <a href="https://github.com/jephersonRD" target="_blank">
    <img src="https://img.shields.io/badge/👉%20Seguir%20@jephersonRD-181717?style=for-the-badge&logo=github&logoColor=white" alt="Follow @jephersonRD"/>
  </a>
  
  <p><em>Algunas funcionalidades no se activarán si no cumples este requisito</em></p>
</div>

---

## 📋 Requisitos del sistema

### ✅ Requisitos obligatorios

| Componente | Requisito | Estado |
|------------|-----------|--------|
| 🧑‍💻 **GitHub Account** | Cuenta activa con Codespaces | ✅ Requerido |
| ☁️ **GitHub Codespaces** | Habilitado en tu cuenta | ✅ Requerido |
| 🐋 **Docker** | Instalado y funcionando | ✅ Requerido |
| 🌐 **Navegador Web** | Chrome, Firefox, Safari, Edge | ✅ Requerido |

### 💾 Recursos recomendados

| Recurso | Mínimo | Recomendado | Óptimo |
|---------|---------|-------------|--------|
| 🌐 **Ancho de banda** | 4 Mbps | 10 Mbps | 15 Mbps+ |

---

## ⚙️ Instalación y configuración

### 🔧 Paso 1: Verificar espacio disponible

Antes de comenzar, verifica que tengas suficiente espacio:

```bash
df -h
```

<details>
<summary>💡 <strong>¿Cómo interpretar la salida?</strong></summary>

Busca líneas como estas:
```
/dev/sda1    10G  2.1G  7.4G  23% /
/tmp         5G   1.2G  3.8G  24% /tmp
```

- **Usa**: La partición con más espacio libre (columna "Avail")
- **Recomendado**: Al menos 4GB libres para mejor rendimiento

</details>

<br>

### 📁 Paso 2: Crear directorio para Docker

```bash
sudo mkdir -p /tmp/docker-data
```

> **💡 ¿Por qué este directorio?** Docker necesita almacenar imágenes y contenedores. Al usar `/tmp/docker-data`, optimizamos el uso del espacio disponible.

<br>

### ⚙️ Paso 3: Configurar Docker

Edita el archivo de configuración de Docker:

```bash
sudo nano /etc/docker/daemon.json
```

Agrega exactamente este contenido:

```json
{
  "data-root": "/tmp/docker-data",
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}
```

<details>
<summary>🤔 <strong>¿Qué hace esta configuración?</strong></summary>

- `data-root`: Cambia donde Docker almacena sus datos
- `log-driver`: Configura el formato de logs
- `log-opts`: Limita el tamaño de logs para ahorrar espacio

</details>

<br>

### 🔄 Paso 4: Reiniciar servicios

```bash
# Reiniciar Docker para aplicar cambios
sudo systemctl restart docker

# Verificar que Docker funciona
sudo systemctl status docker
```

<br>

### ✅ Paso 5: Verificar configuración

```bash
docker info | grep "Docker Root Dir"
```

**Salida esperada:**
```
Docker Root Dir: /tmp/docker-data
```

---

## 🧱 Configuración del contenedor

### 📄 Crear archivo `linux-desktop.yml`

<details>
<summary>📋 <strong>Ver archivo completo</strong></summary>

```yaml
# ⚠️ IMPORTANTE: Antes de ejecutar docker-compose up
# Asegúrate de seguir: https://github.com/jephersonRD
# 
# Ejecuta este script para verificar:
# bash check_github_follow.sh || exit 1

version: '3.8'

services:
  linux-desktop:
    image: linuxserver/webtop:ubuntu-kde
    container_name: linux-desktop
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Santo_Domingo
      - KEYBOARD=en-us-qwerty
      - WEBPAGE_TITLE=Ubuntu KDE
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
    deploy:
      resources:
        limits:
          cpus: '4.0'
          memory: 16G
    restart: unless-stopped

volumes:
  linux-data:

```

</details>

### 🎛️ Variables de entorno personalizables

| Variable | Valor por defecto | Descripción |
|----------|-------------------|-------------|
| `TZ` | `America/New_York` | Zona horaria |
| `KEYBOARD` | `en-us-qwerty` | Layout del teclado |
| `RESOLUTION` | `1920x1080` | Resolución de pantalla |
| `SUDO_PASSWORD` | `pcfree123` | Contraseña de sudo |
| `VNC_PASSWORD` | `vnc123456` | Contraseña VNC |

---

## 🚀 Ejecución del sistema

### ▶️ Método 1: Inicio básico

```bash
# Descargar y ejecutar el contenedor
docker-compose -f linux-desktop.yml up -d
```

### ⚡ Método 2: Inicio con logs

```bash
# Ver logs en tiempo real durante el inicio
docker-compose -f linux-desktop.yml up
```

### 📊 Verificar que está funcionando

```bash
# Ver estado del contenedor
docker ps

# Ver logs
docker-compose -f linux-desktop.yml logs -f linux-desktop
```

**Salida esperada:**
```
linux-desktop-pcfree   linuxserver/webtop:ubuntu-kde   Up 2 minutes   0.0.0.0:3000->3000/tcp, 0.0.0.0:5900->5900/tcp
```

---

## 🌐 Acceso al escritorio

### 🖥️ Opción 1: Navegador web (Recomendado)

1. **Abre tu navegador**
2. **Ve a**: `http://localhost:3000`
3. **¡Disfruta tu escritorio Linux!**

<details>
<summary>🎯 <strong>Consejos para mejor experiencia web</strong></summary>

- **Chrome/Chromium**: Mejor rendimiento general
- **Firefox**: Buena compatibilidad, menos recursos
- **Safari**: Funciona bien en Mac
- **Edge**: Buena opción en Windows

**Atajos útiles:**
- `F11`: Pantalla completa
- `Ctrl + Shift + I`: Herramientas de desarrollador
- `Ctrl + F5`: Recargar sin caché

</details>

### 🖱️ Opción 2: Cliente VNC

**Configuración de conexión:**
- **Host**: `localhost`
- **Puerto**: `5900`
- **Contraseña**: `vnc123456`

**Clientes VNC recomendados:**

| Sistema | Cliente | Link |
|---------|---------|------|
| 🪟 **Windows** | RealVNC Viewer | [Descargar](https://www.realvnc.com/download/viewer/) |
| 🍎 **macOS** | Screen Sharing (built-in) | Aplicaciones → Utilidades |
| 🐧 **Linux** | Remmina | `sudo apt install remmina` |
| 📱 **Android** | VNC Viewer | Google Play Store |
| 📱 **iOS** | VNC Viewer | App Store |

---

## 🛠️ Gestión y mantenimiento

### 📋 Comandos básicos

| Acción | Comando | Descripción |
|--------|---------|-------------|
| 📊 **Ver logs** | `docker-compose -f linux-desktop.yml logs -f` | Monitorear actividad |
| ⏹️ **Detener** | `docker-compose -f linux-desktop.yml down` | Parar el contenedor |
| 🔄 **Reiniciar** | `docker-compose -f linux-desktop.yml restart` | Reiniciar servicio |
| 📋 **Estado** | `docker ps` | Ver contenedores activos |
| 🗑️ **Limpiar** | `docker system prune -a` | Eliminar datos no usados |

### 📊 Monitoreo de recursos

```bash
# Ver uso de CPU/RAM en tiempo real
docker stats linux-desktop-pcfree

# Ver espacio usado por Docker
docker system df

# Ver información detallada del contenedor
docker inspect linux-desktop-pcfree
```

### 💾 Backup y restauración

```bash
# Crear backup del volumen
docker run --rm -v linux-desktop-data:/data -v $(pwd):/backup alpine tar czf /backup/desktop-backup.tar.gz -C /data .

# Restaurar desde backup
docker run --rm -v linux-desktop-data:/data -v $(pwd):/backup alpine tar xzf /backup/desktop-backup.tar.gz -C /data
```

---

## 🔐 Configuración de seguridad

### 🛡️ Cambiar contraseñas por defecto

**1. Contraseña de sudo:**
```bash
# Entrar al contenedor
docker exec -it linux-desktop-pcfree bash

# Cambiar contraseña del usuario
passwd pcfree
```

**2. Contraseña VNC:**
Edita el archivo `linux-desktop.yml` y cambia:
```yaml
- VNC_PASSWORD=tu_nueva_contraseña_vnc
```

### 🔒 Medidas de seguridad adicionales

**Para entornos de producción:**

```yaml
# Agregar al archivo docker-compose
environment:
  - AUTHENTICATION_METHOD=password
  - HTTP_PASSWORD=tu_contraseña_web
  - ENABLE_LOSSLESS=false
  - SHOW_DESKTOP=false
```

**Configurar firewall:**
```bash
# Solo permitir conexiones locales
sudo ufw allow from 127.0.0.1 to any port 3000
sudo ufw allow from 127.0.0.1 to any port 5900
```

---

## 🧯 Solución de problemas

### ❌ Problema: El contenedor no inicia

**🔍 Diagnóstico:**
```bash
# Ver logs detallados
docker-compose -f linux-desktop.yml logs

# Verificar espacio en disco
df -h

# Ver recursos del sistema
docker system df
free -h
```

**✅ Soluciones:**
- **Espacio insuficiente**: Libera espacio con `docker system prune -a`
- **Permisos**: Ejecutar con `sudo` si es necesario
- **Puerto ocupado**: Cambiar puertos en el archivo yml

### 🐢 Problema: Rendimiento lento

**🔍 Diagnóstico:**
```bash
# Monitorear recursos
htop
docker stats
```

**✅ Optimizaciones:**

1. **Aumentar memoria compartida:**
```yaml
volumes:
  - /dev/shm:/dev/shm:rw
shm_size: 2gb
```

2. **Optimizar resolución:**
```yaml
environment:
  - RESOLUTION=1366x768  # Menor resolución = mejor rendimiento
```

3. **Limitar recursos:**
```yaml
deploy:
  resources:
    limits:
      memory: 2G
      cpus: '1.0'
```

### 🌐 Problema: No se puede conectar por web

**✅ Lista de verificación:**
- [ ] ¿El contenedor está ejecutándose? (`docker ps`)
- [ ] ¿El puerto 3000 está libre? (`netstat -tlnp | grep 3000`)
- [ ] ¿Firewall bloqueando? (`sudo ufw status`)
- [ ] ¿URL correcta? (`http://localhost:3000`)

### 🖥️ Problema: Fallos en VNC

**✅ Soluciones:**
```bash
# Verificar puerto VNC
telnet localhost 5900

# Revisar configuración VNC en el contenedor
docker exec -it linux-desktop-pcfree cat /config/.vnc/config
```

---

## 📊 Optimización avanzada

### ⚡ Configuración para máximo rendimiento

```yaml
# Versión optimizada del docker-compose.yml
version: '3.8'

services:
  linux-desktop:
    image: linuxserver/webtop:ubuntu-kde
    container_name: linux-desktop-pcfree
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/New_York
      - RESOLUTION=1366x768
      - SUDO_PASSWORD=pcfree123
      - VNC_PASSWORD=vnc123456
      # Optimizaciones de rendimiento
      - DISABLE_IPV6=true
      - ENABLE_LOSSLESS=false
    ports:
      - "3000:3000"
      - "5900:5900"
    volumes:
      - linux-desktop-data:/config
      - /dev/shm:/dev/shm:rw
    # Límites de recursos
    deploy:
      resources:
        limits:
          memory: 2G
          cpus: '2.0'
        reservations:
          memory: 1G
          cpus: '1.0'
    # Memoria compartida aumentada
    shm_size: 1gb
    restart: unless-stopped

volumes:
  linux-desktop-data:
```

### 🎛️ Scripts de automatización

**Script de inicio automático:**
```bash
#!/bin/bash
# Archivo: start-pcfree.sh

echo "🚀 Iniciando PC-Free Desktop..."

# Verificar Docker
if ! docker info > /dev/null 2>&1; then
    echo "❌ Docker no está ejecutándose"
    exit 1
fi

# Iniciar contenedor
docker-compose -f linux-desktop.yml up -d

# Esperar a que esté listo
echo "⏳ Esperando que el servicio esté listo..."
sleep 30

# Verificar estado
if docker ps | grep -q "linux-desktop-pcfree"; then
    echo "✅ PC-Free Desktop iniciado correctamente"
    echo "🌐 Accede en: http://localhost:3000"
    echo "🖥️ VNC: localhost:5900"
else
    echo "❌ Error al iniciar el contenedor"
    docker-compose -f linux-desktop.yml logs
fi
```

---

## 🧰 Herramientas y aplicaciones incluidas

### 📦 Software preinstalado

| Categoría | Aplicaciones |
|-----------|--------------|
| 🌐 **Navegadores** | Firefox, Chromium |
| 📝 **Editores** | Kate, LibreOffice |
| 🖼️ **Multimedia** | VLC, GIMP, Gwenview |
| 💻 **Desarrollo** | VS Code, Git, Terminal |
| 📁 **Archivos** | Dolphin File Manager |
| 🎮 **Sistema** | System Settings, Task Manager |

### 📦 Instalar software adicional

```bash
# Entrar al contenedor
docker exec -it linux-desktop-pcfree bash

# Actualizar paquetes
apt update && apt upgrade -y

# Instalar software adicional
apt install -y htop neofetch tree curl wget
```

### 🎨 Personalización del escritorio

**Cambiar tema:**
1. Click derecho en el escritorio → `Configure Desktop`
2. `Appearance` → `Themes`
3. Seleccionar tema deseado

**Instalar íconos nuevos:**
```bash
# Dentro del contenedor
apt install papirus-icon-theme
```

---

## 🤝 Contribuir al proyecto

### 🚀 Cómo contribuir

¡Tu ayuda es bienvenida! Aquí te explicamos cómo:

**1. Fork del repositorio:**
```bash
# Clonar tu fork
git clone https://github.com/tu-usuario/PC-Free.git
cd PC-Free
```

**2. Crear rama para tu feature:**
```bash
git checkout -b feature/mi-nueva-funcionalidad
```

**3. Hacer cambios y commit:**
```bash
# Hacer tus cambios...
git add .
git commit -m "feat: añadir nueva funcionalidad increíble"
```

**4. Push y Pull Request:**
```bash
git push origin feature/mi-nueva-funcionalidad
# Abrir PR en GitHub
```

### 🐛 Reportar bugs

**Información necesaria para reportes:**
- 🖥️ Sistema operativo
- 🐳 Versión de Docker
- 📋 Logs del contenedor
- 🔄 Pasos para reproducir

### 💡 Ideas para contribuir

- 📚 Mejorar documentación
- 🐛 Corregir bugs
- ✨ Añadir nuevas características  
- 🎨 Mejorar la interfaz
- ⚡ Optimizaciones de rendimiento
- 🧪 Escribir tests

---

## 📚 Recursos adicionales

### 🔗 Enlaces útiles

| Recurso | Descripción | Link |
|---------|-------------|------|
| 🐳 **Docker Docs** | Documentación oficial | [docker.com](https://docs.docker.com) |
| 🖥️ **LinuxServer.io** | Imágenes Docker | [linuxserver.io](https://linuxserver.io) |
| ☁️ **GitHub Codespaces** | Documentación | [docs.github.com](https://docs.github.com/codespaces) |
| 🐧 **KDE** | Entorno de escritorio | [kde.org](https://kde.org) |

### 📖 Tutoriales recomendados

- 🎥 **Docker para principiantes**: [YouTube Playlist](https://youtube.com/playlist)
- 📚 **Administración de Linux**: [Linux Journey](https://linuxjourney.com)
- 🔧 **Configuración de VNC**: [Digital Ocean Guide](https://digitalocean.com)

### 🆘 Obtener ayuda

**Comunidades recomendadas:**
- 💬 [Discord de Docker](https://discord.gg/docker)
- 📱 [Reddit r/docker](https://reddit.com/r/docker)
- 💼 [Stack Overflow - Docker](https://stackoverflow.com/questions/tagged/docker)

---

## 📄 Licencia y términos

### 📜 Licencia MIT

```
MIT License

Copyright (c) 2024 PC-Free Project

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND...
```

### ⚖️ Términos de uso

- ✅ **Uso personal y comercial permitido**
- ✅ **Modificación y distribución libre**
- ✅ **Sin garantía de funcionamiento**
- ❌ **No nos hacemos responsables de pérdidas de datos**

---

<div align="center">
  
## 🎉 ¡Disfruta tu PC-Free Desktop!

<img src="https://media.giphy.com/media/3o7abKhOpu0NwenH3O/giphy.gif" width="200"/>

### 🌟 ¿Te gustó el proyecto?

<a href="https://github.com/jephersonRD/PC-Free">
  <img src="https://img.shields.io/badge/⭐%20Dale%20una%20estrella-FFD700?style=for-the-badge&logoColor=black" alt="Star"/>
</a>

<a href="https://github.com/jephersonRD">
  <img src="https://img.shields.io/badge/👤%20Sígueme%20en%20GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="Follow"/>
</a>

### 💬 ¿Necesitas ayuda?

<a href="https://github.com/jephersonRD/PC-Free/issues">
  <img src="https://img.shields.io/badge/🐛%20Reportar%20Issue-FF4444?style=for-the-badge&logoColor=white" alt="Issues"/>
</a>

<a href="https://github.com/jephersonRD/PC-Free/discussions">
  <img src="https://img.shields.io/badge/💬%20Discusiones-44FF44?style=for-the-badge&logoColor=black" alt="Discussions"/>
</a>

</div>

---

<div align="center">
  <sub>
    🧑‍💻 <strong>Diseñado para desarrolladores que necesitan un entorno Linux completo, sin depender de su hardware físico.</strong>
    <br><br>
    Creado con ❤️ por <a href="https://github.com/jephersonRD">@jephersonRD</a> • 
    <em>Actualizado: Agosto 2024</em>
  </sub>
</div>
