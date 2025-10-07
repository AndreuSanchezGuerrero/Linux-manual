# Usuarios y permisos

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

## El fichero `/etc/shadow` contiene las contraseñas encriptadas y políticas de expiración.

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
