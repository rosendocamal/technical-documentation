# MIS PRIMEROS PASOS CON FEDORA

| Autor         | Fecha      |
|---------------|------------|
| Rosendo Camal | 11/12/2025 |

## Actualización de Fedora

Antes de configurar Fedora actualicé todo:

```
$ sudo dnf upgrade --refresh -y
```

```
$ sudo dnf clean all
```

```
$ sudo dnf update -y
```

Esto evitará conflictos de paquetes y errores al instalar software.

## Instalación de programas

Una vez hecho lo anterior paso a instalar mis programas de uso con el comando `sudo dnf install program -y`.

> Sustituye `program` por el programa a instalar.

En mi caso los programas que instalaré es `ranger`, `neovim`, `git`, `chromium` y `tmux`.

```
$ sudo dnf install ranger neovim git chromium tmux -y
```

También instalaré `tlp` y `tlp-rdw` para la gestión de batería.

```
$ sudo dnf install tlp tlp-rdw -y
``` 

Para las actualización de firmware (si en dado caso no está instalado):

```
$ sudo dnf install fwupd -y
```

También instalo a través de la tienda de aplicaciones otros programas o siguiendo las guías oficiales de los respectivos programas como el caso de Visual Studio Code.

## Límite de carga al 80%

Debido a que mi dispositivo es una Laptop Lenovo y tenía configuraciones que perdí al pasarme de Windows 11 a Linux Fedora. Quizé recuperar varias opciones que me brindaba la aplicación de Lenovo Vantage y el límite de carga es uno de ellos. Gracias a que Fedora sí lo soporta vía ACPI.

Lo activo así:

```
$ sudo systemctl enable tlp --now
```

Verifico el estado de servicio:

```
$ tlp-stat -s
```

Si está activo, continuó configurando un archivo en la ruta `/etc/tlp.conf` ya sea con nvim, vim o nano:

```
$ sudo nvim /etc/tlp.conf
```

Y busco las líneas comentadas `START_CHARGE_THRESH_BAT0=75` y `STOP_CHARGE_THRESH_BAT0=80`, las descomento y guardo el archivo.

En `/etc/tlp.conf` también se puede configurar otros ajustes adicionales que ahora no están a mi alcance.

## Otros pasos adicionales

Son pasos que en algún momento intenté, pero al menos Gnome trae las opciones fáciles de emplear.

### Modo de energía inteligente

Instala:

```
$ sudo dnf isntall power-profiles-daemon -y
```

Con esto Fedora me da los modos de Ahorro, Balanceado y Rendimiento.

Verificamos que esté habilitado:

```
$ systemctl status power-profiles-daemon
```

Para cambiar mode de energía: 

``` 
# Modo ahorro de energía
$ sudo powerprofilesctl set powersave

# Modo balanceado
$ sudo powerprofilesctl set balanced

# Modo rendimiento
$ sudo powerprofilesctl set performance
```

### Diagnóstico y ajuste de consumo

Instala `powertop` y habilitálo:

```
$ sudo dnf install powertop -y

$ sudo powertop

$ sudo powertop --auto-tune
```

Esto aplica optimizaciones en tiempo real. Se puede hacer al principio, al iniciar si se quiere máximo ahorro.

Para la temperatura y el ventilador:

```
$ sudo dnf install lm_sensors hddtemp -y

$ sudo sensors-dectect

$ sensors
```

Permite vigilar la CPU y la batería, especialmente si se va a utilizar el modo rendimiento.

### Manejo de CPU

Esto permitirá cambiar la frecuencia de la CPU y el gobernador:

```
$ sudo dnf isntall kernel-tools -y

$ sudo cpupower frequency-info

# powersave reduce la temperatura y consumo
$ sudo cpupower frequency-set -g powersave

# performance maximiza el rendimiento cuando se necesite
$ sudo cpupower frequency-set -g performance
```

### Hibernación y suspensión optimizada

Se verifica el soporte vía:

```
$ cat /sys/power/state
```

Para la suspensión:

```
$ systemctl suspend
```

Para la hibernación (si lo permite la BIOS):

```
$ systemctl hibernate
```

### Optimización de almacenamiento

Para verificar si se tiene el TRIM automático:

```
$ sudo systemctl status fstrim.timer
```

Para limpiar archivos temporales:

```
$ sudo dnf autoremove

$ sudo dnf clean all
```

Si se desea una limpieza periódica de caché y logs:

```
$ sudo dnf clean all

$ sudo journalctl --vacuum-time=7d
```

Se puede aumentar el tamaño de la caché de disco para acelerar lectura/escritura en SSD:

```
$ sudo sysctl vm.swappiness=10

$ sudo sysctl vm.vfs_cache_pressure=50
```

### Seguridad y actualizaciones automáticas

Para activar las actualizaciones automáticas de seguridad se puede emplear esto:

```
$ sudo dnf install dnf-automatic -y

$ sudo systemctl enable --now dnf-automatic.timer
```

Para un firewall activo:

```
$ sudo systemctl enable-now firewalld

$ sudo firewall-cmd --state
```

Aunque en Fedora está SELinux activo (ya viene por defecto) y esto protege el sistema sin nuestra intervención.

También para el firewall se puede emplear:

```
$ sudo ufw enable

$ sudo ufw status
```

Para cifrado de archivos sensibles se puede emplear Veracrypt para volumenes o para cifrado de archivos lo siguiente:

```
gcp -c file
```

### Timeshift: para snapshots del sistema

Perfecto antes de grandes actualizaciones o cambios de configuración.

```
$ sudo dnf install timeshift -y
```

### Auditoría

Para revisión de logs se puede emplear `journactl -xe`, indagar en la actividad de sudo `sudo cat /var/log/auth.log`, y existen herramientas de análisis como `fail2ban` para prevenir ataques SSH, firewalls como `ufw`, otras herramientas para intrusión y malware son: `failban` y `rkhunter`.

## Nota final

Estos son los comandos o aspectos técnicos que requiero al momento de una instalación o reinstalación de Linux Fedora. Hay más cosas que realizo pero no son tan técnica
