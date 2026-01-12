# INSTALACIÓN DE VIRTUAL BOX EN FEDORA 43

| Autor | Fecha | Actualización |
|-------|-------|------ |
| Rosendo Camal | 17/12/2025 | 12/01/2026 |

## OBJETIVO

Instalar en Fedora 43 el programa de virtualización VirtualBox.

## INSTALAR DEPENDENCIAS NECESARIAS

Instalamos el kernel headers y otros paquetes esenciales.

```
$ sudo dnf install @development-tools
$ sudo dnf install kernel-headers kernel-devel dkms
```

## AÑADIR REPOSITORIO DE VIRTUALBOX

Añadimos el repositorio oficial de VirtualBox a nuestro sistema.

```
$ sudo nvim /etc/yum.repos.d/virtualbox.repo
```
Y en el archivo anterior copiamos y guardamos lo siguiente:

``` 
[virtualbox]
name=Fedora $releasever - $basearch - VirtualBox
baseurl=http://download.virtualbox.org/virtualbox/rpm/fedora/$releasever/$basearch
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://www.virtualbox.org/download/oracle_vbox_2016.asc
```

## INSTALAR VIRTUALBOX

Primero actualizamos el sistema y posteriormente instalamos VirtualBox.

```
$ sudo dnf update
$ sudo dnf install VirtualBox-7.2 # O la versión específica actual en el repo
```

## INSTALAR EXTENSIONES DE VIRTUALBOX

Verificamos la versión instalada de VirtualBox.

``` 
$ vboxmanage -v | cut -d'r' -f1
```

Con el comando anterior es posible saber que versión descargar y continuamos con ello.

```
# 1. Obtenemos la versión instalada y la guardamos en una variable
$ VBOX_VER=$(vboxmanage -v | cut -d'r' -f1)

# 2. Descargamos la versión exacta
$ wget [https://download.virtualbox.org/virtualbox/$VBOX_VER/Oracle_VirtualBox_Extension_Pack-$VBOX_VER.vbox-extpack](https://download.virtualbox.org/virtualbox/$VBOX_VER/Oracle_VirtualBox_Extension_Pack-$VBOX_VER.vbox-extpack)

# 3. Instalamos
$ sudo vboxmanage extpack install Oracle_VirtualBox_Extension_Pack-$VBOX_VER.vbox-extpack
```

## AGREGAR USUARIO AL GRUPO *VBOXUSERS*

Añadimos nuestro usuario actual al grupo **vboxusers** con el siguiente comando:

```
$ sudo usermod -a -G vboxusers $USER
```

> **IMPORTANTE:** Debemos cerrar sesión y volver a entrar (o reiniciar) para que este cambio tenga efecto.

## NOTA SOBRE KERNEL Y MÓDULOS

### SECURE BOOT DESACTIVADO

Al tener el **Secure Boot desactivado**, si después de una actualización de Fedora el programa no abre, simplemente ejecutamos el siguiente comando para reconstruir los controladores :

```
$ sudo /sbin/vboxconfig
```

### SECURE BOOT ACTIVADO (FIRMA DE MÓDULOS)

Si el Secure Boot está activado en la BIOS, el Kernel de Fedora bloqueará los controladores de VirtualBox por no tener una firma digital confiable. Para solucionarlo, debemos crear nuestra propia llave de firma e importarla al sistema.

**1. Generar la llave de firma (MOK - Machine Owner Key):** Crea una carpeta para tus llaves y genera el certificado. Este paso solo se hace la primera vez.

```
$ sudo mkdir -p /root/module-signing
$ sudo openssl req -new -x509 -newkey rsa:2048 -keyout /root/module-signing/MOK.priv -outform DER -out /root/module-signing/MOK.der -nodes -days 36500 -subj "/CN=VirtualBox/"
```

**2. Importar la llave a la BIOS:** Debes "decirle" a tu computadora que confíe en esta nueva llave. Te pedirá una contraseña: elígela y anótala, la necesitarás al reiniciar.

```
$ sudo mokutil --import /root/module-signing/MOK.der
```

**3. Reiniciar y Enrolar:** (Paso Crítico). Reinicia tu computadora. Antes de iniciar Fedora, aparecerá una pantalla azul/negra llamada Shim UEFI Key Management:

- Selecciona Enroll MOK.
- Selecciona View key 0 (opcional, para confirmar que es la tuya).
- Selecciona Continue.
- Selecciona Yes.
- Introduce la contraseña que creaste en el paso anterior.
- Selecciona Reboot.

**4. Firmar los módulos de VirtualBox:** Ahora que el sistema confía en tu llave, firma los controladores instalados. Este comando debe ejecutarse después de instalar VirtualBox o tras una actualización mayor:

```
$ sudo /usr/src/kernels/$(uname -r)/scripts/sign-file sha256 /root/module-signing/MOK.priv /root/module-signing/MOK.der $(modinfo -n vboxdrv)
```

**5. Cargar el módulo:** Finalmente, carga el controlador firmado:

```
$ sudo modprobe vboxdrv
```

**¿Cuándo repetir esto?**

- El paso 1, 2 y 3: Solo se hacen una vez en la vida de tu instalación actual.
- El paso 4: Solo se repite si actualizas el Kernel de Fedora y VirtualBox deja de abrir.

## DESINSTALAR VIRTUALBOX

Para desinstalar basta con ejecutar:

```
# Opción genérica (borra cualquier versión de VB instalada)
$ sudo dnf remove VirtualBox* -y
```
