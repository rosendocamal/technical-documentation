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
$ sudo dnf install VirtualBox-7.2
```

## INSTALAR EXTENSIONES DE VIRTUALBOX

Verificamos la versión instalada de VirtualBox.

``` 
$ vboxmanage -v | cut -dr -f1
```

Con el comando anterior es posible saber que versión descargar y continuamos con ello.

```
$ wget https://download.virtualbox.org/virtualbox/7.2.4/Oracle_VirtualBox_Extension_Pack-7.2.4.vbox-extpack
$ sudo vboxmanage extpack install Oracle_VirtualBox_Extension_Pack-7.2.4.vbox-extpack
```

## AGREGAR USUARIO AL GRUPO *VBOXUSERS*

Añadimos nuestro usuario actual al grupo **vboxusers** con el siguiente comando:

```
$ sudo usermod -a -G vboxusers $USER
```

## DESINSTALAR VIRTUALBOX

Para desinstalar basta con ejecutar:

```
$ sudo dnf remove VirtualBox-7.2 -y
```
