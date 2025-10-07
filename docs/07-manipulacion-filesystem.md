# Manipulación del sistema del filesystem

Aprender a crear, copiar, mover, renombrar, enlazar, borrar y gestionar permisos/propietarios de archivos y directorios de forma segura y eficiente.

## Comandos básicos para crear

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

## Comandos básicos para copiar, mover/renombrar y borrar

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
