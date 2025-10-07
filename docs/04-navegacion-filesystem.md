# Navegación por el filesystem

## Entendiendo la jerarquía de directorios

La jerarquía de directorios en Linux se basa en un filesystem en forma de árbol. Aquí hay algunos de los directorios más importantes que encontrarás:

- `/` : El directorio raíz, donde empieza la jerarquía de archivos.
- `/bin` : Ejecutables esenciales para el sistema.
- `/sbin` : Ejecutables del sistema, generalmente para administración.
- `/usr` : Históricamente: "Unix System Resources", no "user". Es la jerarquía principal de software del sistema (binarios, librerías, documentación...). Por ejemplo, `/usr/bin` a diferencia de `/bin`, contiene aplicaciones y utilidades adicionales no esenciales para el arranque.
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
