# El comando MAN

El comando man (manual) es la herramienta esencial para consultar la documentación de cualquier comando en Linux. No se trata solo de mostrar ayuda: aprender a leer e interpretar correctamente las páginas del manual es una habilidad clave para todo administrador de sistemas.

## La sintaxis básica es:

```bash
man [sección] comando
```

- [sección] → opcional, indica la sección del manual donde buscar.
- comando → el nombre del comando, función o fichero que queremos consultar.

## Estructura de una página man:

Aunque puede variar, normalmente veremos:

- NAME → nombre del comando y breve descripción.
- SYNOPSIS → aquí se encuentra la sintaxis del comando. Es la parte más importante y, al principio, la más confusa.
- DESCRIPTION → explicación detallada de las opciones.
- OPTIONS → listado de parámetros que se pueden usar con el comando.
- EXAMPLES → ejemplos prácticos (no todos los comandos los incluyen).
- SEE ALSO → otros comandos relacionados.

## Cómo leer la sintaxis en SYNOPSIS:

- [] → los corchetes indican que el elemento es opcional.
- < > → los ángulos muestran un valor que debes sustituir, como <file> o <user>.
- | → significa "o", debes elegir una de las opciones.
- ... → indica que el elemento puede repetirse varias veces.
- { } → agrupan opciones que van juntas.

## Ejemplo con el comando man ssh

![alt text](../image.png)

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

## Secciones de man

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

## Dónde se almacenan y cómo se buscan las man pages

En Linux, las páginas del manual se distribuyen por secciones (1, 2, 3, …) en rutas conocidas. 

Lo más habitual:

- /usr/share/man/ → man pages instaladas por el sistema.
- /usr/local/share/man/ → man pages instaladas localmente por el administrador (prioridad sobre /usr).
- /opt/<paquete>/share/man → algunos paquetes autoinstalados guardan ahí su documentación.
- /var/cache/man/ → cat pages (páginas preformateadas en caché) e índices.

## La variable MANPATH (y el fichero de configuración de man) dictan en qué árboles buscar y en qué orden.

Ver tu ruta efectiva:

```bash
manpath
```

## Inspeccionar dónde está una página concreta:

```bash
man -w ssh     # primera encontrada
man -aw ssh    # muestra todas las rutas donde existe
```

---

# Crear tus propias man pages

## Estructura básica del archivo

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

## Ubicación

- /usr/local/share/man/man1/

## Nombre del archivo

- Nombre: comando.sección → ej: mi-tool.1

## Comprimir la página

```bash
gzip -9 /usr/local/share/man/man1/mi-tool.1
```

Esto genera: /usr/local/share/man/man1/mi-tool.1.gz

## Actualizar la base de datos de man

Para que man reconozca tu página:

```bash
sudo mandb -q /usr/local/share/man
```

## Probar tu man page

```bash
man mi-tool
```

## Y si quieres ver todas las ubicaciones donde se encuentra:

```bash
man -aw mi-tool
```

---

> **Lectura recomendada**: 
> [Linux man page guide - It's FOSS](https://itsfoss.com/linux-man-page-guide)

---
