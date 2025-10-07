# Guía Completa de Linux

Guía modular de sistemas y servicios clave de Linux, comandos útiles y ejemplos prácticos.

- Incluye tutorial de Vim para moverse por terminal
- Incluye un apartado especial sobre el kernel de Linux para entender su funcionamiento y configuración
- Aprenderemos a modificar parámetros del kernel y compilar uno personalizado

---

## 📚 Índice de Contenidos

### 1. [Introducción](docs/01-introduccion.md)
- Introducción a la guía
- Distribuciones de Linux en entornos empresariales
- Criterios para elegir una distribución

### 2. [Comandos Básicos](docs/02-comandos-basicos.md)
- Tipos de comandos en Linux con `compgen`
- Comandos internos (built-ins)
- Comandos externos (binarios)
- Alias

### 3. [Man Pages](docs/03-man-pages.md)
- El comando MAN
- Cómo leer la sintaxis de comandos
- Secciones del manual
- Crear tus propias man pages

### 4. [Navegación por el Filesystem](docs/04-navegacion-filesystem.md)
- Jerarquía de directorios en Linux
- Comandos de movimiento (`pwd`, `cd`, `ls`)
- El comando `tree`

### 5. [Visualización de Ficheros](docs/05-visualizacion-ficheros.md)
- Comandos de lectura (`cat`, `less`, `head`, `tail`)
- Filtros y combinaciones (`grep`, `sort`, `uniq`)
- Análisis de logs

### 6. [Rutas en Linux](docs/06-rutas.md)
- Rutas absolutas vs relativas
- Símbolos especiales (`.`, `..`, `~`, `-`)
- Variables de entorno
- Consejos prácticos

### 7. [Manipulación del Filesystem](docs/07-manipulacion-filesystem.md)
- Crear archivos y directorios (`mkdir`, `touch`)
- Copiar, mover y renombrar (`cp`, `mv`)
- Eliminar archivos y directorios (`rm`, `rmdir`)

### 8. [Gestión de Paquetes](docs/08-gestion-paquetes.md)
- APT (Debian/Ubuntu)
- DNF/YUM (RHEL/CentOS/Rocky)
- Zypper (openSUSE)
- Pacman (Arch Linux)
- Gestores universales (Snap, Flatpak, AppImage)
- Compilación desde código fuente

### 9. [Usuarios y Permisos](docs/09-usuarios-permisos.md)
- Conceptos básicos
- Tipos de usuarios
- Ficheros `/etc/passwd` y `/etc/shadow`
- Gestión de usuarios y grupos
- Permisos en Linux (chmod, chown)
- Permisos especiales (SUID, SGID, Sticky Bit)

---

## 🚧 Contenido en desarrollo

Los siguientes módulos están en proceso de creación:

- **Procesos y gestión del sistema**
- **Redes en Linux**
- **Servicios y systemd**
- **Scripts en Bash**
- **Editor Vim**
- **El kernel de Linux**
- **Logs y troubleshooting**
- **SSH y acceso remoto**

---

## 📝 Notas

Esta guía está organizada de forma modular para facilitar su mantenimiento y consulta. Cada tema está en un fichero separado en la carpeta `docs/`.

---

## 🤝 Contribuciones

Esta guía es un trabajo en progreso. Si encuentras errores o quieres sugerir mejoras, no dudes en contribuir.

---

**Última actualización:** Octubre 2025
