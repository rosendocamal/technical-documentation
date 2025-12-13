# CONFIGURAR GIT Y CONECTAR CON GITHUB: PASOS BÁSICOS

---

| Nombre | Fecha |
|--------|-------|
| Rosendo Camal | 12/12/2025 |

## Objetivo

Configurar **Git** de manera básica para poder conectarse rápidamente a **GitHub** y empezar a trabajar local y poder enviar nuestros cambios del repositorio al servidor. Esto lo haremos en nuestra computadora con Fedora.

## Configuración de Git

Para ello introducimos a Git un usuario que será prácticamente la firma de los cambios, modificaciones o cualquier acción que se realice con dicha herramienta. Se suele ingresar el usuario y el correo electrónico al cual tenemos vinculado con GitHub.

```
# Sustituye "Your name" por tu nombre dentro de comillas dobles.
$ git config --global user.name "Your name"

# Sustituye mail@domain.com por tu dirección de correo electrónico.
$ git config --global user.email mail@domain.com
```

Para ver que dichos cambios hayan tenido efectos o lo que estemos modificando utilizamos el siguiente comando:

```
$ git config --list
```

Con el comando anterior, verificamos los cambios hechos que hemos realizado actualmente con `git config --global user.name` y `git config --global user.email`.

## Conexión a GitHub

### Conexión a GitHub con SSH

Comprobamos las **claves SSH** que tengamos con el siguiente comando:

``` 
$ ls -al ~/.ssh
```

Nos va a listar las claves SSH con las que contamos y en caso contrario, nos dará de error y eso indica que no tenemos ninguna clave creada.

Creamos una clave SSH indicando nuestro correo utilizado anteriormente con Git (debe ser igual al vinculado con GitHub).

```
# Puedes cambiar la parte de "git_workshop" por el de tu preferencia.
$ ssh-keygen -t ed25519 -f ~/.ssh/git_workshop -C "mail@domain.com"
```

> También se puede crear la clave SSH de manera más rápida pero sin tanta personalización al usar únicamente `ssh-keygen`.

Necesitamos comprobar que **ssh-agent** esté funcionando ya que esté nos permite iniciar sesión en otros servidores sin que nosotros introducazmos de nuevo una contraseña.

``` 
$ eval "$(ssh-agent -s)"
```

Y añadimos la clave creada a ssh-agent:

```
# Recuerda intercambiar "git_workshop" por el nombre que le hayas puesto al crear la clave SSH.
$ ssh-add ~/.ssh/git_workshop
```

Luego inicia sesión en GitHub, ve a **Settings** y de ahí a **SSH and GPG Keys**. Crea una nueva clave SSH, pónle un nombre descriptivo (sección *Title*) para identificar desde que equipo te pertenece y se usa la clave, y en el recuadro indicado (sección *Key*) se pega el contenido del archivo **git_workshop.pub** ubicado en la ruta `~/.ssh/git_workshop.pub`.

Lo podemos hacer de esta manera, para ello nos cerciarmos o instalamos la utilería `xclip`. Si no está instalado procedemos a ejecutar `sudo dnf install xclip -y` y posteriormente:

```
# Recuerda cambiar "git_workshop" por el que hayas indicado anteriormente.
$ cat ~/.ssh/git_workshop.pub | xclip -sel clip
```

Lo anterior hace lo siguiente:
* La primera parte `cat ~/.ssh/git_workshop.pub` accede y permite visualizar el contenido del archivo en cuestión.
* La segunda parte `xclip -sel clip` nos permite copiar información.
* La parte `|` indica que el contenido del archivo (o salida de la primera parte del código) va a ser la entrada de la entrada al comando de la segunda del código.

Dado lo anterior nos permite copiar el contenido del archivo .pub y lo peguemos directamente donde se indicó en GitHub.

Comprobamos la conexión el servidor de GitHub:

```
$ ssh -T git@github.com
```

Nos va a preguntar si nos queremos conectar y le escribiremos que sí en inglés `yes`. Si nos arroja un `Hi, <user>!` donde <user> es tu usuario lo habrás hecho bien. Con ello estaremos seguros de tener conexión con dicho servidor.

Esto nos permite configurar la conexión de Git de nuestro equipo con el servidor remoto de GitHub.

Ya de ahí te clonas un repositorio al que tengas acceso mediante la siguiente forma:

```
$ git clone git@github:user/repo.git
```

Y ya con ello podremos trabajar con Git en dicho repositorio

## Resultado

Con lo anterior ya tendremos configurado nuestro Git, tendremos acceso al servidor remoto de GitHub, podremos trabajar en nuestros repositorios y subir cambios o hacer ciertas acciones con el respaldo que nos brinda GitHub al disponer con ellos nuestros repositorios.

## Referencias

* [Configuración de Git](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration)
* [Generación de una nueva clave SSH y adición al agente SSH](https://docs.github.com/es/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
