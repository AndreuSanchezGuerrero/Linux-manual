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

## Ejemplos prácticos

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
