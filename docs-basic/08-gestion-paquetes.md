# Instalación de software y gestores de paquetes

La gestión de software en Linux se realiza principalmente a través de **gestores de paquetes**, que automatizan la instalación, actualización y eliminación de programas. Cada distribución tiene su propio ecosistema de paquetes y herramientas.

## ¿Qué es un paquete?

Un **paquete** es un archivo que contiene:
- Los binarios del programa
- Archivos de configuración
- Documentación
- Scripts de instalación/desinstalación  
- **Metadatos**: dependencias, versión, descripción, etc.

Los paquetes resuelven automáticamente las **dependencias** (librerías y otros programas necesarios).

## Comprobar si el software ya está instalado

Antes de instalar un programa, es buena práctica verificar si ya está instalado en el sistema y en qué versión.

Hay varias formas de hacerlo:

```bash
# Comprobar si el binario está en el PATH:
which nombre_programa

# Alternativa a which:
# whereis busca en una lista de directorios estándar, no depende del PATH.
# Muestra dónde están el binario, sus páginas man y, a veces, el código fuente.
whereis nombre_programa

# Si conoces el nombre del paquete, puedes verificar si está instalado y en qué versión:
dpkg -l | grep nombre_paquete  # Debian/Ubuntu
```

## Gestores de paquetes por distribución

### Debian/Ubuntu - APT (Advanced Package Tool)

**Comando principal:** `apt` (versión moderna) y `apt-get` (versión clásica)

**Archivos de configuración:**

- `/etc/apt/sources.list` → repositorios principales
- `/etc/apt/sources.list.d/` → repositorios adicionales

**Comandos esenciales:**

```bash
# Actualizar lista de paquetes disponibles
sudo apt update

# Actualizar todos los paquetes instalados
sudo apt upgrade

# Actualizar distribución (upgrade + manejo de dependencias complejas)
sudo apt dist-upgrade

# Instalar un paquete
sudo apt install nombre_paquete

# Instalar múltiples paquetes
sudo apt install paquete1 paquete2 paquete3

# Desinstalar un paquete (mantiene configuración)
sudo apt remove nombre_paquete

# Desinstalar completamente (incluye configuración)
sudo apt purge nombre_paquete

# Buscar paquetes
apt search palabra_clave

# Mostrar información de un paquete
apt show nombre_paquete

# Listar paquetes instalados
apt list --installed

# Limpiar caché de paquetes descargados
sudo apt autoclean

# Eliminar paquetes huérfanos (no necesarios)
sudo apt autoremove
```

**Gestión de repositorios:**

```bash
# Añadir repositorio PPA (Personal Package Archive)
sudo add-apt-repository ppa:usuario/repositorio

# Eliminar repositorio PPA
sudo add-apt-repository --remove ppa:usuario/repositorio

# Añadir clave GPG de repositorio. La clave asegura la autenticidad de los paquetes.
wget -qO - https://url-del-repositorio/key.gpg | sudo apt-key add -
```

### RHEL/CentOS/Rocky Linux - YUM/DNF

**YUM** (Yellowdog Updater Modified) en versiones antiguas, **DNF** (Dandified YUM) en versiones modernas.

**Archivos de configuración:**

- `/etc/yum.repos.d/` → definición de repositorios
- `/etc/dnf/dnf.conf` → configuración principal (DNF)

**Comandos esenciales DNF:**

```bash
# Actualizar metadatos de repositorios
sudo dnf check-update

# Actualizar todos los paquetes
sudo dnf update

# Instalar un paquete
sudo dnf install nombre_paquete

# Desinstalar un paquete
sudo dnf remove nombre_paquete

# Buscar paquetes
dnf search palabra_clave

# Mostrar información de un paquete
dnf info nombre_paquete

# Listar paquetes instalados
dnf list installed

# Listar repositorios habilitados
dnf repolist

# Limpiar caché
sudo dnf clean all

# Mostrar historial de transacciones
dnf history

# Deshacer una transacción
sudo dnf history undo ID
```

**Comandos YUM (sistemas antiguos):**

```bash
# Equivalentes a DNF pero con sintaxis yum
sudo yum update
sudo yum install nombre_paquete
sudo yum remove nombre_paquete
yum search palabra_clave
```

### openSUSE - Zypper

**Comando principal:** `zypper`

**Comandos esenciales:**

```bash
# Actualizar metadatos de repositorios
sudo zypper refresh

# Actualizar sistema
sudo zypper update

# Instalar paquete
sudo zypper install nombre_paquete

# Desinstalar paquete
sudo zypper remove nombre_paquete

# Buscar paquetes
zypper search palabra_clave

# Mostrar información de paquete
zypper info nombre_paquete

# Listar repositorios
zypper repos

# Añadir repositorio
sudo zypper addrepo URL_repositorio nombre_alias
```

### Arch Linux - Pacman

**Comando principal:** `pacman`

**Comandos esenciales:**

```bash
# Actualizar sistema completo
sudo pacman -Syu

# Instalar paquete
sudo pacman -S nombre_paquete

# Desinstalar paquete
sudo pacman -R nombre_paquete

# Desinstalar con dependencias huérfanas
sudo pacman -Rs nombre_paquete

# Buscar paquetes
pacman -Ss palabra_clave

# Buscar paquetes instalados
pacman -Qs palabra_clave

# Mostrar información de paquete
pacman -Si nombre_paquete

# Limpiar caché
sudo pacman -Sc
```

## Gestores de paquetes universales

### Snap (Canonical)

Paquetes autocontenidos que funcionan en múltiples distribuciones.

```bash
# Instalar snap si no está disponible (Ubuntu lo tiene por defecto)
sudo apt install snapd

# Instalar paquete snap
sudo snap install nombre_paquete

# Listar snaps instalados
snap list

# Actualizar todos los snaps
sudo snap refresh

# Desinstalar snap
sudo snap remove nombre_paquete

# Buscar snaps
snap find palabra_clave
```

### Flatpak

Otro sistema de paquetes universales, muy usado en entornos desktop.

```bash
# Instalar flatpak
sudo apt install flatpak  # Debian/Ubuntu
sudo dnf install flatpak  # RHEL/Fedora

# Añadir repositorio Flathub
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# Instalar aplicación
flatpak install nombre_aplicacion

# Ejecutar aplicación
flatpak run com.ejemplo.aplicacion

# Listar aplicaciones instaladas
flatpak list

# Actualizar aplicaciones
flatpak update
```

### AppImage

Paquetes portátiles que no requieren instalación.

```bash
# Descargar AppImage
wget https://github.com/proyecto/releases/app.AppImage

# Dar permisos de ejecución
chmod +x app.AppImage

# Ejecutar
./app.AppImage
```

## Compilación desde código fuente

Cuando un programa no está disponible en los repositorios de tu distribución, o necesitas una versión más reciente, puedes compilarlo directamente desde su código fuente.

Esto significa convertir el código (C, C++, etc.) en binarios ejecutables adaptados a tu sistema.

### Conceptos básicos

La compilación es el proceso de transformar el código fuente en ejecutables.

Normalmente sigue tres fases:

- Configurar → preparar el entorno y detectar dependencias.

- Compilar → traducir el código fuente a binarios.

- Instalar → copiar los binarios y archivos a las rutas del sistema.

### Proceso básico

```bash
# 1. Instalar herramientas de compilación
sudo apt install build-essential  # Debian/Ubuntu
sudo dnf groupinstall "Development Tools"  # RHEL/Fedora

# 2. Descargar código fuente
wget https://proyecto.org/codigo.tar.gz
tar -xzf codigo.tar.gz
cd codigo/

# 3. Configurar compilación. El script configure analiza tu sistema:
# Verifica que tengas las librerías necesarias.
# Detecta la arquitectura (x86_64, ARM, etc.).
# Genera el archivo Makefile, donde se definen las reglas de compilación.
./configure --prefix=/usr/local

# 4. Compilar. 
# Make lee el archivo Makefile generado y ejecuta las instrucciones para construir los binarios.
make

# 5. Instalar. 
# Este paso copia los binarios y archivos a las rutas del sistema.
sudo make install
```

### Gestión con checkinstall

`checkinstall` crea un paquete a partir de `make install`, facilitando la desinstalación posterior.

```bash
# Instalar checkinstall
sudo apt install checkinstall

# Usar en lugar de make install
sudo checkinstall
```

## Buenas prácticas

### Seguridad

- **Siempre actualiza** antes de instalar: `sudo apt update && sudo apt install paquete`
- **Verifica las fuentes**: usa repositorios oficiales cuando sea posible
- **Revisa dependencias** antes de instalar software de terceros

### Mantenimiento

- **Limpia regularmente** el caché de paquetes
- **Elimina paquetes huérfanos** periódicamente
- **Mantén el sistema actualizado** con updates regulares

### Troubleshooting común

```bash
# Reparar dependencias rotas (Debian/Ubuntu)
sudo apt --fix-broken install

# Reconfigurar paquetes
sudo dpkg-reconfigure nombre_paquete

# Forzar instalación de dependencias
sudo apt install -f

# Ver qué paquetes están bloqueando una actualización
sudo apt list --upgradable
```

## Comparación de comandos comunes

| Acción | Debian/Ubuntu | RHEL/CentOS | openSUSE | Arch |
|--------|---------------|-------------|----------|------|
| Actualizar lista | `apt update` | `dnf check-update` | `zypper refresh` | `pacman -Sy` |
| Actualizar sistema | `apt upgrade` | `dnf update` | `zypper update` | `pacman -Syu` |
| Instalar | `apt install` | `dnf install` | `zypper install` | `pacman -S` |
| Desinstalar | `apt remove` | `dnf remove` | `zypper remove` | `pacman -R` |
| Buscar | `apt search` | `dnf search` | `zypper search` | `pacman -Ss` |
| Info | `apt show` | `dnf info` | `zypper info` | `pacman -Si` |
