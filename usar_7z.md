# USO BÁSICO DE 7Z EN LINUX

| Autor | Fecha |
|-------|-------|
| Rosendo Camal | 17/12/2025 |

## CIFRADO/COMPRESIÓN DE ARCHIVOS CON 7ZIP

El comando es el siguiente:

```
$ 7z a -p$("cat password.txt") -t7z -mx=9 -m0=lzma2 -md=4g -mmt=on file.7z path
```

El comando anterior se puede desglosar a la manera siguiente:

* `7z`: es el comando principal de la utilería de 7zip en Linux.
* `a`: añade archivos a la comprensión.
* `-t7z`: es el formato de compresión 7z.
* `-mx=9`: indica la máxima compresión.
* `-m0=lzma2`: indica el método de compresión lzma2.
* `md=4g`: señala el tamaño del diccionario (en este caso, 64mb).
* `-mmt=on`: activa el multihilo.
* `file.7z`: ruta y nombre del archivo de la compresión.
* `path`: ruta de los archivos a comprimir
* `-scrc`: algoritmo sha256.
