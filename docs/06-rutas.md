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
