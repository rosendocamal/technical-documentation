# RECUPERACIÓN DE UNIDAD USB POR MEDIO DE DISKPART EN SISTEMAS WINDOWS

| Autor        | Fecha      | Actualización |
|--------------|------------|---------------|
|Rosendo Camal | 10/12/2025 | 12/01/26      |

## Objetivo

El objetivo es recuperar una unidad USB dañada mediante comandos y uso del 
programa de consola **DISKPART** en sistemas operativos Windows.

## Proceso de recuperación

A continuación ingrese `diskpart` en la terminal de Windows.

Posteriormente ingresamos el comando `list disk` y para ver la lista
de los dispositivos que tenemos conectados al equipo.

```
DISKPART> list disk
```

Una vez hecho lo anterior debemos buscar cual la unidad USB dañada.

Ingresas el comando `select disk n`, donde *n* es el número de la unidad USB
y se puede conocer dicho dato mirando la sección de la columna *Núm Disco*.

```
DISKPART> select disk n
```

> Nota importante: verificar que la unidad seleccionada sea la unidad USB a recuperar.

Nos dará el mensaje de cuál disco ha sido seleccionado e ingresamos el comado `clean`.

> Nota importante: Con este comando se borrará todos los datos almacenados en la unidad USB a reparar.

```
DISKPART> clean
```

Una vez que el disco esté limpio, pasamos a crear las particiones.

Ingresamos el comando `create partition primary` y luego `select partition 1`.

```
DISKPART> create partition primary
```

```
DISKPART> select partition 1
```

```
DISKPART> active
```

Y por último seleccionamos el sistema de archivos de la unidad USB recuperada,
con el comando `format fs=file_system` donde *file_system* es uno de los siguientes
sistemas de archivo como *fat32*, *ntfs* o *exfat*.

```
DISKPART> format fs=file_system
```

Toca esperar el proceso ya que se formateando con el sistema de archivo de nuestra elección.

### Notas adicionales

Con el comando `format fs=file_system` se requiere esperar demasiado tiempo, una opción para reducir la espera es:

```
DISKPART> format fs=file_system quick
```

También se puede nombrar la unidad USB, sustituye *NAME* por la de su preferencia:

```
DISKPART> format fs=file_system quick label="NAME"

# o bien
DISKPART> format fs=file_system label="NAME"
```

Una vez finalizado el proceso con `quick` o sin ella se puede asignar una letra para que el sistema reconozca en automático:

```
DISKPART> assing
```

## Resultado esperado

Una vez lo anterior se haya concluido, tendremos la unidad USB recuperada y lista para poder usarse.

## Tabla resumen de comandos

| Comando | Función |
|---------|---------|
| `diskpart` | Abre el programa de terminal *diskpart*. |
| `list disk` | Lista las unidades de almacenamiento conectados a la computadora. |
| `select disk n` | Seleciona la unidad USB dañada mediante el índice `n`. |
| `clean` | Borra todos los datos de la unidad USB seleccionada. |
| `create partition primary` | Crea la única y principal partición en la unidad USB seleccionada. |
| `select partition 1` | Seleccionas la partición *1*, en este caso es la única y por defecto *1*. |
| `format fs=file_system` | Se elige el sistema de archivos mediante el argumento *file_system* y puede ser *fat32*, *ntfs* o *exfat*. |
| `format fs=file_system quick` | El argumento `quick` ahorra mucho tiempo. |
| `format fs=file_system label="NAME"` | Asigna el nombre a la unidad USB. |
| `active` | Marca la partición como activa (arrancable) | 
| `assing` | Le asigna en automático una letra para que aparezca en el sistema. |
| `exit` | Sale del programa Diskpart. |

## Referencias

Documentación oficial de Microsoft sobre [Diskpart](https://learn.microsoft.com/es-es/windows-server/administration/windows-commands/diskpart) 
