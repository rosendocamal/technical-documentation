# USO BÁSICO DE 7Z EN LINUX

| Autor         | Fecha      | Actualización |
|---------------|------------|---------------|
| Rosendo Camal | 17/12/2025 | 12/01/2026    |

## CIFRADO/COMPRESIÓN DE ARCHIVOS CON 7ZIP

El comando optimizado para máxima seguridad y compresión es:

```
$ 7z a -p$(cat password.txt) -mhe=on -t7z -mx=9 -m0=lzma2 -md=64m -mmt=on -scrcsha256 [file_name] [path]
```

### Desglose de parámetros:

El comando anterior se puede desglosar a la manera siguiente:

* `7z a`: Comando para "añadir" (comprimir).
* `-p$(cat password.txt)`: Lee la contraseña directamente de un archivo de texto (sin espacio entre `-p` y el comando).
* `-mhe=on`: Cifra los encabezados. Esto hace que no se puedan ver los nombres de los archivos dentro del comprimido sin la clave.
* `-t7z`: Establece el formato contenedor como 7z.
* `-mx=9`: Nivel de compresión "Ultra".
* `-m0=lzma2`: Uso del algoritmo LZMA2, el más eficiente.
* `-md=64m`: Tamaño del diccionario. Se ajustó a 64MB para que sea compatible con la mayoría de las laptops.
* `-mmt=on`: Permite que 7zip use todos los núcleos de tu procesador para terminar más rápido.
* `-scrcsha256`: Genera un hash SHA256 para asegurar que los datos no han sido alterados.
* `[file_name]`: Nombre del archivo final.
* `[path]`: La carpeta o archivos que quieres guardar.

### ¿Cómo extraer este archivo después?

Para descomprimir un archivo con este nivel de seguridad, usa:

```
$ 7z x [file_name] -p$(cat password.txt)
```

> **Nota:** La `x` extrae manteniendo la estructura de carpetas original (mejor que `e`).
