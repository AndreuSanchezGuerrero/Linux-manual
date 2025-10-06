# Introducción

Guía completa de sistemas y servicios clave de Linux, comandos útiles y ejemplos prácticos 

- Incluye tutorial de Vim para moverse por terminal. 

- Incluye un apartado especial sobre el kernel de Linux para entender su funcionamiento y configuración. Modificaremos parámetros del kernel y compilaremos uno personalizado . 

<br><br>

# Distribuciones de Linux en entornos empresariales

En el ecosistema Linux existen cientos de distribuciones, pero en la práctica solo unas pocas dominan el ámbito empresarial. En ámbito empresarial la elección no suele depender de preferencias personales, sino de factores como estabilidad, soporte a largo plazo, seguridad y ecosistema de herramientas.

- Ubuntu/Debian → comunidad enorme, flexibilidad, ecosistema muy activo.

- RHEL/Rocky → soporte empresarial fuerte, certificaciones de software/hardware.

- SUSE (SLES / openSUSE Leap / Micro) → muy sólido en entornos corporativos y cloud, gran enfoque en estabilidad y administración (YaST, SUSE Manager, Kubernetes con Rancher).

- Proxmox → especializado en virtualización y contenedores, rápido de montar, interfaz web lista para producción.

Al elegir, lo importante no son gustos, sino estabilidad, soporte y seguridad. Pregúntate:

- ¿Tiene soporte oficial o una comunidad activa que asegure continuidad?
- ¿Qué rapidez tiene en aplicar parches críticos?
- ¿Existe buena documentación y ecosistema de herramientas?
- ¿Cuánto cuesta mantenerla (soporte, formación, licencias)?
- ¿Equilibra estabilidad con flexibilidad para nuevas tecnologías?

<br><br>

# Tipos de comandos en Linux con `compgen`

Si en la terminal pulsas TAB dos veces, verás una lista enorme de comandos disponibles. Pero, ¿sabías que existen diferentes tipos de comandos en Linux?

En Bash, el comando interno `compgen` permite listar los diferentes tipos de comandos que existen en el sistema:

## 1. Comandos internos (built-ins)

Son los comandos que **viven dentro de la shell** y no existen como binarios separados.

Ver todos los builtins:

```bash
compgen -b
```

## 2. Comandos externos (binarios en $PATH)

Son los ejecutables del sistema almacenados en rutas como /bin, /usr/bin, /usr/local/bin.

```bash
compgen -c
```

> Nota: compgen -c muestra todos los comandos ejecutables (incluye builtins, alias y funciones).

Para ver únicamente los binarios, puedes filtrarlos con type:

```bash
compgen -c | while read cmd; do type "$cmd" | grep -q 'file' && echo "$cmd"; done
```

## 3. Alias

Son atajos definidos en la shell (por ti o por configuración del sistema).

Ver todos los alias:

```bash
compgen -a
```

Para ver los alias definidos en tu sistema, puedes usar el comando `alias` sin argumentos.

---

> **Lectura recomendada**: 
> [Entendiendo el comando compgen](https://fraterneo.blogspot.com/2012/07/entendiendo-el-comando-compgen.html)

---

<br><br>

# El comando MAN

El comando man (manual) es la herramienta esencial para consultar la documentación de cualquier comando en Linux. No se trata solo de mostrar ayuda: aprender a leer e interpretar correctamente las páginas del manual es una habilidad clave para todo administrador de sistemas.

### La sintaxis básica es:

```bash
man [sección] comando
```

- [sección] → opcional, indica la sección del manual donde buscar.
- comando → el nombre del comando, función o fichero que queremos consultar.

### Estructura de una página man:

Aunque puede variar, normalmente veremos:

- NAME → nombre del comando y breve descripción.
- SYNOPSIS → aquí se encuentra la sintaxis del comando. Es la parte más importante y, al principio, la más confusa.
- DESCRIPTION → explicación detallada de las opciones.
- OPTIONS → listado de parámetros que se pueden usar con el comando.
- EXAMPLES → ejemplos prácticos (no todos los comandos los incluyen).
- SEE ALSO → otros comandos relacionados.

### Cómo leer la sintaxis en SYNOPSIS:

- [] → los corchetes indican que el elemento es opcional.
- < > → los ángulos muestran un valor que debes sustituir, como <file> o <user>.
- | → significa “o”, debes elegir una de las opciones.
- ... → indica que el elemento puede repetirse varias veces.
- { } → agrupan opciones que van juntas.

### Ejemplo con el comando man ssh

![alt text](image.png)

Esta parte puede ser la más confusa de todas -> ssh [-46AaCfGgKkMNnqsTtVvXxYy]

¿Qué significa?

- Eso es un conjunto de opciones cortas que se pueden usar de manera independiente o combinadas.
- Los corchetes [...] → indican que todas son opcionales.
- Las letras dentro → cada una corresponde a una opción corta de ssh.
- -4 → fuerza a usar IPv4.
- -6 → fuerza a usar IPv6.
- -A → habilita reenvío de agente de autenticación.
- -a → deshabilita reenvío de agente.
- -C → habilita compresión.
- -q → modo silencioso (quiet).
- -v → modo verbose (muestra más información).
- -vvv → aún más detallado (debug).

### Secciones de man

En Linux, las páginas del manual están divididas en secciones (no solo comandos). Por eso, cuando ejecutas man -f comando (o whatis comando), te devuelve todas las secciones en las que aparece.

Por ejemplo:

```bash
man -f open
```

Salida:

```bash
open (1)   - open a file descriptor
open (2)   - open and possibly create a file
```

Repaso de secciones:

- Sección 1: Comandos y aplicaciones de Shell.
- Sección 2: Servicios básicos del núcleo: llamadas al sistema y códigos de error.
- Sección 3: Información sobre bibliotecas para programadores.
- Sección 4: Servicios de red: si TCP/IP o NFS están instalados. Controladores de dispositivos y protocolos de red.
- Sección 5: Formatos de archivo estándar: por ejemplo, muestra cómo es un archivo tar.
- Sección 6: Juegos.
- Sección 7: Archivos y documentos varios.
- Sección 8: Comandos de administración y mantenimiento del sistema.
- Sección 9: Especificaciones e interfaces poco conocidas del núcleo.

### Dónde se almacenan y cómo se buscan las man pages

En Linux, las páginas del manual se distribuyen por secciones (1, 2, 3, …) en rutas conocidas. 

Lo más habitual:

- /usr/share/man/ → man pages instaladas por el sistema.
- /usr/local/share/man/ → man pages instaladas localmente por el administrador (prioridad sobre /usr).
- /opt/<paquete>/share/man → algunos paquetes autoinstalados guardan ahí su documentación.
- /var/cache/man/ → cat pages (páginas preformateadas en caché) e índices.

### La variable MANPATH (y el fichero de configuración de man) dictan en qué árboles buscar y en qué orden.

Ver tu ruta efectiva:

manpath

### Inspeccionar dónde está una página concreta:
-  man -w ssh     # primera encontrada
- man -aw ssh    # muestra todas las rutas donde existe

<br><br>

# Crear tus propias man pages

Estructura básica del archivo

Una man page es un archivo de texto escrito en troff/groff con macros específicas.

El formato es:

```
.TH NOMBRE SECCIÓN "FECHA" "VERSIÓN" "AUTOR/EMPRESA"
.SH NAME
nombre \- breve descripción
.SH SYNOPSIS
cómo se usa el comando
.SH DESCRIPTION
explicación detallada
.SH OPTIONS
\-a   Explicación de la opción -a  
\-b   Explicación de la opción -b
.SH EXAMPLES
Ejemplos de uso reales
.SH SEE ALSO
otros comandos relacionados
```

### Ubicación

- /usr/local/share/man/man1/

### Nombre del archivo

- Nombre: comando.sección → ej: mi-tool.1

### Comprimir la página

gzip -9 /usr/local/share/man/man1/mi-tool.1

Esto genera: /usr/local/share/man/man1/mi-tool.1.gz

### Actualizar la base de datos de man

Para que man reconozca tu página:

sudo mandb -q /usr/local/share/man

### Probar tu man page

man mi-tool

### Y si quieres ver todas las ubicaciones donde se encuentra:

man -aw mi-tool

---

> **Lectura recomendada**: 
> [Linux man page guide - It’s FOSS](https://itsfoss.com/linux-man-page-guide)

---

<br> <br>

# Navegación por el filesystem

## Entendiendo la jerarquía de directorios

La jerarquía de directorios en Linux se basa en un filesystem en forma de árbol. Aquí hay algunos de los directorios más importantes que encontrarás:

- `/` : El directorio raíz, donde empieza la jerarquía de archivos.
- `/bin` : Ejecutables esenciales para el sistema.
- `/sbin` : Ejecutables del sistema, generalmente para administración.
- `/usr` : Históricamente: “Unix System Resources”, no “user”. Es la jerarquía principal de software del sistema (binarios, librerías, documentación...). Por ejemplo, `/usr/bin` a diferencia de `/bin`, contiene aplicaciones y utilidades adicionales no esenciales para el arranque.
- `/usr/local` : Software instalado localmente, fuera del gestor de paquetes del sistema.
- `/etc` : Archivos de configuración del sistema.
- `/home` : Contiene los directorios personales de los usuarios.
- `/var` : Datos variables: logs, bases de datos temporales.
- `/tmp` : Archivos temporales, se suelen borrar al reiniciar.
- `/dev` : Archivos de dispositivos (hardware).
- `/proc` : filesystem virtual que proporciona información sobre procesos y el sistema.
- `/sys` : filesystem virtual que expone información del kernel y dispositivos.
- `/mnt` : Punto de montaje temporal para sistemas de archivos.
- `/media` : Punto de montaje para medios extraíbles (USB, CD-ROM).
- `/lib` : Bibliotecas compartidas necesarias para ejecutar programas.
- `/opt` : Software adicional y paquetes de terceros.
- `/root` : Directorio personal del usuario root (administrador).

Para ver la estructura completa, puedes usar el comando `tree`. 

El comando tree muestra el contenido de un directorio en forma de árbol jerárquico, lo que permite visualizar fácilmente la estructura de carpetas y subcarpetas.

Si no tienes `tree` instalado, puedes instalarlo con:

En Debian/Ubuntu:

```bash
sudo apt install tree
```

En RHEL/CentOS:

```bash
sudo yum install tree
```

```bash
tree -L 2 /
```

El parámetro `-L 2` limita la profundidad del árbol a 2 niveles, para evitar una salida demasiado larga. La / indica que quieres ver la estructura desde el directorio raíz.


## Comandos de movimiento y ubicación

Para moverte por el filesystem y orientarte dentro de él, Linux ofrece varios comandos básicos pero muy potentes. Dominar estos comandos es esencial para trabajar de forma fluida desde la terminal.

### pwd — Print Working Directory

- Muestra la ruta completa del directorio actual.

### cd — Change Directory

- Cambia al directorio especificado.
- Existen dos formas de especificar rutas: absolutas y relativas.
- Ejemplos de ruta absoluta:
  - `cd /home/usuario/documentos` → Ir a documentos.
- Para las rutas relativas existen varios atajos:
  - `cd ..` → Sube un nivel (al directorio padre).
  - `cd ../..` → Sube dos niveles. Puedes combinar tantos `..` como necesites.
  - `cd ./` → Permanece en el directorio actual.
  - `cd -` → Vuelve al directorio anterior.
  - `cd` o `cd ~` → Va al directorio home del usuario actual.
  - `cd ~usuario` → Va al home de otro usuario (requiere permisos).
  - `cd /` → Va al directorio raíz.

### ls — List Directory Contents

- Muestra el contenido del directorio actual o de uno especificado.
- Ejemplos de uso:
  - `ls` → Lista los archivos y carpetas en el directorio actual.
  - `ls -a` → Muestra todos los archivos, incluidos los ocultos (que empiezan con .).
  - `ls -l` → Muestra una lista detallada (permisos, propietario, tamaño, fecha).
  - `ls -lah` → Muestra una lista detallada con tamaños legibles por humanos (ej. 1K, 234M, 2G).
  - `ls -R` → Lista los archivos y carpetas de forma recursiva.

# Visualización de ficheros

Objetivo: aprender a leer información sin modificarla, inspeccionar archivos de configuración, analizar logs y combinar herramientas de lectura con filtros como grep, sort o uniq.

Antes de manipular archivos, es fundamental saber cómo leerlos e interpretarlos correctamente. Linux ofrece varios comandos para este propósito. También necesitamos saber que son los logs y dónde encontrarlos.

Los logs son archivos donde el sistema y las aplicaciones registran eventos, errores y actividades. Son esenciales para diagnosticar problemas y entender el comportamiento del sistema.

## Comandos básicos

- `cat` → Muestra el contenido de un archivo.
  - -n → Numera las líneas.
  - -E → Muestra los finales de línea con $ (útil para ver saltos).
  - -s → Suprime líneas en blanco repetidas.
- `more` → Muestra el contenido de un archivo página por página.
- `less` → less es el visor de texto estándar en Linux. Permite desplazarse, buscar y moverse libremente sin cargar todo el archivo en memoria.
  - Navegación:
    - Barra espaciadora → Avanza una página.
    - b → Retrocede una página.
    - Enter → Avanza una línea.
    - ↓ → Desplazarse hacia abajo.
    - ↑ → Desplazarse hacia arriba.
    - /texto → Busca "texto" hacia adelante.
    - ?texto → Busca "texto" hacia atrás.
    - n → Repite la búsqueda en la misma dirección.
    - N → Repite la búsqueda en la dirección opuesta.
    - g → Va al principio del archivo.
    - G → Va al final del archivo.
    - q → Sale de less. 
- `head` → Muestra las primeras líneas de un archivo.
  - -n N → Muestra las primeras N líneas (por defecto 10). 
- `tail` → Muestra las últimas líneas de un archivo.
  - -n N → Muestra las últimas N líneas (por defecto 10).
  - -f → Sigue mostrando nuevas líneas en tiempo real (útil para logs). Significa "follow".
- `nl` → Numera las líneas de un archivo. Similar a `cat -n`, pero con más opciones de formato.
- `wc` → Cuenta líneas, palabras y bytes en un archivo.
  -  Salida: líneas, palabras, bytes y nombre del archivo.
  -  -l → Solo cuenta líneas.
  -  -w → Solo cuenta palabras.
  -  -c → Solo cuenta bytes.
- `strings` → Extrae cadenas de texto legibles de archivos binarios.

### Ejemplos prácticos

- Ver las primeras 20 líneas de un log:

  ```bash
  head -n 20 /var/log/syslog
  ```

- Seguir un log en tiempo real:
  
  ```bash
  tail -f /var/log/syslog
  ```

- Ver un fichero grande con paginación:
  
  ```bash
  less /var/log/syslog
  ```

- Contar líneas en un log:

  ```bash
  wc -l /var/log/syslog
  ```

## Filtros y combinaciones

Los comandos de visualización son aún más poderosos cuando se combinan con filtros como `grep`, `sort` o `uniq`. Estos permiten buscar, ordenar y filtrar la información mostrada.

### Concepto de filtro

Un filtro es un comando que lee datos de la entrada estándar (stdin), los procesa, y envía un resultado a la salida estándar (stdout). En Linux, los filtros se usan comúnmente en la línea de comandos para procesar texto.

### Ejemplos de filtros comunes

- `grep` → Busca texto en la entrada y muestra las líneas que coinciden.
  - -i → Ignora mayúsculas/minúsculas.
  - -v → Invierte la búsqueda (muestra líneas que no coinciden).
  - -r → Busca recursivamente en directorios.
  - -E → Usa expresiones regulares extendidas.
  - -c → Cuenta las líneas que coinciden.
  - -n → Muestra el número de línea junto a la coincidencia.
  - -H → Muestra el nombre del archivo junto a la coincidencia.
- `sort` → Ordena líneas de texto.
  - -n → Ordena numéricamente.
  - -r → Ordena en orden inverso.
  - -u → Ordena y elimina duplicados (único). 
- `uniq` → Elimina líneas duplicadas.
  - -c → Muestra cuántas veces aparece cada línea.
  - -d → Muestra solo las líneas duplicadas.
  - -u → Muestra solo líneas únicas (no duplicadas). 

### Ejemplos prácticos sin combinaciones

- Buscar errores en un log:

  ```bash
  grep "error" /var/log/syslog
  ```

- Contar ocurrencias de un error:

  ```bash
  grep -c "error" /var/log/syslog
  ```

- Ordenar un archivo alfabéticamente:

  ```bash
  sort /var/log/syslog
  ```

- Eliminar líneas duplicadas:

  ```bash
  uniq /var/log/syslog
  ```

- Excluir comentarios (líneas que empiezan con #):

  ```bash
  grep -vE "^#" /etc/passwd
  ```

- Excluir líneas vacías:

  ```bash
  grep -vE "^\s*$" /etc/passwd
  ```

- Excluir comentarios y líneas vacías:

  ```bash
  grep -vE "^\s*#|^\s*$" /etc/passwd
  ```

- Buscar archivos que contengan una palabra en un directorio y sus subdirectorios:

  ```bash
  grep "palabra" -nH /ruta/del/directorio
  ```

### Ejemplos prácticos con combinaciones: lectura + filtros

Para combinar comandos y filtros, usamos el operador pipe `|`, que redirige la salida de un comando (stdout) como entrada del siguiente (stdin).

- Ver las últimas 50 líneas de un log y filtrar errores:

  ```bash
  tail -n 50 /var/log/syslog | grep "error"
  ```

- Ver lineas que contengan una palabra y ordenarlas:

  ```bash
  cat /var/log/syslog | grep "warning" | sort
  ```

- Contar errores únicos en un log:

  ```bash
  cat /var/log/syslog | grep "error" | sort | uniq -c
  ```

- Ver lineas que concidadan con varias palabras (OR) + contar ocurrencias + ordenar alfabéticamente:

  ```bash
  cat /var/log/syslog | grep -E "error|fail|warning" | sort | uniq -c | sort
  ```

  <br><br>
  
# Rutas en Linux

Entender el concepto de rutas es fundamental para navegar y trabajar eficientemente en Linux. Una ruta define la ubicación exacta de un archivo o directorio dentro del sistema de archivos.

## Tipos de rutas

### Rutas absolutas

Una **ruta absoluta** especifica la ubicación completa desde el directorio raíz (`/`). Siempre comienzan con `/` y proporcionan la ubicación exacta sin importar desde dónde las ejecutes.

**Características:**
- Siempre empiezan con `/`
- Son inequívocas: siempre apuntan al mismo lugar
- Funcionan desde cualquier directorio de trabajo

**Ejemplos:**
```bash
/home/usuario/documentos/archivo.txt
/etc/passwd
/usr/bin/vim
/var/log/syslog
```

### Rutas relativas

Una **ruta relativa** especifica la ubicación en relación al directorio actual de trabajo. No comienzan con `/` y su resultado depende de tu ubicación actual.

**Características:**

- No empiezan con `/`
- Su resultado depende del directorio actual (`pwd`)
- Son más cortas pero pueden ser ambiguas

**Ejemplos:**

```bash
documentos/archivo.txt        # del directorio actual a un fichero en documentos/
../otracarpeta/archivo.txt    # subir un nivel, luego entrar en otracarpeta/
./script.sh                   # archivo en el directorio actual
```

## Símbolos especiales en rutas

### El punto (.)

- `.` → Representa el **directorio actual**
- `./archivo` → archivo en el directorio actual
- `./script.sh` → ejecutar script en directorio actual

### Dos puntos (..)

- `..` → Representa el **directorio padre** (un nivel arriba)
- `../archivo` → archivo en el directorio padre
- `../../archivo` → archivo dos niveles arriba
- `../hermano/archivo` → subir un nivel, entrar en carpeta hermano

### La virgulilla (~)

- `~` → Representa el **directorio home** del usuario actual
- `~/documentos` → equivale a `/home/usuario/documentos`
- `~usuario` → directorio home de otro usuario
- `~root` → directorio home del usuario root (`/root`)

### El guión (-)

- `-` → En `cd`, representa el **directorio anterior**
- `cd -` → vuelve al directorio donde estabas antes

## Consejos prácticos

### Autocompletado con TAB

- Pulsa `TAB` para autocompletar rutas
- Pulsa `TAB` dos veces para ver opciones disponibles

### Verificar tu ubicación

```bash
pwd                      # mostrar directorio actual
ls -la                   # ver contenido detallado del directorio actual
```

### Rutas con espacios

Cuando una ruta contiene espacios, debes escaparla:
```bash
cd "Mi Carpeta Con Espacios"
cd My\ Folder\ With\ Spaces
cd 'Otra Carpeta'
```

### Historial de directorios

```bash
cd -                     # volver al directorio anterior
dirs                     # ver pila de directorios (si usas pushd/popd)
```

## Variables de entorno relacionadas

- `$HOME` → ruta del directorio home del usuario actual
- `$PWD` → directorio actual de trabajo
- `$OLDPWD` → directorio anterior

```bash
echo $HOME               # mostrar tu directorio home
echo $PWD                # mostrar directorio actual
cd $OLDPWD              # ir al directorio anterior (como cd -)
```

<br><br>

# Manipulación del sistema del filesystem

Aprender a crear, copiar, mover, renombrar, enlazar, borrar y gestionar permisos/propietarios de archivos y directorios de forma segura y eficiente.

## Comandos básicos para crear.

- `mkdir` → Crea nuevos directorios.
  - `mkdir nuevo_directorio` → Crea un directorio.
  - `mkdir -p /ruta/completa/nuevo_directorio` → Crea directorios padres si no existen (parents).
  - `mkdir -v` → Muestra los directorios a medida que se crean (verbose).
- `touch` → Crea archivos vacíos o actualiza timestamps.
  - `touch archivo.txt` → Crea archivo.txt o actualiza su timestamp si ya existe.
  - `touch -c archivo.txt` → No crea el archivo si no existe (no clobber). Si el archivo ya existe, touch actualiza sus timestamps (como siempre). Si el archivo no existe, touch no lo crea. Simplemente no hace nada y no muestra error. 
  - `touch -a` → Actualiza solo el tiempo de acceso.
  - `touch -m` → Actualiza solo el tiempo de modificación.
  - `touch -d "YYYY-MM-DD HH:MM:SS" archivo.txt` → Establece una fecha y hora específicas.

**También puedes crear múltiples archivos a la vez:**

- `touch archivo1.txt archivo2.txt` → Crea archivo1.txt y archivo2.txt.
- `touch archivo{1..5}.txt` → Crea archivo1.txt, archivo2.txt, ..., archivo5.txt.

**También se pueden crear archivos directamente desde el editor de texto.**

- `nano archivo.txt` → Abre nano para crear/editar archivo.txt. Si no existe, lo crea.
- `vim archivo.txt` → Abre vim para crear/editar archivo.txt. Si no existe, lo crea.

## Comandos básicos para copiar, mover/renombrar y borrar.

- `cp` → Copia archivos y directorios.
  - `cp archivo1 archivo2` → Copia archivo1 a archivo2.
  - `cp -r dir1 dir2` → Copia el directorio dir1 y su contenido a dir2 (recursivo).
  - `cp -i` → Modo interactivo, pregunta antes de sobrescribir.
  - `cp -u` → Copia solo si el origen es más nuevo que el destino o si el destino no existe.
  - `cp -v` → Muestra los archivos a medida que se copian (verbose).
  - `cp -a` → modo archive: preserva atributos (permisos, timestamps, symlinks), equivalente a -dR --preserve=all.
  - `cp -n` → No sobrescribe archivos existentes.
- `mv` → Mueve o renombra archivos y directorios.
  - `mv archivo1 archivo2` → Renombra archivo1 a archivo2.
  - `mv dir1 dir2` → Mueve dir1 a dir2.
  - `mv -i` → Modo interactivo, pregunta antes de sobrescribir.
  - `mv -v` → Muestra los archivos a medida que se mueven (verbose).
- `rm` → Elimina archivos y directorios.
  - `rm archivo` → Elimina el archivo especificado.
  - `rm -r dir` → Elimina el directorio y su contenido (recursivo).
  - `rm -i` → Modo interactivo, pregunta antes de eliminar.
  - `rm -f` → Fuerza la eliminación sin preguntar (force).
  - `rm -v` → Muestra los archivos a medida que se eliminan (verbose).
- `rmdir` → Elimina directorios vacíos.
  - `rmdir directorio` → Elimina el directorio si está vacío.
  - `rmdir -p /ruta/completa/directorio` → Elimina el directorio y sus padres si están vacíos.

<br><br>

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

<br><br>

# Usuario y permisos

## Conceptos básicos

- **Usuario**: Entidad que interactúa con el sistema.
- **Grupo**: Conjunto de usuarios con permisos similares.
- **Permisos**: Controlan el acceso a archivos y recursos.
- **Propietario**: Usuario que posee un archivo o recurso.
- **Superusuario (root)**: Usuario con todos los permisos.
- **UID (User ID)**: Identificador numérico único para cada usuario.
- **GID (Group ID)**: Identificador numérico único para cada grupo.

## Tipos de usuarios en Linux

- **Usuario root**: El administrador del sistema con todos los privilegios. Tiene UID 0.
- **Usuarios del sistema**: Usuarios creados para ejecutar servicios (sin acceso de login). Por ejemplo, el usuario `nginx` para el servidor web Nginx. Su UID suele estar entre el 1 y el 999.
- **Usuarios normales**: Usuarios con permisos limitados. Creados para los usuarios finales, su UID suele empezar desde 1000 en adelante.

## Se puede ver la lista de usuarios en el sistema en el fichero `/etc/passwd`

```bash
cat /etc/passwd
```

Cada línea representa un usuario y tiene el siguiente formato:

```
nombre_usuario:x:UID:GID:comentario:directorio_home:shell
```

Donde cada campo está separado por dos puntos (`:`) y significa:

1. **nombre_usuario**: El nombre de login del usuario
2. **x**: Placeholder de la contraseña (las contraseñas están en `/etc/shadow`)
3. **UID**: User ID - identificador numérico único del usuario
4. **GID**: Group ID - identificador del grupo principal del usuario
5. **comentario**: Campo GECOS (información adicional como nombre completo, teléfono, etc.)
6. **directorio_home**: Ruta del directorio personal del usuario
7. **shell**: Shell por defecto que usa el usuario

### Ejemplos reales:

```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
usuario:x:1000:1000:Usuario Normal,,,:/home/usuario:/bin/bash
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
```

### Explicación de los ejemplos:

- **root**: UID 0, directorio `/root`, shell `/bin/bash`
- **daemon**: Usuario del sistema (UID 1), sin shell de login
- **usuario**: Usuario normal (UID 1000), con directorio home y shell bash
- **www-data**: Usuario para el servidor web (UID 33), sin shell de login

Los usuarios del sistema suelen tener `/usr/sbin/nologin` o `/bin/false` como shell para prevenir login directo.

## El fichero `etc/shadow` contiene las contraseñas encriptadas y políticas de expiración.

```bash
cat /etc/shadow
```

Cada línea tiene el siguiente formato:

```
nombre_usuario:contraseña_encriptada:último_cambio:mín_días:máx_días:aviso_días:inactivo_días:expiración:reservado
```

Donde cada campo significa:

1. **nombre_usuario**: El nombre de login del usuario
2. **contraseña_encriptada**: Contraseña encriptada (o `*` si no tiene contraseña)
3. **último_cambio**: Días desde 1970 cuando se cambió la contraseña por última vez
4. **mín_días**: Mínimo número de días entre cambios de contraseña
5. **máx_días**: Máximo número de días que una contraseña es válida
6. **aviso_días**: Días antes de la expiración para avisar al usuario
7. **inactivo_días**: Días después de la expiración que la cuenta se desactiva
8. **expiración**: Días desde 1970 cuando la cuenta expira
9. **reservado**: Campo reservado para uso futuro

### Ejemplo real:

```
usuario:$6$randomsalt$hashedpassword:18500:0:99999:7:::
```

- **usuario**: Nombre del usuario
- **$6$randomsalt$hashedpassword**: Contraseña encriptada con SHA-512
- **18500**: Último cambio de contraseña (día 18500 desde 1970)
- **0**: No hay mínimo de días entre cambios
- **99999**: La contraseña nunca expira
- **7**: Aviso 7 días antes de la expiración
- Los campos de inactividad, expiración y reservado están vacíos.

## Comandos para gestionar usuarios, grupos y permisos

### Gestión de usuarios

```bash
# Crear un nuevo usuario con directorio home (-m) y bash como shell (-s)
sudo useradd -m -s /bin/bash nuevo_usuario

# Eliminar un usuario y su directorio home (-r)
sudo userdel -r usuario_a_eliminar

# Cambiar la contraseña de un usuario
sudo passwd usuario_a_cambiar

# Listar todos los usuarios
cat /etc/passwd

# Ver información detallada de un usuario
id nombre_usuario

# Modificar un usuario (cambiar shell, home, etc.)
sudo usermod -s /bin/zsh -d /home/nuevo_home nombre_usuario

# Bloquear un usuario (impide login)
sudo usermod -L nombre_usuario

# Desbloquear un usuario
sudo usermod -U nombre_usuario

# Forzar cambio de contraseña en el próximo login
sudo chage -d 0 nombre_usuario

# Añadir a grupo sudo
sudo usermod -aG sudo usuario
```

### Gestión de grupos

```bash
# Crear un nuevo grupo
sudo groupadd nuevo_grupo

# Eliminar un grupo
sudo groupdel grupo_a_eliminar

# Listar todos los grupos
cat /etc/group

# Ver información detallada de un grupo
getent group nombre_grupo

# Añadir un usuario a un grupo
sudo usermod -aG nombre_grupo nombre_usuario

# Eliminar un usuario de un grupo
sudo gpasswd -d nombre_usuario nombre_grupo

# Cambiar el grupo principal de un usuario
sudo usermod -g nuevo_grupo nombre_usuario
```

### Gestión de permisos

Los permisos en Linux se representan con tres conjuntos de bits: para el propietario, el grupo y otros usuarios. Cada conjunto tiene permisos de lectura (r), escritura (w) y ejecución (x).

Si empieza con un `d`, es un directorio; si empieza con un `-`, es un archivo regular.

```bash
# Ver permisos y propietarios de archivos/directorios
ls -l
```

Formato de salida:

```
-rwxr-xr-- 1 usuario grupo 1234 Mar 10 12:34 archivo.txt
```

- `-` : tipo de archivo (archivo regular)
- `rwx` (propietario): lectura, escritura, ejecución
- `r-x` (grupo): lectura, ejecución
- `r--` (otros): solo lectura
- `1` : número de enlaces
- `usuario` : propietario del archivo
- `grupo` : grupo asociado al archivo
- `1234` : tamaño en bytes
- `Mar 10 12:34` : fecha y hora de la última modificación
- `archivo.txt` : nombre del archivo

```bash
# Cambiar permisos (modo simbólico)
sudo chmod u+rwx,g+rx,o+r archivo.txt  # Añadir permisos
sudo chmod u-w,g-w,o-w archivo.txt      # Quitar permisos
sudo chmod u=rwx,g=rx,o=r archivo.txt    # Establecer permisos exactos
sudo chmod +x script.sh                  # Hacer ejecutable
```

```bash
# Cambiar permisos (modo numérico)
sudo chmod 755 archivo.txt  # rwxr-xr-x
sudo chmod 644 archivo.txt  # rw-r--r--
sudo chmod 700 archivo.txt  # rwx------
```

```bash
# Cambiar propietario y grupo
sudo chown nuevo_propietario archivo.txt          # Cambiar propietario
sudo chown :nuevo_grupo archivo.txt               # Cambiar grupo
sudo chown nuevo_propietario:nuevo_grupo archivo.txt  # Cambiar ambos
```

```bash
# Cambiar propietario y grupo recursivamente en un directorio
sudo chown -R nuevo_propietario:nuevo_grupo /ruta/del/directorio
```

### Permisos especiales

```bash
# SUID: Permite a un usuario ejecutar un archivo con los permisos del propietario
sudo chmod u+s /ruta/al/archivo

# SGID: Permite a un usuario ejecutar un archivo con los permisos del grupo
sudo chmod g+s /ruta/al/archivo

# Sticky Bit: Solo el propietario puede eliminar o renombrar archivos en un directorio
sudo chmod +t /ruta/al/directorio
```

### Como identificar si un archivo tiene permisos especiales

```bash
ls -l
```

- Si ves una `s` en lugar de `x` en los permisos del propietario (u) o grupo (g), indica SUID o SGID respectivamente.

- Si ves una `t` en lugar de `x` en los permisos de otros (o), indica que el sticky bit está activado.

### Ver usuarios conectados

```bash
who               # Muestra usuarios conectados
w                 # Muestra usuarios conectados y su actividad
last              # Muestra historial de logins
```