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
