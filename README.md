# Desplegar Jellyfin con Caddy como Proxy Inverso usando Docker

Este proyecto proporciona un **playbook de Ansible** para desplegar **Jellyfin** con **Caddy** como proxy inverso en un servidor VPS utilizando **Docker**.

## 🚀 Características
- **Despliegue automatizado** de Jellyfin en Docker.
- **Caddy como proxy inverso** con configuración automática de HTTPS.
- **Redes Docker separadas** para Jellyfin y el proxy.
- **Configuración sencilla y reproducible** con Ansible.

---

## 📋 Requisitos
### Servidor VPS:
- **Sistema operativo:** Debian 12 o Ubuntu 22.04
- **Ansible instalado** en tu máquina de control
- **Docker y Docker Compose** instalados en el VPS

### Dependencias de Ansible:
Si no tienes Ansible instalado, ejecútalo en tu máquina local:
```bash
sudo apt update && sudo apt install ansible -y
```

Instala los módulos de Ansible necesarios para gestionar Docker:
```bash
ansible-galaxy collection install community.docker
```

---

## ⚙️ Instalación
### 1️⃣ Configurar inventario de Ansible
Edita tu archivo `inventory.ini` y coloca la dirección IP de tu VPS:
```ini
[vps]
mi-servidor ansible_host=XXX.XXX.XXX.XXX ansible_user=root
```

### 2️⃣ Ejecutar el playbook
Para desplegar **Jellyfin con Caddy**, usa el siguiente comando:
```bash
ansible-playbook -i inventory.ini playbook.yml
```

---

## 🖥️ Acceso a Jellyfin
Después de ejecutar el playbook, podrás acceder a Jellyfin en:
```
http://jellyfin.yesweresysadmin.cat
```
Si configuraste correctamente el certificado SSL, también estará disponible en HTTPS:
```
https://jellyfin.yesweresysadmin.cat
```

---

## 🛠️ Configuración del Playbook
El playbook realiza las siguientes tareas:
1. **Actualiza el sistema** e instala dependencias.
2. **Crea redes Docker** para Jellyfin y el proxy.
3. **Despliega el contenedor Jellyfin** en Docker.
4. **Configura Caddy** para manejar peticiones HTTPS.
5. **Despliega el contenedor Caddy** y lo vincula con Jellyfin.

---

## 📝 Personalización
Si deseas cambiar el dominio o el correo para los certificados SSL, edita esta sección del **playbook.yml**:
```yaml
- name: Crear archivo de configuración de Caddy
  copy:
    dest: /etc/caddy/Caddyfile
    content: |
      jellyfin.yesweresysadmin.cat {
        reverse_proxy jellyfin:8096
        tls nalvali057@g.educaand.es
      }
    force: yes
```
Sustituye `jellyfin.yesweresysadmin.cat` por tu dominio y **modifica el correo electrónico** por el tuyo para generar certificados Let’s Encrypt correctamente.

---

## 🛑 Desinstalación
Si deseas eliminar los contenedores y la configuración, ejecuta:
```bash
docker rm -f jellyfin caddy && docker network rm jellyfin_network proxy_network
```
Esto eliminará los contenedores y las redes creadas.

---

## 📜 Licencia
Este proyecto está bajo la **Licencia MIT**, lo que significa que puedes usarlo, modificarlo y distribuirlo libremente.

📌 **Autor:** Nicolás Álvarez Aliaga

