# MIS PRIMEROS PASOS CON FEDORA

| Autor         | Fecha      | Actualización |
|---------------|------------|---------------|
| Rosendo Camal | 11/12/2025 | 12/01/2026    |

## Actualización de Fedora

Antes de configurar Fedora actualicé todo:

```
$ sudo dnf upgrade --refresh -y
```

Esto evitará conflictos de paquetes y errores al instalar software.

> Nota: `dnf upgrade` ya hace el trabajo de `update` y `clean all` no es estrictamente necesario al inicio.

## Instalación de programas

Una vez hecho lo anterior paso a instalar mis programas de uso con el comando `sudo dnf install [program] -y`.

En mi caso los programas que instalaré son `ranger`, `neovim`, `git`, `chromium` y `tmux`.

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

Debido a que mi dispositivo es una Laptop Lenovo, **quise** recuperar varias opciones que me brindaba la aplicación de Lenovo Vantage y el límite de carga es uno de ellos.

> **IMPORTANTE:** Para usar TLP en Fedora sin conflictos, debemos desactivar el gestor de energía por defecto de GNOME.

```
# 1. Deshabilitar el gestor de nativo para evitar conflictos
$ sudo systemctl mask power-profiles-daemon

# 2. Habilitar TLP
$ sudo systemctl enable tlp --now
```

Verifico el estado de servicio:

```
$ tlp-stat -s
```

Si está activo, configuro el archivo en la ruta `/etc/tlp.conf`:

```
$ sudo nvim /etc/tlp.conf
```

Busco y descomento las líneas: `START_CHARGE_THRESH_BAT0=75` y `STOP_CHARGE_THRESH_BAT0=80`.

Para aplicar los cambios automáticamente de dicho archivo, ejecutamos:

```
$ sudo tlp start
```
## Otros pasos adicionales

### Modo de energía inteligente

> *Nota: Solo usar si NO se instaló TLP. Si usas TLP, omite este paso.*

Instalamos si no tenemos el gestor de energía:

```
$ sudo dnf install power-profiles-daemon -y

# Habilitación del gestor de energía
$ systemctl status power-profiles-daemon
```

Para cambiar el modo de energía en terminal: 

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

$ sudo powertop --auto-tune
```

Esto aplica optimizaciones en tiempo real. Se puede hacer al principio, al iniciar si se quiere máximo ahorro.

Para la temperatura y el ventilador:

```
$ sudo dnf install lm_sensors hddtemp -y
$ sudo sensors-detect
$ sensors
```

Permite vigilar la CPU y la batería, especialmente si se va a utilizar el modo rendimiento.

### Manejo de CPU

Esto permitirá cambiar la frecuencia de la CPU y el gobernador:

```
$ sudo dnf install kernel-tools -y
$ sudo cpupower frequency-info

# powersave reduce la temperatura y consumo
$ sudo cpupower frequency-set -g powersave

# performance maximiza el rendimiento cuando se necesite
$ sudo cpupower frequency-set -g performance
```

### Optimización de almacenamiento

```
# Verificar TRIM automático
$ sudo systemctl status fstrim.timer

# Limpieza de archivos
$ sudo dnf autoremove
$ sudo journalctl --vacuum-time=7d
```

### Seguridad y actualizaciones automáticas

Para activar las actualizaciones automáticas de seguridad se puede emplear esto:

```
$ sudo dnf install dnf-automatic -y
$ sudo systemctl enable --now dnf-automatic.timer
```

Para un firewall activo:

```
# Firewall (Firewalld es el estándar en Fedora)
$ sudo systemctl enable --now firewalld
$ sudo firewall-cmd --state
```

Aunque en Fedora está SELinux activo (ya viene por defecto) y esto protege el sistema sin nuestra intervención.

Para cifrado de archivos sensibles:

```
# Cifrar un archivo con contraseña
gpg -c file_name
```

### Timeshift: para snapshots del sistema

Perfecto antes de grandes actualizaciones o cambios de configuración.

```
$ sudo dnf install timeshift -y
```

### Auditoría y Logs

Para revisión de logs en Fedora se emplea:

```
# Ver logs de errores recientes
$ journalctl -p 3 -xb

# Ver actividad de sudo
$ sudo journalctl -t sudo
```

Herramientas recomendadas: `fail2ban` y `rkhunter`.

## Nota final

Estos son los comandos que requiero al momento de una instalación o reinstalación de Linux Fedora.
