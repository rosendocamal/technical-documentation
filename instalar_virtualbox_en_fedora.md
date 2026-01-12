# INSTALACIÓN DE VIRTUAL BOX EN FEDORA 43

| Autor | Fecha |
|-------|-------|
| Rosendo Camal | 17/12/2025 |

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

Al tener el **Secure Boot desactivado**, si después de una actualización de Fedora el programa no abre, simplemente ejecutamos el siguiente comando para reconstruir los controladores :

```
$ sudo /sbin/vboxconfig
```

## DESINSTALAR VIRTUALBOX

Para desinstalar basta con ejecutar:

```
# Opción genérica (borra cualquier versión de VB instalada)
$ sudo dnf remove VirtualBox* -y
```
