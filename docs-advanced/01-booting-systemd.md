# Booting y Systemd

## Conceptos

- **Booting**: Proceso de arranque del sistema operativo.
- **Kernel**: Núcleo del sistema operativo que gestiona el hardware y los recursos del sistema.
- **Systemd**: Sistema de inicio y gestor de servicios utilizado en muchas distribuciones modernas de Linux.
- **Init**: Primer proceso que se ejecuta en el sistema, responsable de iniciar otros procesos.
- **Runlevel/Target**: Estados del sistema que definen qué servicios y procesos deben estar activos.
- **Service**: Unidades gestionadas por systemd que representan servicios del sistema.
- **BIOS/UEFI**: Firmware que inicializa el hardware y carga el bootloader.
- **Bootloader**: Programa que carga el kernel del sistema operativo en memoria.

## Proceso de arranque en Linux

El proceso de arranque (booting) en Linux es el conjunto de pasos que sigue el sistema desde que se enciende el ordenador hasta que el usuario puede interactuar con él. Este proceso incluye varias etapas clave:

1. **BIOS/UEFI**: Al encender el ordenador, el firmware (BIOS o UEFI) realiza una serie de pruebas de hardware (POST) y busca un dispositivo de arranque (disco duro, USB, etc.).

2. **Bootloader**: Una vez que el firmware encuentra un dispositivo de arranque, carga el bootloader (como GRUB) en memoria. El bootloader **muestra un menú** que permite seleccionar el sistema operativo a iniciar. Al seleccionar Linux, carga el kernel y el initramfs (sistema de archivos inicial) en memoria.

3. **Kernel**: El kernel de Linux se carga en memoria y comienza a inicializar el hardware del sistema. Esto incluye la detección de dispositivos, la configuración de controladores y la preparación del entorno para los procesos de usuario.

4. **Systemd**: Una vez que el kernel ha inicializado el hardware, se inicia el sistema de inicio (init system), que en la mayoría de las distribuciones modernas de Linux es systemd. Systemd se encarga de iniciar y gestionar los servicios del sistema, así como de establecer el entorno de usuario.

5. **Servicios y sesiones de usuario**: Systemd inicia los servicios necesarios (como redes, servidores, etc.) y finalmente lanza el gestor de sesiones (como GDM, LightDM) para que el usuario pueda iniciar sesión.

## BIOS vs UEFI

- **BIOS (Basic Input/Output System)**:
  - Usado hasta hace una década (LEGACY).
  - Basado en MBR (Master Boot Record):
    - Primer sector del disco (512 bytes) con bootloader primario.
    - Contiene tabla de particiones muy limitada (máx. 4 particiones primarias).
    - El bootloader en MBR es muy pequeño → carga un segundo bootloader (ej. GRUB).
    - Limitaciones: Máx. 2 TB de disco.
    - Muy dependiente de la ubicación fija en disco.

> Como sysadmin: aún lo verás en sistemas antiguos y máquinas virtuales. Conocerlo sirve para rescatar instalaciones viejas.

- **UEFI (Unified Extensible Firmware Interface)**:
  - Estándar moderno que reemplaza BIOS.
  - Basado en GPT (GUID Partition Table):
    - Permite discos de hasta 9.4 zettabytes.
    - Soporta hasta 128 particiones por defecto.
    - Usa partición especial: ESP (EFI System Partition) con formato FAT32.
    - El bootloader se almacena como archivo .efi en ESP (ej. `/EFI/ubuntu/grubx64.efi`).
  - Ventajas adicionales:
    - Puede arrancar directamente un kernel compilado para UEFI (sin bootloader intermedio).
    - Tiene API formal → UEFI actúa como un mini SO con controladores y utilidades propias.
    - Soporta Secure Boot (arranque seguro con firma digital).
    - Interfaz más moderna (gráfica, soporte ratón).
    - Arranque más rápido que BIOS.
    - Mejor compatibilidad con hardware moderno (más de 2TB).

> Como sysadmin: es el estándar actual. Fundamental para gestionar hardware moderno y grandes discos.
