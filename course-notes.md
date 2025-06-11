# Comandos de Linux: desde cero hasta programar Shell Script

## Índice
- [Games to practice commands](#games-to-practice-commands)
- [Comandos del sistema de ficheros](#comandos-del-sistema-de-ficheros)
  - [Crear, mover, copiar y borrar ficheros y directorios](#crear-mover-copiar-y-borrar-ficheros-y-directorios)
  - [Atajos en la Shell](#atajos-en-la-shell)
  - [Uso de comodines](#uso-de-comodines)
- [Usuarios y Permisos](#usuarios-y-permisos)
  - [Permisos](#permisos)
    - [Cambiar permisos](#cambiar-permisos)
    - [Permisos usando números](#permisos-usando-números)
- [Administración de software](#administración-de-software)
  - [Comandos de gestión de software](#comandos-de-gestión-de-software)
- [Manejo de texto](#manejo-de-texto)
  - [Mostrar y filtrar texto](#mostrar-y-filtrar-texto)
- [Acciones sobre texto](#acciones-sobre-texto)
  - [Ordenar con SORT](#ordenar-con-sort)
  - [Comando UNIQ](#comando-uniq)
  - [Comando WC - Contar](#comando-wc---contar)
  - [Comando REV](#comando-rev)
- [Otros comandos para ficheros](#otros-comandos-para-ficheros)
  - [File -> Tipo de archivo](#file---tipo-de-archivo)
  - [LN -> Crear enlaces](#ln---crear-enlaces)
  - [DU -> Disk Usage](#du---disk-usage)
  - [DF -> Disk Free](#df---disk-free)
- [Búsqueda en el sistema de ficheros](#búsqueda-en-el-sistema-de-ficheros)
  - [LOCATE - Una primera búsqueda](#locate---una-primera-búsqueda)
  - [Comando FIND](#comando-find)
- [Ampliando conocimientos del Shell BASH](#ampliando-conocimientos-del-shell-bash)
  - [ALIAS](#alias)
  - [Sustitución de comandos](#sustitución-de-comandos)
- [Backups](#backups)
  - [Compactar y comprimir TAR](#compactar-y-comprimir-tar)
  - [Copias de seguridad totales, diferenciales e incrementales usando TAR](#copias-de-seguridad-totales-diferenciales-e-incrementales-usando-tar)
- [Gestión de paquetes con dpkg](#gestión-de-paquetes-con-dpkg)
- [Administración de Usuarios, Grupos y Contraseñas](#administración-de-usuarios-grupos-y-contraseñas)
  - [Usuarios](#usuarios)
  - [Contraseñas](#contraseñas)
  - [Grupos](#grupos)
  - [Cambio de propietario o grupo de un elemento](#cambio-de-propietario-o-grupo-de-un-elemento)
  - [Seguridad](#seguridad)
  - [Permisos especiales](#permisos-especiales)
- [Manejo de texto avanzado](#manejo-de-texto-avanzado)
  - [Comando TR](#comando-tr)
  - [Comando SED](#comando-sed)
- [Expresiones Regulares (Regex)](#expresiones-regulares-regex)

---
## Vídeos en YouTube
[Scheduling Tasks with Cron](https://youtu.be/7cbP7fzn0D8?si=Gk-bly6b5wPeRtsB)
[AWK](https://youtu.be/oPEnvuj9QrI?si=QESU7XS5yAe_TvqO)

---
## Games to practice commands
[Over the wire](https://overthewire.org/wargames/bandit/bandit0.html)
[Linux Survival](https://linuxsurvival.com)
[Practice Linux](https://www.practicelinux.com/home)

---
## Comandos del sistema de ficheros

```shell
# Ir al home del usuario
cd
cd ~
# Subir un nivel
cd ..
# Dos niveles
cd ../..

# Mostrar el contenido de uno o varios directorios
ls
# Format llistat
ls -l
# Pes en format llegible
ls -h
# Ordenar por el archivo + pesado
ls -S
# Ordenado al revés
ls -r
# Ordenado por archivo editado primero
ls -t
# Mostrar archivos ocultos
ls -a
```

Conocer el tipo de comando que utilizamos: `type comando`
Posibles resultados:
- orden interna -> built-in, como `cd`
- ejecutable -> /usr/bin/comando
- alias -> apodo de un comando más extenso

Cambiar a superusuario en Debian -> `su -`

### Crear, mover, copiar y borrar ficheros y directorios
```shell
# Crear carpeta
mkdir carpeta
# Crear dentro de un directorio ya existente
mkdir /ruta/carpeta/
# Crear carpetas padre -> hijo
mkdir -p carpeta/carpeta/

# Crear un fichero vacío
touch mi_fichero
# Alternativa
> mi_fichero2

# Mover o renombrar
mv mi_fichero mi_carta  # Renombra
mv mi_carta Documentos/  # Mueve y se podría renombrar

# Copiar: necesita dos parámetros
cp archivo_ruta_origen archivo_ruta_destino
# Copiar directorio y todo su contenido
cp -r directorio_ruta_origen directorio_ruta_destino
# Copiar varios elementos
cp fichero fichero ~  # Pega en la carpeta home

# Borrar fichero
rm fichero
# Borrar directorio vacío
rmdir directorio
# Borrar directorio y su contenido
rm -r directorio

# Ver la jerarquía de archivos y documentos
tree
tree ruta/
```

>[!tip] Copia de archivos que pertenecen a root
>Archivos que pertenecen a root podrían llegar a ser copiados por otros usuarios y luego llegar a ver el contenido que antes no se les permitía.
>
>En cambio si se mueve, el propietario se respeta y con los respectivos permisos.

### Atajos en la Shell

| Atajo                   | Descripción                                    |
| ----------------------- | ---------------------------------------------- |
| Cursores arriba y abajo | Historial de instrucciones                     |
| Tab                     | Completar la ruta de un elemento del sistema   |
| Doble Tab               | Opciones para el auto completado               |
| Ctrl + r                | Búsqueda de instrucciones                      |
| Ctrl + c                | Terminar un comando ejecutado a medias         |
| `history`               | Listado de comandos ejecutados                 |
| `!número`               | Llamar a uno de los comandos de `history`      |
| `sudo !!`               | Ejecutar la última instrucción con privilegios |
| Ctrl + a                | Ir al inicio del comando escrito               |
| Ctrl + e                | Ir al final del comando escrito                |
| Ctrl + l                | borrar la pantalla                             |

### Uso de comodines
Asterisco -> "Cualquier cosa"
```shell
# Hacer referencia a los archivos que tienen una extensión en común, ejemplo:
rm *.gif
# Todo!
rm *

# Cualquier archivo o carpeta cuyo nombre contenga la letra 'u'
cp *u* seleccion/
# O que además en la extensión tengan una 'i'
cp *u*.*i* seleccion/
# Que por lo menos exista una extensión (ficheros)
cp *.*
```

Interrogación -> "Cualquier carácter y cualquier longitud de cadena de caracteres o ninguna"
```shell
# Caracteres obligatorios, cualquier carácter OBLIGATORIO en la posición y lo que siga O NO.
ls c?sa*
# Obligación de una cifra
ls informe-200?.txt
ls informe-19??.txt
# Cualquier longitud de nombre hasta el punto + obligación en el último carácter de la extensión
ls c*.tx?
```

Corchetes -> opciones obligatorias en una posición
```shell
# Elige cualquiera de los tres caracteres marcados en corchetes.
ls c[aio]s*

# Rangos
ls c[a-m]s*
# Mayúsculas
ls c[A-Z]s*
# Números
ls informe-200[1-9].txt
```

> [!tip]
> Resultará útil consultar `man bash` y buscar `Concordancia`:
> /Concordancia + n (seguir buscando hasta llegar a la sección)

[Regresar a índice](#índice)

---
## Usuarios y Permisos
Cuando se crea un usuario, si no se especifica, se crea un grupo con el mismo nombre.

Quién soy y a qué grupos pertenezco -> `id`

| Características | Descripción                                       |
| --------------- | ------------------------------------------------- |
| uid             | identificador del usuario                         |
| gid             | grupo principal al que pertenece el usuario       |
| grupos          | grupos secundarios a los que pertenece el usuario |

Comandos básicos de ejecución:

| Comando                | Descripción                   |
| ---------------------- | ----------------------------- |
| `su usuario`           | Cambio de usuario             |
| `su root -c "comando"` | Ejecutar un comando como root |
| `passwd`               | Cambiar la contraseña         |

Se necessita ser _root_ para crear usuarios y grupos:

| Comando                                      | Descripción                                       |
| -------------------------------------------- | ------------------------------------------------- |
| `groupadd nombre_grupo`                      | Crear grupo                                       |
| `useradd -m nombre_usuario`                  | Crear usuario con su propio grupo                 |
| `useradd -m -g grupo_asignar nombre_usuario` | Crear usuario y asignar a un grupo                |
| `adduser nombre_usuario`                     | Crear usuario con prompts administrativos         |
| `userdel -r nombre_usuario`                  | Eliminar usuario y su directorio home             |
| `groupdel nombre_grupo`                      | Eliminar el grupo (asegurarse que no está en uso) |

### Permisos

| Permisos | Descripción                         |
| -------- | ----------------------------------- |
| `-`      | No tiene permisos                   |
| `r`      | read -> puede ver pero no modificar |
| `w`      | write -> puede modificar            |
| `x`      | execute -> puede ejecutar           |

`-rwxrwxrwx`: permisos por posiciones
- 1r grupo: permisos de usuario
- 2o grupo: permisos del grupo propietario del fichero
- 3r grupo: permisos del resto de usuarios del sistema

|                |   R    |                   W                   |                  X                   |
| -------------- | :----: | :-----------------------------------: | :----------------------------------: |
| **Fichero**    |  Leer  |               Escribir                | Ejecutar<br>(programa, script, etc.) |
| **Directorio** | Listar | Modificar el contenido del directorio |               Acceder                |

#### Cambiar permisos
`chmod`
- Letra del permiso. Si son varios permisos, separarlas por comas.
	- Si no se pone nada, se realiza en todos.
- + para añadir permiso / - para quitar permiso
- Ruta del elemento a cambiar permisos
- Indicación de a quién asignarlo: u=User, g=Group, o=Others

Ejemplos:
```shell
# Añadir permiso de escritura al grupo
chmod g+w ruta

# Quitar el permiso de ejecución a todos
chmod -x ruta

# Añadir permiso de escritura a usuario y a otros
chmod u+w,o+w ruta

# Quitar el permiso de ejecución y escritura al usuario y poner el de escritura al grupo
chmod u-xw,g+w ruta
```

#### Permisos usando números

| Permission Type | Read (4) | Write (2) | Execute (1) |
| --------------- | -------- | --------- | ----------- |
| User            | 1 / 0    | 1 / 0     | 1 / 0       |
| Group           | 1 / 0    | 1 / 0     | 1 / 0       |
| Others          | 1 / 0    | 1 / 0     | 1 / 0       |

Si quiero poner permisos al usuario de lectura y escritura, al grupo sólo lectura y al resto nada:
- Binario -> `110 100 000`
- Decimal -> `640`
- Comando -> `chmod 640 fichero`
En este tipo de cambio, actuará en los tres grupos.

[Regresar a índice](#índice)

---

## Administración de software
En Debian 12, se ha realizado el cambio de que en una instalación limpia se consiguen los paquetes a través de la ISO.
Hasta el día de hoy Debian utilizaba un solo archivo para obtener los repositorios -> `/etc/apt/sources.list`.
A partir de Debian 12 se ha modularizado -> `/etc/sources.list.d/`
Y se debe de añadir un servicio CDN para que funcione el comando APT.

>[!info]
>Es importante conocer el nombre en clave de la distribución -> Debian 12 = Bookworm

**Cómo añadir las fuentes para el APT en un archivo**
[Instrucciones de la web oficial de Debian -> 4.3.1](https://www.debian.org/releases/trixie/release-notes/upgrading.es.html#adding-apt-internet-sources)
1. Nos dirigimos a `/etc/apt/sources.list`
2. Comentamos la linea que especifica que utilizará los paquetes del CD.
3. Añadimos la siguiente linea: `deb https://deb.debian.org/debian bookworm main contrib`

**Cómo añadir las fuentes APT de manera modularizada**
1. Comentamos `/etc/apt/sources.list`
2. `nano /etc/apt/sources.list.d/repo_oficial.list
3. Añadimos `deb https://deb.debian.org/debian bookworm main contrib`

### Comandos de gestión de software

| `apt`        | Descripción                                                                                                    |
| ------------ | -------------------------------------------------------------------------------------------------------------- |
| `update`     | Actualiza la lista de paquetes disponibles en los repositorios                                                 |
| `search`     | Busca paquetes disponibles en los repositorios que coincidan con un término de búsqueda.                       |
| `show`       | Muestra información detallada sobre un paquete específico.                                                     |
| `list`       | Lista paquetes según criterios, como los instalados, actualizables o que cumplen cierta condición de búsqueda. |
| `install`    | Instala un paquete de software en el sistema.                                                                  |
| `remove`     | Elimina un paquete del sistema, manteniendo los archivos de configuración.                                     |
| `purge`      | Elimina un paquete del sistema junto con sus archivos de configuración.                                        |
| `autoremove` | Elimina paquetes que no tienen dependencias.                                                                   |

[Regresar a índice](#índice)

---
## Manejo de texto
>[!warning]
>**¿Por qué es importante?**
>Configuraciones se guardan en ficheros de texto.
>Los logs también son en formato texto.
>Cualquier programa en modo consola que muestra resultados mediante texto.

>[!info]
>Dominar esto ayuda muchísimo como administrador del servidor.

### Mostrar y filtrar texto

**Mostrar texto**
```shell
# Mostrar texto que se recibe
echo "Hola Mundo"

# Interpretar ordenes:
# Salto de línea
echo -e "Hola \nMundo"
# Tabulación
echo -e "Hola\tMundo"

# Ver las variables del sistema
echo $PATH
echo "Soy el usuario: $USER"

# Las comillas simples no interpretarán variables:
echo 'La variable del sistema que guarda el usuario es: $USER'

# Mostrar un fichero de texto
cat /etc/issue
# Mostrar con número de línea en el archivo
cat -n /etc/fstab
```

**Mostrar contenido de ficheros de texto**
```shell
# Mostrar el texto poco a poco -> 'h' muestra la ayuda
more archivo.ext
# Su mejora para los archivos grandes y con una "ventana nueva"
less archivo.ext
```
Búsqueda dentro del comando 'less':
/palabra + enter -> buscar coincidencias hacia delante.
?palabra + enter -> buscar coincidencias hacia atrás.
g -> Ir al principio del documento.
G -> Ir al final del documento.
:10 + g -> Ir a un número de línea concreto.

**Primeras y últimas líneas**
```shell
# Mostrar las 10 primeras líneas
head file.txt
# Mostrar una cantidad de líneas (5 por ejemplo):
head -n5 file.txt

# Mostrar las 10 últimas líneas, funciones como head
tail file.txt

# Conocer quién ha sido el último usuario registrado
tail -1 /etc/passwd
# Quien fue el primer usuario
head -n1 /etc/passwd

# Ejemplo de como monitorizar un archivo log (paquetes en este caso)
tail -f /var/log/dpkg.log  # CTRL + c para salir de este
```

**Filtrar texto**
`grep` -> Muestra sólo las líneas que cumplen con un patrón.
- `-i` -> No distingue entre mayúscula y minúscula.
- `-w` -> El patrón tiene que ser una palabra exacta.
- `-v` -> Muestra las que NO coinciden con el patrón.
- `-r` -> Busca en los ficheros de forma recursiva.
- `-n` -> Indica el número de línea.
- `-l` -> Indica solamente el fichero donde ha encontrado alguna coincidencia.
- `-c` -> La cantidad de líneas que cumplen con el patrón.
- `-B n`, `-A n` -> Muestra una cantidad de líneas n, antes (before) o después (after) de encontrar el patrón.
- `--color` -> Destaca el patrón en color dentro de la línea seleccionada.
- `^a` -> Empieza la línea por...
- `u$` -> ...termina la línea por...

Patrón -> `grep -opción palabra /path/archivo`

```shell
# Buscar una palabra en TODO los archivos de un directorio sin ir a niveles por debajo
grep root /etc/* 2> /dev/null  # enviar errores a nulo

# Buscar teniendo presente los subdirectorios (recursivo)
grep -r contrib /etc/* 2> /dev/null

# Buscar también la siguiente línea de la coincidencia
grep -w -A 1 1000 /etc/passwd

# Buscar también la anterior línea de la coincidencia
grep -w -B 1 1000 /etc/passwd
```

`cut` -> Corte "vertical", sólo una parte de cada línea.
- `-c` -> Selecciona sólo los caracteres que le indiquemos. Se pueden hacer delimitaciones.
	- `cut -c 5,10 file.txt` // `cut -c 7-25 file.txt`
- `-d` -> Indica un carácter separador entre los distintos campos de una línea. Por defecto buscará el tabulador.
- `-f` -> Elegir los campos que queremos que se muestren.

[Regresar a índice](#índice)

---

## Tuberías y redirecciones
### Redirecciones
``` shell
# Redirigir la salida del comando hacia un texto
echo "Hola mundo" > texto.txt
# Comprobar
cat texto.txt

# Si el archivo ya existe, añadir la salida al final de este.
echo "Hello world" >> texto.txt
```

Redirigir mensajes de *error* -> `2>`
Redirigir *todos* los mensajes -> `&>`
Desechar los resultados aquí -> `/dev/null`

### Tuberías
Redirigir el resultado hacia otro comando. Se pueden ir concatenando.
Se le llama "tubería" (pipe) ya que establece un "canal" por el que pasará el texto de un comando a otro.
``` shell
# Ejemplos
echo "HOLA MUNDO" | wc

grep -w ana /etc/passwd | cut -d":" -f1 | sort

ls -lr /etc/ | less

ls -lR /etc/ 2> /dev/null | grep shadow

ps -e | grep as
```

[Regresar a índice](#índice)

---
## Acciones sobre texto
### Ordenar con SORT
- Ordena de forma alfabética por defecto.

| Opciones | Tipo orden                         | Ejemplo                                                                                            |
| :------: | ---------------------------------- | -------------------------------------------------------------------------------------------------- |
|   `-r`   | Inversa                            | `sort -r archivo.txt`                                                                              |
|   `-t`   | Carácter de separación de columnas | `sort -t "," 2 archivo.txt`<br>Divide en el número de columnas dependiendo de cuantas "," existen. |
|   `-k`   | Columnas específicas               | `sort -t "," -k 2 archivo.txt`<br>Escoger la segunda columna de la división.                       |
|   `-n`   | Numéricamente                      | `sort -t "," -k 2 -n archivo.txt`                                                                  |
|          |                                    |                                                                                                    |
|   `-u`   | Eliminar líneas duplicadas         | `sort -u archivo.txt`                                                                              |

### Comando UNIQ
- Elimina las líneas repetidas contiguas en un archivo o entrada estàndar.

| Opciones | Tipo orden                         | Ejemplo                                           |
| :------: | ---------------------------------- | ------------------------------------------------- |
|   `-i`   | Ignora mayúsculas y minúsculas     | `cut -d"," -f1 nombres.txt \| sort \| uniq -i`    |
|   `-c`   | Cuenta la cantidad de repeticiones | `cut -d"," -f1 nombres.txt \| sort \| uniq -i -c` |
|   `-u`   | Muestra las no duplicadas          | `cut -d"," -f1 nombres.txt \| sort \| uniq -i -u` |

### Comando WC - Contar
- Contar líneas, palabras y bytes en archivos de texto o entradas estándar:
	- {Líneas} {Palabras} {Bytes}
		- Hay quien dice que el apartado de bytes se refiere al número de caracteres.
		- No es cierto porque si hacemos la comprobación con la "ñ", veremos que aumentan los bytes. -> consultar la pág. man de wc

| Opciones | Tipo orden |
| :------: | ---------- |
|   `-l`   | Líneas     |
|   `-w`   | Palabras   |
|   `-c`   | Bytes      |
|   `-m`   | Caracteres |

Ejemplos:
```shell
# Entrada estándar
echo "Hola" | wc

# Fichero
wc /etc/passwd
```

### Comando REV
Invertir el order de los caracteres en cada línea de un archivo de texto o entrada estándar.
- Útil cuando una línea se puede dividir en columnas y quiero convertir la última en la primera.

Ejemplos:
```shell
# Listado de todos los directorios
ls -lR ficheros | grep '/' | grep -v ^l | rev | cut -d"/" -f1 | rev
```

[Regresar a índice](#índice)

---

## Otros comandos para ficheros

### File -> Tipo de archivo
```shell
# Mostrar solo la descripción del tipo de archivo, su codificación.
file -b <archivo>

# Muestra el MIME del archivo.
file -i <archivo>

# Lee los nombres de los archivos desde un fichero
file -f <archivo_con_nombres>

# Ver los archivos y los tipos de cada uno en el directorio
file *
```

### LN -> Crear enlaces
Dos tipos:
- **Duro** -> Un puntero a la información del disco duro.
	- Aunque veamos que su peso es idéntico al archivo original, no es así.
		- Es un puntero a un archivo original.
		- Si borramos los enlaces, el peso seguirá siendo el mismo.
	- Comando `ls -lh`, la columna que hay después de los permisos, nos muestra el número de enlaces duros que tiene el archivo.
- **Blando** -> Se sustituye por la ruta del fichero original. Parecido a tener un "alias". Como un acceso directo.
	- Cuidado con poner rutas relativas sobretodo al mover archivos.

```shell
# Enlace duro:
ln <archivo> <ruta_y_nombre_archivo>

# Comprobar detalles del archivo
stat <archivo>

# Crear un enlace blando
ln -s <ruta_absoluta_archivo_original> <ruta_acceso_directo_archivo>
```

### DU -> Disk Usage
Espacio de disco ocupado por un fichero o por un directorio. Si le pasamos un directorio mostrará recursivamente el tamaño de todos los subdirectorios.
```shell
# Mostrar el tamaño del directorio y sus subdirectorios
du <ruta_directorio>

# Mostrar el tamaño en lectura humana
du -h

# Mostrar la cantidad total de lo que ocupa el directorio
du -s

# Mostrar la cantidad de todo lo que hay dentro del directorio
du -sh *
# Se puede concatenar con sort
du -sh * | sort -hr
```

### DF -> Disk Free
Información sobre las particiones del sistema: tamaño total, usado y libre.
```shell
# Ver el espacio disponible en el disco y sus particiones
df -h

# Verlo por tipo
df -hT

# Ver el tamaño de la partición donde está ubicado un archivo
df -h <ruta_archivo>
```
**Alternativa a DF**
```shell
sudo apt install dfc
# Ejecución
dfc
```

[Regresar a índice](#índice)

---

## Búsqueda en el sistema de ficheros
### LOCATE - Una primera búsqueda
```shell
# Instalar el paquete
sudo apt install locate

# Actualizar el sistema de ficheros con una base de datos para utilizar locate
su -
# Si se crea un archivo, un directorio, etc., actualizar la BD
updatedb

# Sintaxis locate
locate [opciones] [patrón]
```

| Opciones | Descripción                                    |
| :------: | ---------------------------------------------- |
|   `-i`   | Ignorar mayúsculas y minúsculas en la búsqueda |
|   `-c`   | Contar el número de coincidencias encontradas  |
|   `-n`   | Limitar el número de resultados.               |
|   `-r`   | Usar expresiones regulares para la búsqueda.   |

Ejemplos:
```shell
# Ignorando may y minus
locate -i "sources.list"

# Utilizando regex
locate -r "/sources.list$"
```

### Comando FIND
>[!danger]
>Comando super importante de dominar como administrador.

- Busca de forma recursiva en un directorio todos los ficheros que cumplan ciertas condiciones.
Sintaxis:
```shell
find [ruta] [opciones] [acción]
```

|       Opciones        | Descripción                                                                                                                                                                                                                                   |
| :-------------------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|        `-name`        | Especificar patrones para los nombres de los ficheros a buscar.                                                                                                                                                                               |
|       `-iname`        | Igual que `name` pero sin diferencias mayúsculas y minúsculas.                                                                                                                                                                                |
|        `-type`        | Tipo de fichero a buscar:<br>`d`-> directorios<br>`f` -> ficheros<br>`l` -> enlaces simbólicos<br>`b` -> dispositivos de bloque<br>`c` -> dispositivos de carácter<br>`p` -> para tuberías<br>`s` -> para sockets.                            |
|    `-size +/- <n>`    | `c` -> bytes<br>`k` -> kilobytes<br>`M` -> megabytes<br>`G` -> gigabytes                                                                                                                                                                      |
|     `-perm <num>`     | Ficheros cuyos permisos sean los expresados exactamente por número.<br>`-720` -> El signo `-` actuará como un AND: quiero exactamente esos permisos.<br>`/111` -> El signo `/` actuará como un OR: uno de los tres números debe de coincidir. |
|   `-user <usuario>`   | Especifica el usuario propietario del fichero                                                                                                                                                                                                 |
|  `-group <usuario>`   | Especifica el grupo propietario del fichero                                                                                                                                                                                                   |
|    `-mmin [+/-]n`     | Los datos del fichero fueron modificados por última vez hace `n` minutos.                                                                                                                                                                     |
|    `-mtime [+/-]n`    | Los datos del fichero fueron modificados por última vez hace `n` días.                                                                                                                                                                        |
| `-maxdepth <niveles>` | Desciende el número entero de niveles positivos en la jerarquía del directorio.                                                                                                                                                               |
|        `-path`        | Buscar por toda la ruta de los elementos. Se pueden utilizar comodines.                                                                                                                                                                       |
|       `-regex`        | Igual que `-path`, pero usando expresiones regulares.                                                                                                                                                                                         |

| Modificadores | Descripción                                                                        |
| :-----------: | ---------------------------------------------------------------------------------- |
|    `-not`     | Niega la condición que pongamos a continuación                                     |
|     `-o`      | Se puede cumplir cualquiera de las dos opciones que tiene a su izquierda y derecha |
|     `-a`      | Opción por defecto, se tienen que cumplir las dos opciones que une.                |

|     Acciones      | Descripción                                                                                                                                                                                                         |
| :---------------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|       `-ls`       | Lista con detalles los elementos encontrados                                                                                                                                                                        |
|     `-delete`     | Elimina los elementos encontrados                                                                                                                                                                                   |
| `-exec <comando>` | Definir un comando a ejecutarse para cada resultado de la búsqueda.<br>- `'{}'` ->(Origen) En el resultado se sustituye por el nombre de los ficheros encontrados.<br>- `';'` -> indica la finalización del comando |


Ejemplos:
```shell
# Buscar un tipo de extensión en un directorio
find ficheros/ -iname "*.jpg"

# Mostrar ficheros y directorios
find ficheros/ -iname "*carta*"
# Directorio
find ficheros/ -iname "*carta*" -type d
# Fichero
find ficheros/ -iname "*carta*" -type f

# Buscar a partir del directorio actual, recursivamente.
find . -iname "*.txt"

# Buscar elementos superiores a 200k y que solamente sean .jpg
find . -size +200k -iname "*.jpg"
# Menores de 200k
find . -size -200k -iname "*.jpg"

# Buscar por permisos exactos en ficheros
find . -perm -777 -type f
# Buscar por permisos posibles en directorios
find . -perm /111 -type d

# Buscar lo que pertenece a un usuario
find /var/ -user man
# Buscar lo que pertenece al grupo de usuario
find /var/ -group man


# Archivos modificados hace menos de 5 minutos
find . -mmin -5
# Archivos modificados hace más de 10 minutos
find . -mmin +10
# Archivos modificados hace menos de 1 día
find . -mtime -1
# Archivos modificados hace más de 1 día
find . -mtime +1 

# Rutas que contengan 2009
find . -path "*2009*"
# Utilizando expresiones regulares
find . -regex ".*20+.*"
# Buscar hasta en 3 niveles de profundidad
find . -maxdepth 3 -iname "*.txt"

# Buscar archivos que tengan la extensión .jpg o .jpeg
find . -iname "*.jpg" -o -iname "*.jpeg"
# NO mostrar los archivos que tengan la extensión .jpg
find . -not -iname "*.jpg"

# Listar todos los ficheros de /etc/ con extensión .conf que sean menores de un MB
find /etc/ -inmae '*.conf' -size -1M -ls
# Borra de mi directorio personal todos los ficheros de más de 2GB
find ~ -size +2G -delete

# Copia todos los ficheros con extensión .conf que sean menores de un MB al directorio /home/copias
find /etc/ -iname '*.conf' -size -1M -exec cp '{}' /home/copias/ ';'
# Borra de mi directorio personal todos los ficheros de más de 2GB
find ~ -size +2G -exec rm '{}' ';'
```

>[!tip]
>Sigue la filosofía de "divide y vencerás":
>Empieza por un comando y ve acumulando pipes hasta llegar al resultado deseado.

[Regresar a índice](#índice)

---

## Ampliando conocimientos del Shell BASH

### ALIAS
- Mostrar el listado de los que están configurados: `alias`
- Sintaxis para crear un alias: `alias nombre_alias='comando'`
- Para que sean permanentes, hay que añadirlos al fichero que carga al arrancar el sistema.
	- BASH -> `~/.bashrc
- Eliminar un alias: `unalias nombre_alias`

### Sustitución de comandos
- Realización -> `$(comando)`
```shell
# Ejemplo de ejecución
echo Estoy trabajando en $(pwd)
echo Hoy es $(date)
touch $(date +%s-%N).dat
```

[Regresar a índice](#índice)

---
## Backups

### Compactar y comprimir TAR
Sintaxis -> `tar [opciones] destino.tar datos`

```shell
# Compactar: Compact file
tar -cf fichero.tar /etc/ /var/

# Extraer: Extract file
tar -xf fichero.tar

# Comprimir con gzip
tar -czf fichero.tar.gz /etc/ var/

# Extraer con gzip
tar -xzf fichero.tar.gz
```

| Opciones | Descripción                                           |
| :------: | ----------------------------------------------------- |
|   `-c`   | Compacta                                              |
|   `-x`   | Extrae                                                |
|   `-f`   | Escribe o lee de un fichero                           |
|   `-z`   | Comprime o descomprime con gzip                       |
|   `-j`   | Comprime o descomprime con bzip2                      |
|   `-P`   | Utiliza rutas absolutas (por defecto serán relativas) |
|   `-p`   | Preserva los permisos de los ficheros originales      |
|   `-r`   | Añade elementos a un fichero compactado               |
|   `-t`   | Muestra la información que contiene un fichero tar    |

Ejemplos
```shell
# Crear fichero COMPACTADO -> se podrá añadir información
tar -cf copia.tar /var/log/
# Añadir archivos en el compactado
tar -rf copia.tar /home/alumno/ficheros

# Crear fichero COMPRIMIDO con GZip -> el tamaño es menor que compactar
tar -czf copia.tar.gz /var/log/

# Comprimir con BZip2
tar -cjf copiaJ.tar.gz /var/log/
# Descomprimir en el directorio donde estamos
tar -xjf copiaJ.tar.gz
# Comprimir con la ruta absoluta
tar -cPjf copiaJA.tar.gz /var/log/
# Descomprimir con la ruta absoluta
tar -xPjf copiaJA.tar.gz
```

### Copias de seguridad totales, diferenciales e incrementales usando TAR

- Totales: se copian todos los datos
- Diferenciales: se copian sólo los que se hayan modificado desde una fecha indicada.
```shell
# Se propone fecha americana para las copias que estén en un cron, por ejemplo.
# Al final se indican los directorios que se copiarán
# a partir de una fecha.
tar -czf $(date +%F)-<nombre>.tar.gz <ruta_directorio>/ -N 2024-06-01
```
- Incrementales: se copian sólo los ficheros que se han modificado desde la última copia.
```shell
# Al ejecutar el archivo de registro.
tar -czf $(date +%F)-<nombre>.tar.gz <ruta_directorio>/ -g copias.log
# Si se vuelve a ejecutar, solamente hará copia de aquello que no tiene registrado en el .log
```

**Estrategia Abuelo-Padre-Hijo**:
- 1 Copia *total* al mes.
- 1 Copia *diferencial* cuando se inicia la semana.
- 1 Copia _incremental_ de lunes a viernes.

[Regresar a índice](#índice)

---
## Gestión de paquetes con dpkg
Se utiliza cuando el fichero ya está en nuestro disco. Se utiliza para distribuciones Debian.

| Opciones | Descripción                                                                      | Ejemplo                          |
| :------: | -------------------------------------------------------------------------------- | -------------------------------- |
|   `-i`   | Necesita la ruta completa del fichero .deb a instalar                            | `dpkg -i htop_2.0.2-1_amd64.deb` |
|   `-r`   | Borra un paquete pero deja los ficheros de configuración.                        | `dpkg -r htop`                   |
|   `-P`   | Borra un paquete incluyendo todos los ficheros de configuración.                 | `dpkg -P htop`                   |
|   `-s`   | Muestra información y el estado de un paquete instalado                          | `dpkg -s htop`                   |
|   `-I`   | Muestra información de un archivo .deb                                           | `dpkg -I htop_2.0.2-1_amd64.deb` |
|   `-l`   | Lista todos los paquetes instalados que coincidan con un patrón determinado      | `dpkg -l apache*`                |
|   `-L`   | Muestra todos los ficheros que ha instalado un paquete                           | `dpkg -L htop`                   |
|   `-S`   | Muestra los paquetes que contienen ficheros que coincidan con el patrón indicado | `dpkg -S mount*`                 |

**dpkg-reconfigure** -> Reconfigurar un paquete instalado -> `dpkg-reconfigure nslcd`

[Regresar a índice](#índice)

---
## Administración de Usuarios, Grupos y Contraseñas

### Usuarios
`/etc/passwd` -> fichero donde se guardarán los usuarios del sistema.
Estructura -> `usuario:X:UID:GID:datos_personales:directorio_home:shell`

Añadir un usuario -> `useradd [opciones] nombre_usuario`

| Opciones | Descripción             |
| :------: | ----------------------- |
|   `-d`   | Directorio home         |
|   `-m`   | Crea el directorio home |
|   `-g`   | Grupo principal         |
|   `-G`   | Grupo/s secundario/s    |
|   `-s`   | Intérprete de comandos  |
|   `-k`   | Directorio de plantilla |

Observaciones:
- Cuando se añade un usuario sin opciones, se crea también un grupo con el mismo nombre del usuario y con otro GID.
	- No se crea la home, ni tampoco tiene un intérprete de comandos.

Crear un usuario:
```shell
# Ver los grupos de los que disponemos para asignarle uno
tail /etc/group

# Crear usuario con las opciones de uso normal.
useradd -d /home/<nombre> -m -g <nombre_grupo> -s /bin/bash <nombre_usuario>

# Lo mismo pero añadiendo grupos secundarios y dejando el home por defecto
useradd -m -g <nombre_grupo> -s /bin/bash -G <grupo1>,<grupo2> <nombre_usuario>
```

### Contraseñas
`/etc/shadow` -> fichero donde se guardarán las contraseñas cifradas de los usuarios y la información referente a su validez.

Estructura:
1. Nombre de Usuario
2. Contraseña cifrada -> `$id$salt$hashed`
	- `$id` -> Indica el algoritmo de cifrado:
		- `$1# Comandos de Linux: desde cero hasta programar Shell Script

## Índice
- [Games to practice commands](#games-to-practice-commands)
- [Comandos del sistema de ficheros](#comandos-del-sistema-de-ficheros)
  - [Crear, mover, copiar y borrar ficheros y directorios](#crear-mover-copiar-y-borrar-ficheros-y-directorios)
  - [Atajos en la Shell](#atajos-en-la-shell)
  - [Uso de comodines](#uso-de-comodines)
- [Usuarios y Permisos](#usuarios-y-permisos)
  - [Permisos](#permisos)
    - [Cambiar permisos](#cambiar-permisos)
    - [Permisos usando números](#permisos-usando-números)
- [Administración de software](#administración-de-software)
  - [Comandos de gestión de software](#comandos-de-gestión-de-software)
- [Manejo de texto](#manejo-de-texto)
  - [Mostrar y filtrar texto](#mostrar-y-filtrar-texto)
- [Acciones sobre texto](#acciones-sobre-texto)
  - [Ordenar con SORT](#ordenar-con-sort)
  - [Comando UNIQ](#comando-uniq)
  - [Comando WC - Contar](#comando-wc---contar)
  - [Comando REV](#comando-rev)
- [Otros comandos para ficheros](#otros-comandos-para-ficheros)
  - [File -> Tipo de archivo](#file---tipo-de-archivo)
  - [LN -> Crear enlaces](#ln---crear-enlaces)
  - [DU -> Disk Usage](#du---disk-usage)
  - [DF -> Disk Free](#df---disk-free)
- [Búsqueda en el sistema de ficheros](#búsqueda-en-el-sistema-de-ficheros)
  - [LOCATE - Una primera búsqueda](#locate---una-primera-búsqueda)
  - [Comando FIND](#comando-find)
- [Ampliando conocimientos del Shell BASH](#ampliando-conocimientos-del-shell-bash)
  - [ALIAS](#alias)
  - [Sustitución de comandos](#sustitución-de-comandos)
- [Backups](#backups)
  - [Compactar y comprimir TAR](#compactar-y-comprimir-tar)
  - [Copias de seguridad totales, diferenciales e incrementales usando TAR](#copias-de-seguridad-totales-diferenciales-e-incrementales-usando-tar)
- [Gestión de paquetes con dpkg](#gestión-de-paquetes-con-dpkg)
- [Administración de Usuarios, Grupos y Contraseñas](#administración-de-usuarios-grupos-y-contraseñas)
  - [Usuarios](#usuarios)
  - [Contraseñas](#contraseñas)
  - [Grupos](#grupos)
  - [Cambio de propietario o grupo de un elemento](#cambio-de-propietario-o-grupo-de-un-elemento)
  - [Seguridad](#seguridad)
  - [Permisos especiales](#permisos-especiales)
- [Manejo de texto avanzado](#manejo-de-texto-avanzado)
  - [Comando TR](#comando-tr)
  - [Comando SED](#comando-sed)
- [Expresiones Regulares (Regex)](#expresiones-regulares-regex)

---
## Vídeos en YouTube
[Scheduling Tasks with Cron](https://youtu.be/7cbP7fzn0D8?si=Gk-bly6b5wPeRtsB)
[AWK](https://youtu.be/oPEnvuj9QrI?si=QESU7XS5yAe_TvqO)

---
## Games to practice commands
[Over the wire](https://overthewire.org/wargames/bandit/bandit0.html)
[Linux Survival](https://linuxsurvival.com)
[Practice Linux](https://www.practicelinux.com/home)

---
## Comandos del sistema de ficheros

```shell
# Ir al home del usuario
cd
cd ~
# Subir un nivel
cd ..
# Dos niveles
cd ../..

# Mostrar el contenido de uno o varios directorios
ls
# Format llistat
ls -l
# Pes en format llegible
ls -h
# Ordenar por el archivo + pesado
ls -S
# Ordenado al revés
ls -r
# Ordenado por archivo editado primero
ls -t
# Mostrar archivos ocultos
ls -a
```

Conocer el tipo de comando que utilizamos: `type comando`
Posibles resultados:
- orden interna -> built-in, como `cd`
- ejecutable -> /usr/bin/comando
- alias -> apodo de un comando más extenso

Cambiar a superusuario en Debian -> `su -`

### Crear, mover, copiar y borrar ficheros y directorios
```shell
# Crear carpeta
mkdir carpeta
# Crear dentro de un directorio ya existente
mkdir /ruta/carpeta/
# Crear carpetas padre -> hijo
mkdir -p carpeta/carpeta/

# Crear un fichero vacío
touch mi_fichero
# Alternativa
> mi_fichero2

# Mover o renombrar
mv mi_fichero mi_carta  # Renombra
mv mi_carta Documentos/  # Mueve y se podría renombrar

# Copiar: necesita dos parámetros
cp archivo_ruta_origen archivo_ruta_destino
# Copiar directorio y todo su contenido
cp -r directorio_ruta_origen directorio_ruta_destino
# Copiar varios elementos
cp fichero fichero ~  # Pega en la carpeta home

# Borrar fichero
rm fichero
# Borrar directorio vacío
rmdir directorio
# Borrar directorio y su contenido
rm -r directorio

# Ver la jerarquía de archivos y documentos
tree
tree ruta/
```

>[!tip] Copia de archivos que pertenecen a root
>Archivos que pertenecen a root podrían llegar a ser copiados por otros usuarios y luego llegar a ver el contenido que antes no se les permitía.
>
>En cambio si se mueve, el propietario se respeta y con los respectivos permisos.

### Atajos en la Shell

| Atajo                   | Descripción                                    |
| ----------------------- | ---------------------------------------------- |
| Cursores arriba y abajo | Historial de instrucciones                     |
| Tab                     | Completar la ruta de un elemento del sistema   |
| Doble Tab               | Opciones para el auto completado               |
| Ctrl + r                | Búsqueda de instrucciones                      |
| Ctrl + c                | Terminar un comando ejecutado a medias         |
| `history`               | Listado de comandos ejecutados                 |
| `!número`               | Llamar a uno de los comandos de `history`      |
| `sudo !!`               | Ejecutar la última instrucción con privilegios |
| Ctrl + a                | Ir al inicio del comando escrito               |
| Ctrl + e                | Ir al final del comando escrito                |
| Ctrl + l                | borrar la pantalla                             |

### Uso de comodines
Asterisco -> "Cualquier cosa"
```shell
# Hacer referencia a los archivos que tienen una extensión en común, ejemplo:
rm *.gif
# Todo!
rm *

# Cualquier archivo o carpeta cuyo nombre contenga la letra 'u'
cp *u* seleccion/
# O que además en la extensión tengan una 'i'
cp *u*.*i* seleccion/
# Que por lo menos exista una extensión (ficheros)
cp *.*
```

Interrogación -> "Cualquier carácter y cualquier longitud de cadena de caracteres o ninguna"
```shell
# Caracteres obligatorios, cualquier carácter OBLIGATORIO en la posición y lo que siga O NO.
ls c?sa*
# Obligación de una cifra
ls informe-200?.txt
ls informe-19??.txt
# Cualquier longitud de nombre hasta el punto + obligación en el último carácter de la extensión
ls c*.tx?
```

Corchetes -> opciones obligatorias en una posición
```shell
# Elige cualquiera de los tres caracteres marcados en corchetes.
ls c[aio]s*

# Rangos
ls c[a-m]s*
# Mayúsculas
ls c[A-Z]s*
# Números
ls informe-200[1-9].txt
```

> [!tip]
> Resultará útil consultar `man bash` y buscar `Concordancia`:
> /Concordancia + n (seguir buscando hasta llegar a la sección)

[Regresar a índice](#índice)

---
## Usuarios y Permisos
Cuando se crea un usuario, si no se especifica, se crea un grupo con el mismo nombre.

Quién soy y a qué grupos pertenezco -> `id`

| Características | Descripción                                       |
| --------------- | ------------------------------------------------- |
| uid             | identificador del usuario                         |
| gid             | grupo principal al que pertenece el usuario       |
| grupos          | grupos secundarios a los que pertenece el usuario |

Comandos básicos de ejecución:

| Comando                | Descripción                   |
| ---------------------- | ----------------------------- |
| `su usuario`           | Cambio de usuario             |
| `su root -c "comando"` | Ejecutar un comando como root |
| `passwd`               | Cambiar la contraseña         |

Se necessita ser _root_ para crear usuarios y grupos:

| Comando                                      | Descripción                                       |
| -------------------------------------------- | ------------------------------------------------- |
| `groupadd nombre_grupo`                      | Crear grupo                                       |
| `useradd -m nombre_usuario`                  | Crear usuario con su propio grupo                 |
| `useradd -m -g grupo_asignar nombre_usuario` | Crear usuario y asignar a un grupo                |
| `adduser nombre_usuario`                     | Crear usuario con prompts administrativos         |
| `userdel -r nombre_usuario`                  | Eliminar usuario y su directorio home             |
| `groupdel nombre_grupo`                      | Eliminar el grupo (asegurarse que no está en uso) |

### Permisos

| Permisos | Descripción                         |
| -------- | ----------------------------------- |
| `-`      | No tiene permisos                   |
| `r`      | read -> puede ver pero no modificar |
| `w`      | write -> puede modificar            |
| `x`      | execute -> puede ejecutar           |

`-rwxrwxrwx`: permisos por posiciones
- 1r grupo: permisos de usuario
- 2o grupo: permisos del grupo propietario del fichero
- 3r grupo: permisos del resto de usuarios del sistema

|                |   R    |                   W                   |                  X                   |
| -------------- | :----: | :-----------------------------------: | :----------------------------------: |
| **Fichero**    |  Leer  |               Escribir                | Ejecutar<br>(programa, script, etc.) |
| **Directorio** | Listar | Modificar el contenido del directorio |               Acceder                |

#### Cambiar permisos
`chmod`
- Letra del permiso. Si son varios permisos, separarlas por comas.
	- Si no se pone nada, se realiza en todos.
- + para añadir permiso / - para quitar permiso
- Ruta del elemento a cambiar permisos
- Indicación de a quién asignarlo: u=User, g=Group, o=Others

Ejemplos:
```shell
# Añadir permiso de escritura al grupo
chmod g+w ruta

# Quitar el permiso de ejecución a todos
chmod -x ruta

# Añadir permiso de escritura a usuario y a otros
chmod u+w,o+w ruta

# Quitar el permiso de ejecución y escritura al usuario y poner el de escritura al grupo
chmod u-xw,g+w ruta
```

#### Permisos usando números

| Permission Type | Read (4) | Write (2) | Execute (1) |
| --------------- | -------- | --------- | ----------- |
| User            | 1 / 0    | 1 / 0     | 1 / 0       |
| Group           | 1 / 0    | 1 / 0     | 1 / 0       |
| Others          | 1 / 0    | 1 / 0     | 1 / 0       |

Si quiero poner permisos al usuario de lectura y escritura, al grupo sólo lectura y al resto nada:
- Binario -> `110 100 000`
- Decimal -> `640`
- Comando -> `chmod 640 fichero`
En este tipo de cambio, actuará en los tres grupos.

[Regresar a índice](#índice)

---

## Administración de software
En Debian 12, se ha realizado el cambio de que en una instalación limpia se consiguen los paquetes a través de la ISO.
Hasta el día de hoy Debian utilizaba un solo archivo para obtener los repositorios -> `/etc/apt/sources.list`.
A partir de Debian 12 se ha modularizado -> `/etc/sources.list.d/`
Y se debe de añadir un servicio CDN para que funcione el comando APT.

>[!info]
>Es importante conocer el nombre en clave de la distribución -> Debian 12 = Bookworm

**Cómo añadir las fuentes para el APT en un archivo**
[Instrucciones de la web oficial de Debian -> 4.3.1](https://www.debian.org/releases/trixie/release-notes/upgrading.es.html#adding-apt-internet-sources)
1. Nos dirigimos a `/etc/apt/sources.list`
2. Comentamos la linea que especifica que utilizará los paquetes del CD.
3. Añadimos la siguiente linea: `deb https://deb.debian.org/debian bookworm main contrib`

**Cómo añadir las fuentes APT de manera modularizada**
1. Comentamos `/etc/apt/sources.list`
2. `nano /etc/apt/sources.list.d/repo_oficial.list
3. Añadimos `deb https://deb.debian.org/debian bookworm main contrib`

### Comandos de gestión de software

| `apt`        | Descripción                                                                                                    |
| ------------ | -------------------------------------------------------------------------------------------------------------- |
| `update`     | Actualiza la lista de paquetes disponibles en los repositorios                                                 |
| `search`     | Busca paquetes disponibles en los repositorios que coincidan con un término de búsqueda.                       |
| `show`       | Muestra información detallada sobre un paquete específico.                                                     |
| `list`       | Lista paquetes según criterios, como los instalados, actualizables o que cumplen cierta condición de búsqueda. |
| `install`    | Instala un paquete de software en el sistema.                                                                  |
| `remove`     | Elimina un paquete del sistema, manteniendo los archivos de configuración.                                     |
| `purge`      | Elimina un paquete del sistema junto con sus archivos de configuración.                                        |
| `autoremove` | Elimina paquetes que no tienen dependencias.                                                                   |

[Regresar a índice](#índice)

---
## Manejo de texto
>[!warning]
>**¿Por qué es importante?**
>Configuraciones se guardan en ficheros de texto.
>Los logs también son en formato texto.
>Cualquier programa en modo consola que muestra resultados mediante texto.

>[!info]
>Dominar esto ayuda muchísimo como administrador del servidor.

### Mostrar y filtrar texto

**Mostrar texto**
```shell
# Mostrar texto que se recibe
echo "Hola Mundo"

# Interpretar ordenes:
# Salto de línea
echo -e "Hola \nMundo"
# Tabulación
echo -e "Hola\tMundo"

# Ver las variables del sistema
echo $PATH
echo "Soy el usuario: $USER"

# Las comillas simples no interpretarán variables:
echo 'La variable del sistema que guarda el usuario es: $USER'

# Mostrar un fichero de texto
cat /etc/issue
# Mostrar con número de línea en el archivo
cat -n /etc/fstab
```

**Mostrar contenido de ficheros de texto**
```shell
# Mostrar el texto poco a poco -> 'h' muestra la ayuda
more archivo.ext
# Su mejora para los archivos grandes y con una "ventana nueva"
less archivo.ext
```
Búsqueda dentro del comando 'less':
/palabra + enter -> buscar coincidencias hacia delante.
?palabra + enter -> buscar coincidencias hacia atrás.
g -> Ir al principio del documento.
G -> Ir al final del documento.
:10 + g -> Ir a un número de línea concreto.

**Primeras y últimas líneas**
```shell
# Mostrar las 10 primeras líneas
head file.txt
# Mostrar una cantidad de líneas (5 por ejemplo):
head -n5 file.txt

# Mostrar las 10 últimas líneas, funciones como head
tail file.txt

# Conocer quién ha sido el último usuario registrado
tail -1 /etc/passwd
# Quien fue el primer usuario
head -n1 /etc/passwd

# Ejemplo de como monitorizar un archivo log (paquetes en este caso)
tail -f /var/log/dpkg.log  # CTRL + c para salir de este
```

**Filtrar texto**
`grep` -> Muestra sólo las líneas que cumplen con un patrón.
- `-i` -> No distingue entre mayúscula y minúscula.
- `-w` -> El patrón tiene que ser una palabra exacta.
- `-v` -> Muestra las que NO coinciden con el patrón.
- `-r` -> Busca en los ficheros de forma recursiva.
- `-n` -> Indica el número de línea.
- `-l` -> Indica solamente el fichero donde ha encontrado alguna coincidencia.
- `-c` -> La cantidad de líneas que cumplen con el patrón.
- `-B n`, `-A n` -> Muestra una cantidad de líneas n, antes (before) o después (after) de encontrar el patrón.
- `--color` -> Destaca el patrón en color dentro de la línea seleccionada.
- `^a` -> Empieza la línea por...
- `u$` -> ...termina la línea por...

Patrón -> `grep -opción palabra /path/archivo`

```shell
# Buscar una palabra en TODO los archivos de un directorio sin ir a niveles por debajo
grep root /etc/* 2> /dev/null  # enviar errores a nulo

# Buscar teniendo presente los subdirectorios (recursivo)
grep -r contrib /etc/* 2> /dev/null

# Buscar también la siguiente línea de la coincidencia
grep -w -A 1 1000 /etc/passwd

# Buscar también la anterior línea de la coincidencia
grep -w -B 1 1000 /etc/passwd
```

`cut` -> Corte "vertical", sólo una parte de cada línea.
- `-c` -> Selecciona sólo los caracteres que le indiquemos. Se pueden hacer delimitaciones.
	- `cut -c 5,10 file.txt` // `cut -c 7-25 file.txt`
- `-d` -> Indica un carácter separador entre los distintos campos de una línea. Por defecto buscará el tabulador.
- `-f` -> Elegir los campos que queremos que se muestren.

[Regresar a índice](#índice)

---

## Tuberías y redirecciones
### Redirecciones
``` shell
# Redirigir la salida del comando hacia un texto
echo "Hola mundo" > texto.txt
# Comprobar
cat texto.txt

# Si el archivo ya existe, añadir la salida al final de este.
echo "Hello world" >> texto.txt
```

Redirigir mensajes de *error* -> `2>`
Redirigir *todos* los mensajes -> `&>`
Desechar los resultados aquí -> `/dev/null`

### Tuberías
Redirigir el resultado hacia otro comando. Se pueden ir concatenando.
Se le llama "tubería" (pipe) ya que establece un "canal" por el que pasará el texto de un comando a otro.
``` shell
# Ejemplos
echo "HOLA MUNDO" | wc

grep -w ana /etc/passwd | cut -d":" -f1 | sort

ls -lr /etc/ | less

ls -lR /etc/ 2> /dev/null | grep shadow

ps -e | grep as
```

[Regresar a índice](#índice)

---
## Acciones sobre texto
### Ordenar con SORT
- Ordena de forma alfabética por defecto.

| Opciones | Tipo orden                         | Ejemplo                                                                                            |
| :------: | ---------------------------------- | -------------------------------------------------------------------------------------------------- |
|   `-r`   | Inversa                            | `sort -r archivo.txt`                                                                              |
|   `-t`   | Carácter de separación de columnas | `sort -t "," 2 archivo.txt`<br>Divide en el número de columnas dependiendo de cuantas "," existen. |
|   `-k`   | Columnas específicas               | `sort -t "," -k 2 archivo.txt`<br>Escoger la segunda columna de la división.                       |
|   `-n`   | Numéricamente                      | `sort -t "," -k 2 -n archivo.txt`                                                                  |
|          |                                    |                                                                                                    |
|   `-u`   | Eliminar líneas duplicadas         | `sort -u archivo.txt`                                                                              |

### Comando UNIQ
- Elimina las líneas repetidas contiguas en un archivo o entrada estàndar.

| Opciones | Tipo orden                         | Ejemplo                                           |
| :------: | ---------------------------------- | ------------------------------------------------- |
|   `-i`   | Ignora mayúsculas y minúsculas     | `cut -d"," -f1 nombres.txt \| sort \| uniq -i`    |
|   `-c`   | Cuenta la cantidad de repeticiones | `cut -d"," -f1 nombres.txt \| sort \| uniq -i -c` |
|   `-u`   | Muestra las no duplicadas          | `cut -d"," -f1 nombres.txt \| sort \| uniq -i -u` |

### Comando WC - Contar
- Contar líneas, palabras y bytes en archivos de texto o entradas estándar:
	- {Líneas} {Palabras} {Bytes}
		- Hay quien dice que el apartado de bytes se refiere al número de caracteres.
		- No es cierto porque si hacemos la comprobación con la "ñ", veremos que aumentan los bytes. -> consultar la pág. man de wc

| Opciones | Tipo orden |
| :------: | ---------- |
|   `-l`   | Líneas     |
|   `-w`   | Palabras   |
|   `-c`   | Bytes      |
|   `-m`   | Caracteres |

Ejemplos:
```shell
# Entrada estándar
echo "Hola" | wc

# Fichero
wc /etc/passwd
```

### Comando REV
Invertir el order de los caracteres en cada línea de un archivo de texto o entrada estándar.
- Útil cuando una línea se puede dividir en columnas y quiero convertir la última en la primera.

Ejemplos:
```shell
# Listado de todos los directorios
ls -lR ficheros | grep '/' | grep -v ^l | rev | cut -d"/" -f1 | rev
```

[Regresar a índice](#índice)

---

## Otros comandos para ficheros

### File -> Tipo de archivo
```shell
# Mostrar solo la descripción del tipo de archivo, su codificación.
file -b <archivo>

# Muestra el MIME del archivo.
file -i <archivo>

# Lee los nombres de los archivos desde un fichero
file -f <archivo_con_nombres>

# Ver los archivos y los tipos de cada uno en el directorio
file *
```

### LN -> Crear enlaces
Dos tipos:
- **Duro** -> Un puntero a la información del disco duro.
	- Aunque veamos que su peso es idéntico al archivo original, no es así.
		- Es un puntero a un archivo original.
		- Si borramos los enlaces, el peso seguirá siendo el mismo.
	- Comando `ls -lh`, la columna que hay después de los permisos, nos muestra el número de enlaces duros que tiene el archivo.
- **Blando** -> Se sustituye por la ruta del fichero original. Parecido a tener un "alias". Como un acceso directo.
	- Cuidado con poner rutas relativas sobretodo al mover archivos.

```shell
# Enlace duro:
ln <archivo> <ruta_y_nombre_archivo>

# Comprobar detalles del archivo
stat <archivo>

# Crear un enlace blando
ln -s <ruta_absoluta_archivo_original> <ruta_acceso_directo_archivo>
```

### DU -> Disk Usage
Espacio de disco ocupado por un fichero o por un directorio. Si le pasamos un directorio mostrará recursivamente el tamaño de todos los subdirectorios.
```shell
# Mostrar el tamaño del directorio y sus subdirectorios
du <ruta_directorio>

# Mostrar el tamaño en lectura humana
du -h

# Mostrar la cantidad total de lo que ocupa el directorio
du -s

# Mostrar la cantidad de todo lo que hay dentro del directorio
du -sh *
# Se puede concatenar con sort
du -sh * | sort -hr
```

### DF -> Disk Free
Información sobre las particiones del sistema: tamaño total, usado y libre.
```shell
# Ver el espacio disponible en el disco y sus particiones
df -h

# Verlo por tipo
df -hT

# Ver el tamaño de la partición donde está ubicado un archivo
df -h <ruta_archivo>
```
**Alternativa a DF**
```shell
sudo apt install dfc
# Ejecución
dfc
```

[Regresar a índice](#índice)

---

## Búsqueda en el sistema de ficheros
### LOCATE - Una primera búsqueda
```shell
# Instalar el paquete
sudo apt install locate

# Actualizar el sistema de ficheros con una base de datos para utilizar locate
su -
# Si se crea un archivo, un directorio, etc., actualizar la BD
updatedb

# Sintaxis locate
locate [opciones] [patrón]
```

| Opciones | Descripción                                    |
| :------: | ---------------------------------------------- |
|   `-i`   | Ignorar mayúsculas y minúsculas en la búsqueda |
|   `-c`   | Contar el número de coincidencias encontradas  |
|   `-n`   | Limitar el número de resultados.               |
|   `-r`   | Usar expresiones regulares para la búsqueda.   |

Ejemplos:
```shell
# Ignorando may y minus
locate -i "sources.list"

# Utilizando regex
locate -r "/sources.list$"
```

### Comando FIND
>[!danger]
>Comando super importante de dominar como administrador.

- Busca de forma recursiva en un directorio todos los ficheros que cumplan ciertas condiciones.
Sintaxis:
```shell
find [ruta] [opciones] [acción]
```

|       Opciones        | Descripción                                                                                                                                                                                                                                   |
| :-------------------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|        `-name`        | Especificar patrones para los nombres de los ficheros a buscar.                                                                                                                                                                               |
|       `-iname`        | Igual que `name` pero sin diferencias mayúsculas y minúsculas.                                                                                                                                                                                |
|        `-type`        | Tipo de fichero a buscar:<br>`d`-> directorios<br>`f` -> ficheros<br>`l` -> enlaces simbólicos<br>`b` -> dispositivos de bloque<br>`c` -> dispositivos de carácter<br>`p` -> para tuberías<br>`s` -> para sockets.                            |
|    `-size +/- <n>`    | `c` -> bytes<br>`k` -> kilobytes<br>`M` -> megabytes<br>`G` -> gigabytes                                                                                                                                                                      |
|     `-perm <num>`     | Ficheros cuyos permisos sean los expresados exactamente por número.<br>`-720` -> El signo `-` actuará como un AND: quiero exactamente esos permisos.<br>`/111` -> El signo `/` actuará como un OR: uno de los tres números debe de coincidir. |
|   `-user <usuario>`   | Especifica el usuario propietario del fichero                                                                                                                                                                                                 |
|  `-group <usuario>`   | Especifica el grupo propietario del fichero                                                                                                                                                                                                   |
|    `-mmin [+/-]n`     | Los datos del fichero fueron modificados por última vez hace `n` minutos.                                                                                                                                                                     |
|    `-mtime [+/-]n`    | Los datos del fichero fueron modificados por última vez hace `n` días.                                                                                                                                                                        |
| `-maxdepth <niveles>` | Desciende el número entero de niveles positivos en la jerarquía del directorio.                                                                                                                                                               |
|        `-path`        | Buscar por toda la ruta de los elementos. Se pueden utilizar comodines.                                                                                                                                                                       |
|       `-regex`        | Igual que `-path`, pero usando expresiones regulares.                                                                                                                                                                                         |

| Modificadores | Descripción                                                                        |
| :-----------: | ---------------------------------------------------------------------------------- |
|    `-not`     | Niega la condición que pongamos a continuación                                     |
 -> MD5
		- `$2a# Comandos de Linux: desde cero hasta programar Shell Script

## Índice
- [Games to practice commands](#games-to-practice-commands)
- [Comandos del sistema de ficheros](#comandos-del-sistema-de-ficheros)
  - [Crear, mover, copiar y borrar ficheros y directorios](#crear-mover-copiar-y-borrar-ficheros-y-directorios)
  - [Atajos en la Shell](#atajos-en-la-shell)
  - [Uso de comodines](#uso-de-comodines)
- [Usuarios y Permisos](#usuarios-y-permisos)
  - [Permisos](#permisos)
    - [Cambiar permisos](#cambiar-permisos)
    - [Permisos usando números](#permisos-usando-números)
- [Administración de software](#administración-de-software)
  - [Comandos de gestión de software](#comandos-de-gestión-de-software)
- [Manejo de texto](#manejo-de-texto)
  - [Mostrar y filtrar texto](#mostrar-y-filtrar-texto)
- [Acciones sobre texto](#acciones-sobre-texto)
  - [Ordenar con SORT](#ordenar-con-sort)
  - [Comando UNIQ](#comando-uniq)
  - [Comando WC - Contar](#comando-wc---contar)
  - [Comando REV](#comando-rev)
- [Otros comandos para ficheros](#otros-comandos-para-ficheros)
  - [File -> Tipo de archivo](#file---tipo-de-archivo)
  - [LN -> Crear enlaces](#ln---crear-enlaces)
  - [DU -> Disk Usage](#du---disk-usage)
  - [DF -> Disk Free](#df---disk-free)
- [Búsqueda en el sistema de ficheros](#búsqueda-en-el-sistema-de-ficheros)
  - [LOCATE - Una primera búsqueda](#locate---una-primera-búsqueda)
  - [Comando FIND](#comando-find)
- [Ampliando conocimientos del Shell BASH](#ampliando-conocimientos-del-shell-bash)
  - [ALIAS](#alias)
  - [Sustitución de comandos](#sustitución-de-comandos)
- [Backups](#backups)
  - [Compactar y comprimir TAR](#compactar-y-comprimir-tar)
  - [Copias de seguridad totales, diferenciales e incrementales usando TAR](#copias-de-seguridad-totales-diferenciales-e-incrementales-usando-tar)
- [Gestión de paquetes con dpkg](#gestión-de-paquetes-con-dpkg)
- [Administración de Usuarios, Grupos y Contraseñas](#administración-de-usuarios-grupos-y-contraseñas)
  - [Usuarios](#usuarios)
  - [Contraseñas](#contraseñas)
  - [Grupos](#grupos)
  - [Cambio de propietario o grupo de un elemento](#cambio-de-propietario-o-grupo-de-un-elemento)
  - [Seguridad](#seguridad)
  - [Permisos especiales](#permisos-especiales)
- [Manejo de texto avanzado](#manejo-de-texto-avanzado)
  - [Comando TR](#comando-tr)
  - [Comando SED](#comando-sed)
- [Expresiones Regulares (Regex)](#expresiones-regulares-regex)

---
## Vídeos en YouTube
[Scheduling Tasks with Cron](https://youtu.be/7cbP7fzn0D8?si=Gk-bly6b5wPeRtsB)
[AWK](https://youtu.be/oPEnvuj9QrI?si=QESU7XS5yAe_TvqO)

---
## Games to practice commands
[Over the wire](https://overthewire.org/wargames/bandit/bandit0.html)
[Linux Survival](https://linuxsurvival.com)
[Practice Linux](https://www.practicelinux.com/home)

---
## Comandos del sistema de ficheros

```shell
# Ir al home del usuario
cd
cd ~
# Subir un nivel
cd ..
# Dos niveles
cd ../..

# Mostrar el contenido de uno o varios directorios
ls
# Format llistat
ls -l
# Pes en format llegible
ls -h
# Ordenar por el archivo + pesado
ls -S
# Ordenado al revés
ls -r
# Ordenado por archivo editado primero
ls -t
# Mostrar archivos ocultos
ls -a
```

Conocer el tipo de comando que utilizamos: `type comando`
Posibles resultados:
- orden interna -> built-in, como `cd`
- ejecutable -> /usr/bin/comando
- alias -> apodo de un comando más extenso

Cambiar a superusuario en Debian -> `su -`

### Crear, mover, copiar y borrar ficheros y directorios
```shell
# Crear carpeta
mkdir carpeta
# Crear dentro de un directorio ya existente
mkdir /ruta/carpeta/
# Crear carpetas padre -> hijo
mkdir -p carpeta/carpeta/

# Crear un fichero vacío
touch mi_fichero
# Alternativa
> mi_fichero2

# Mover o renombrar
mv mi_fichero mi_carta  # Renombra
mv mi_carta Documentos/  # Mueve y se podría renombrar

# Copiar: necesita dos parámetros
cp archivo_ruta_origen archivo_ruta_destino
# Copiar directorio y todo su contenido
cp -r directorio_ruta_origen directorio_ruta_destino
# Copiar varios elementos
cp fichero fichero ~  # Pega en la carpeta home

# Borrar fichero
rm fichero
# Borrar directorio vacío
rmdir directorio
# Borrar directorio y su contenido
rm -r directorio

# Ver la jerarquía de archivos y documentos
tree
tree ruta/
```

>[!tip] Copia de archivos que pertenecen a root
>Archivos que pertenecen a root podrían llegar a ser copiados por otros usuarios y luego llegar a ver el contenido que antes no se les permitía.
>
>En cambio si se mueve, el propietario se respeta y con los respectivos permisos.

### Atajos en la Shell

| Atajo                   | Descripción                                    |
| ----------------------- | ---------------------------------------------- |
| Cursores arriba y abajo | Historial de instrucciones                     |
| Tab                     | Completar la ruta de un elemento del sistema   |
| Doble Tab               | Opciones para el auto completado               |
| Ctrl + r                | Búsqueda de instrucciones                      |
| Ctrl + c                | Terminar un comando ejecutado a medias         |
| `history`               | Listado de comandos ejecutados                 |
| `!número`               | Llamar a uno de los comandos de `history`      |
| `sudo !!`               | Ejecutar la última instrucción con privilegios |
| Ctrl + a                | Ir al inicio del comando escrito               |
| Ctrl + e                | Ir al final del comando escrito                |
| Ctrl + l                | borrar la pantalla                             |

### Uso de comodines
Asterisco -> "Cualquier cosa"
```shell
# Hacer referencia a los archivos que tienen una extensión en común, ejemplo:
rm *.gif
# Todo!
rm *

# Cualquier archivo o carpeta cuyo nombre contenga la letra 'u'
cp *u* seleccion/
# O que además en la extensión tengan una 'i'
cp *u*.*i* seleccion/
# Que por lo menos exista una extensión (ficheros)
cp *.*
```

Interrogación -> "Cualquier carácter y cualquier longitud de cadena de caracteres o ninguna"
```shell
# Caracteres obligatorios, cualquier carácter OBLIGATORIO en la posición y lo que siga O NO.
ls c?sa*
# Obligación de una cifra
ls informe-200?.txt
ls informe-19??.txt
# Cualquier longitud de nombre hasta el punto + obligación en el último carácter de la extensión
ls c*.tx?
```

Corchetes -> opciones obligatorias en una posición
```shell
# Elige cualquiera de los tres caracteres marcados en corchetes.
ls c[aio]s*

# Rangos
ls c[a-m]s*
# Mayúsculas
ls c[A-Z]s*
# Números
ls informe-200[1-9].txt
```

> [!tip]
> Resultará útil consultar `man bash` y buscar `Concordancia`:
> /Concordancia + n (seguir buscando hasta llegar a la sección)

[Regresar a índice](#índice)

---
## Usuarios y Permisos
Cuando se crea un usuario, si no se especifica, se crea un grupo con el mismo nombre.

Quién soy y a qué grupos pertenezco -> `id`

| Características | Descripción                                       |
| --------------- | ------------------------------------------------- |
| uid             | identificador del usuario                         |
| gid             | grupo principal al que pertenece el usuario       |
| grupos          | grupos secundarios a los que pertenece el usuario |

Comandos básicos de ejecución:

| Comando                | Descripción                   |
| ---------------------- | ----------------------------- |
| `su usuario`           | Cambio de usuario             |
| `su root -c "comando"` | Ejecutar un comando como root |
| `passwd`               | Cambiar la contraseña         |

Se necessita ser _root_ para crear usuarios y grupos:

| Comando                                      | Descripción                                       |
| -------------------------------------------- | ------------------------------------------------- |
| `groupadd nombre_grupo`                      | Crear grupo                                       |
| `useradd -m nombre_usuario`                  | Crear usuario con su propio grupo                 |
| `useradd -m -g grupo_asignar nombre_usuario` | Crear usuario y asignar a un grupo                |
| `adduser nombre_usuario`                     | Crear usuario con prompts administrativos         |
| `userdel -r nombre_usuario`                  | Eliminar usuario y su directorio home             |
| `groupdel nombre_grupo`                      | Eliminar el grupo (asegurarse que no está en uso) |

### Permisos

| Permisos | Descripción                         |
| -------- | ----------------------------------- |
| `-`      | No tiene permisos                   |
| `r`      | read -> puede ver pero no modificar |
| `w`      | write -> puede modificar            |
| `x`      | execute -> puede ejecutar           |

`-rwxrwxrwx`: permisos por posiciones
- 1r grupo: permisos de usuario
- 2o grupo: permisos del grupo propietario del fichero
- 3r grupo: permisos del resto de usuarios del sistema

|                |   R    |                   W                   |                  X                   |
| -------------- | :----: | :-----------------------------------: | :----------------------------------: |
| **Fichero**    |  Leer  |               Escribir                | Ejecutar<br>(programa, script, etc.) |
| **Directorio** | Listar | Modificar el contenido del directorio |               Acceder                |

#### Cambiar permisos
`chmod`
- Letra del permiso. Si son varios permisos, separarlas por comas.
	- Si no se pone nada, se realiza en todos.
- + para añadir permiso / - para quitar permiso
- Ruta del elemento a cambiar permisos
- Indicación de a quién asignarlo: u=User, g=Group, o=Others

Ejemplos:
```shell
# Añadir permiso de escritura al grupo
chmod g+w ruta

# Quitar el permiso de ejecución a todos
chmod -x ruta

# Añadir permiso de escritura a usuario y a otros
chmod u+w,o+w ruta

# Quitar el permiso de ejecución y escritura al usuario y poner el de escritura al grupo
chmod u-xw,g+w ruta
```

#### Permisos usando números

| Permission Type | Read (4) | Write (2) | Execute (1) |
| --------------- | -------- | --------- | ----------- |
| User            | 1 / 0    | 1 / 0     | 1 / 0       |
| Group           | 1 / 0    | 1 / 0     | 1 / 0       |
| Others          | 1 / 0    | 1 / 0     | 1 / 0       |

Si quiero poner permisos al usuario de lectura y escritura, al grupo sólo lectura y al resto nada:
- Binario -> `110 100 000`
- Decimal -> `640`
- Comando -> `chmod 640 fichero`
En este tipo de cambio, actuará en los tres grupos.

[Regresar a índice](#índice)

---

## Administración de software
En Debian 12, se ha realizado el cambio de que en una instalación limpia se consiguen los paquetes a través de la ISO.
Hasta el día de hoy Debian utilizaba un solo archivo para obtener los repositorios -> `/etc/apt/sources.list`.
A partir de Debian 12 se ha modularizado -> `/etc/sources.list.d/`
Y se debe de añadir un servicio CDN para que funcione el comando APT.

>[!info]
>Es importante conocer el nombre en clave de la distribución -> Debian 12 = Bookworm

**Cómo añadir las fuentes para el APT en un archivo**
[Instrucciones de la web oficial de Debian -> 4.3.1](https://www.debian.org/releases/trixie/release-notes/upgrading.es.html#adding-apt-internet-sources)
1. Nos dirigimos a `/etc/apt/sources.list`
2. Comentamos la linea que especifica que utilizará los paquetes del CD.
3. Añadimos la siguiente linea: `deb https://deb.debian.org/debian bookworm main contrib`

**Cómo añadir las fuentes APT de manera modularizada**
1. Comentamos `/etc/apt/sources.list`
2. `nano /etc/apt/sources.list.d/repo_oficial.list
3. Añadimos `deb https://deb.debian.org/debian bookworm main contrib`

### Comandos de gestión de software

| `apt`        | Descripción                                                                                                    |
| ------------ | -------------------------------------------------------------------------------------------------------------- |
| `update`     | Actualiza la lista de paquetes disponibles en los repositorios                                                 |
| `search`     | Busca paquetes disponibles en los repositorios que coincidan con un término de búsqueda.                       |
| `show`       | Muestra información detallada sobre un paquete específico.                                                     |
| `list`       | Lista paquetes según criterios, como los instalados, actualizables o que cumplen cierta condición de búsqueda. |
| `install`    | Instala un paquete de software en el sistema.                                                                  |
| `remove`     | Elimina un paquete del sistema, manteniendo los archivos de configuración.                                     |
| `purge`      | Elimina un paquete del sistema junto con sus archivos de configuración.                                        |
| `autoremove` | Elimina paquetes que no tienen dependencias.                                                                   |

[Regresar a índice](#índice)

---
## Manejo de texto
>[!warning]
>**¿Por qué es importante?**
>Configuraciones se guardan en ficheros de texto.
>Los logs también son en formato texto.
>Cualquier programa en modo consola que muestra resultados mediante texto.

>[!info]
>Dominar esto ayuda muchísimo como administrador del servidor.

### Mostrar y filtrar texto

**Mostrar texto**
```shell
# Mostrar texto que se recibe
echo "Hola Mundo"

# Interpretar ordenes:
# Salto de línea
echo -e "Hola \nMundo"
# Tabulación
echo -e "Hola\tMundo"

# Ver las variables del sistema
echo $PATH
echo "Soy el usuario: $USER"

# Las comillas simples no interpretarán variables:
echo 'La variable del sistema que guarda el usuario es: $USER'

# Mostrar un fichero de texto
cat /etc/issue
# Mostrar con número de línea en el archivo
cat -n /etc/fstab
```

**Mostrar contenido de ficheros de texto**
```shell
# Mostrar el texto poco a poco -> 'h' muestra la ayuda
more archivo.ext
# Su mejora para los archivos grandes y con una "ventana nueva"
less archivo.ext
```
Búsqueda dentro del comando 'less':
/palabra + enter -> buscar coincidencias hacia delante.
?palabra + enter -> buscar coincidencias hacia atrás.
g -> Ir al principio del documento.
G -> Ir al final del documento.
:10 + g -> Ir a un número de línea concreto.

**Primeras y últimas líneas**
```shell
# Mostrar las 10 primeras líneas
head file.txt
# Mostrar una cantidad de líneas (5 por ejemplo):
head -n5 file.txt

# Mostrar las 10 últimas líneas, funciones como head
tail file.txt

# Conocer quién ha sido el último usuario registrado
tail -1 /etc/passwd
# Quien fue el primer usuario
head -n1 /etc/passwd

# Ejemplo de como monitorizar un archivo log (paquetes en este caso)
tail -f /var/log/dpkg.log  # CTRL + c para salir de este
```

**Filtrar texto**
`grep` -> Muestra sólo las líneas que cumplen con un patrón.
- `-i` -> No distingue entre mayúscula y minúscula.
- `-w` -> El patrón tiene que ser una palabra exacta.
- `-v` -> Muestra las que NO coinciden con el patrón.
- `-r` -> Busca en los ficheros de forma recursiva.
- `-n` -> Indica el número de línea.
- `-l` -> Indica solamente el fichero donde ha encontrado alguna coincidencia.
- `-c` -> La cantidad de líneas que cumplen con el patrón.
- `-B n`, `-A n` -> Muestra una cantidad de líneas n, antes (before) o después (after) de encontrar el patrón.
- `--color` -> Destaca el patrón en color dentro de la línea seleccionada.
- `^a` -> Empieza la línea por...
- `u$` -> ...termina la línea por...

Patrón -> `grep -opción palabra /path/archivo`

```shell
# Buscar una palabra en TODO los archivos de un directorio sin ir a niveles por debajo
grep root /etc/* 2> /dev/null  # enviar errores a nulo

# Buscar teniendo presente los subdirectorios (recursivo)
grep -r contrib /etc/* 2> /dev/null

# Buscar también la siguiente línea de la coincidencia
grep -w -A 1 1000 /etc/passwd

# Buscar también la anterior línea de la coincidencia
grep -w -B 1 1000 /etc/passwd
```

`cut` -> Corte "vertical", sólo una parte de cada línea.
- `-c` -> Selecciona sólo los caracteres que le indiquemos. Se pueden hacer delimitaciones.
	- `cut -c 5,10 file.txt` // `cut -c 7-25 file.txt`
- `-d` -> Indica un carácter separador entre los distintos campos de una línea. Por defecto buscará el tabulador.
- `-f` -> Elegir los campos que queremos que se muestren.

[Regresar a índice](#índice)

---

## Tuberías y redirecciones
### Redirecciones
``` shell
# Redirigir la salida del comando hacia un texto
echo "Hola mundo" > texto.txt
# Comprobar
cat texto.txt

# Si el archivo ya existe, añadir la salida al final de este.
echo "Hello world" >> texto.txt
```

Redirigir mensajes de *error* -> `2>`
Redirigir *todos* los mensajes -> `&>`
Desechar los resultados aquí -> `/dev/null`

### Tuberías
Redirigir el resultado hacia otro comando. Se pueden ir concatenando.
Se le llama "tubería" (pipe) ya que establece un "canal" por el que pasará el texto de un comando a otro.
``` shell
# Ejemplos
echo "HOLA MUNDO" | wc

grep -w ana /etc/passwd | cut -d":" -f1 | sort

ls -lr /etc/ | less

ls -lR /etc/ 2> /dev/null | grep shadow

ps -e | grep as
```

[Regresar a índice](#índice)

---
## Acciones sobre texto
### Ordenar con SORT
- Ordena de forma alfabética por defecto.

| Opciones | Tipo orden                         | Ejemplo                                                                                            |
| :------: | ---------------------------------- | -------------------------------------------------------------------------------------------------- |
|   `-r`   | Inversa                            | `sort -r archivo.txt`                                                                              |
|   `-t`   | Carácter de separación de columnas | `sort -t "," 2 archivo.txt`<br>Divide en el número de columnas dependiendo de cuantas "," existen. |
|   `-k`   | Columnas específicas               | `sort -t "," -k 2 archivo.txt`<br>Escoger la segunda columna de la división.                       |
|   `-n`   | Numéricamente                      | `sort -t "," -k 2 -n archivo.txt`                                                                  |
|          |                                    |                                                                                                    |
|   `-u`   | Eliminar líneas duplicadas         | `sort -u archivo.txt`                                                                              |

### Comando UNIQ
- Elimina las líneas repetidas contiguas en un archivo o entrada estàndar.

| Opciones | Tipo orden                         | Ejemplo                                           |
| :------: | ---------------------------------- | ------------------------------------------------- |
|   `-i`   | Ignora mayúsculas y minúsculas     | `cut -d"," -f1 nombres.txt \| sort \| uniq -i`    |
|   `-c`   | Cuenta la cantidad de repeticiones | `cut -d"," -f1 nombres.txt \| sort \| uniq -i -c` |
|   `-u`   | Muestra las no duplicadas          | `cut -d"," -f1 nombres.txt \| sort \| uniq -i -u` |

### Comando WC - Contar
- Contar líneas, palabras y bytes en archivos de texto o entradas estándar:
	- {Líneas} {Palabras} {Bytes}
		- Hay quien dice que el apartado de bytes se refiere al número de caracteres.
		- No es cierto porque si hacemos la comprobación con la "ñ", veremos que aumentan los bytes. -> consultar la pág. man de wc

| Opciones | Tipo orden |
| :------: | ---------- |
|   `-l`   | Líneas     |
|   `-w`   | Palabras   |
|   `-c`   | Bytes      |
|   `-m`   | Caracteres |

Ejemplos:
```shell
# Entrada estándar
echo "Hola" | wc

# Fichero
wc /etc/passwd
```

### Comando REV
Invertir el order de los caracteres en cada línea de un archivo de texto o entrada estándar.
- Útil cuando una línea se puede dividir en columnas y quiero convertir la última en la primera.

Ejemplos:
```shell
# Listado de todos los directorios
ls -lR ficheros | grep '/' | grep -v ^l | rev | cut -d"/" -f1 | rev
```

[Regresar a índice](#índice)

---

## Otros comandos para ficheros

### File -> Tipo de archivo
```shell
# Mostrar solo la descripción del tipo de archivo, su codificación.
file -b <archivo>

# Muestra el MIME del archivo.
file -i <archivo>

# Lee los nombres de los archivos desde un fichero
file -f <archivo_con_nombres>

# Ver los archivos y los tipos de cada uno en el directorio
file *
```

### LN -> Crear enlaces
Dos tipos:
- **Duro** -> Un puntero a la información del disco duro.
	- Aunque veamos que su peso es idéntico al archivo original, no es así.
		- Es un puntero a un archivo original.
		- Si borramos los enlaces, el peso seguirá siendo el mismo.
	- Comando `ls -lh`, la columna que hay después de los permisos, nos muestra el número de enlaces duros que tiene el archivo.
- **Blando** -> Se sustituye por la ruta del fichero original. Parecido a tener un "alias". Como un acceso directo.
	- Cuidado con poner rutas relativas sobretodo al mover archivos.

```shell
# Enlace duro:
ln <archivo> <ruta_y_nombre_archivo>

# Comprobar detalles del archivo
stat <archivo>

# Crear un enlace blando
ln -s <ruta_absoluta_archivo_original> <ruta_acceso_directo_archivo>
```

### DU -> Disk Usage
Espacio de disco ocupado por un fichero o por un directorio. Si le pasamos un directorio mostrará recursivamente el tamaño de todos los subdirectorios.
```shell
# Mostrar el tamaño del directorio y sus subdirectorios
du <ruta_directorio>

# Mostrar el tamaño en lectura humana
du -h

# Mostrar la cantidad total de lo que ocupa el directorio
du -s

# Mostrar la cantidad de todo lo que hay dentro del directorio
du -sh *
# Se puede concatenar con sort
du -sh * | sort -hr
```

### DF -> Disk Free
Información sobre las particiones del sistema: tamaño total, usado y libre.
```shell
# Ver el espacio disponible en el disco y sus particiones
df -h

# Verlo por tipo
df -hT

# Ver el tamaño de la partición donde está ubicado un archivo
df -h <ruta_archivo>
```
**Alternativa a DF**
```shell
sudo apt install dfc
# Ejecución
dfc
```

[Regresar a índice](#índice)

---

## Búsqueda en el sistema de ficheros
### LOCATE - Una primera búsqueda
```shell
# Instalar el paquete
sudo apt install locate

# Actualizar el sistema de ficheros con una base de datos para utilizar locate
su -
# Si se crea un archivo, un directorio, etc., actualizar la BD
updatedb

# Sintaxis locate
locate [opciones] [patrón]
```

| Opciones | Descripción                                    |
| :------: | ---------------------------------------------- |
|   `-i`   | Ignorar mayúsculas y minúsculas en la búsqueda |
|   `-c`   | Contar el número de coincidencias encontradas  |
|   `-n`   | Limitar el número de resultados.               |
|   `-r`   | Usar expresiones regulares para la búsqueda.   |

Ejemplos:
```shell
# Ignorando may y minus
locate -i "sources.list"

# Utilizando regex
locate -r "/sources.list$"
```

### Comando FIND
>[!danger]
>Comando super importante de dominar como administrador.

- Busca de forma recursiva en un directorio todos los ficheros que cumplan ciertas condiciones.
Sintaxis:
```shell
find [ruta] [opciones] [acción]
```

|       Opciones        | Descripción                                                                                                                                                                                                                                   |
| :-------------------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|        `-name`        | Especificar patrones para los nombres de los ficheros a buscar.                                                                                                                                                                               |
|       `-iname`        | Igual que `name` pero sin diferencias mayúsculas y minúsculas.                                                                                                                                                                                |
|        `-type`        | Tipo de fichero a buscar:<br>`d`-> directorios<br>`f` -> ficheros<br>`l` -> enlaces simbólicos<br>`b` -> dispositivos de bloque<br>`c` -> dispositivos de carácter<br>`p` -> para tuberías<br>`s` -> para sockets.                            |
|    `-size +/- <n>`    | `c` -> bytes<br>`k` -> kilobytes<br>`M` -> megabytes<br>`G` -> gigabytes                                                                                                                                                                      |
|     `-perm <num>`     | Ficheros cuyos permisos sean los expresados exactamente por número.<br>`-720` -> El signo `-` actuará como un AND: quiero exactamente esos permisos.<br>`/111` -> El signo `/` actuará como un OR: uno de los tres números debe de coincidir. |
|   `-user <usuario>`   | Especifica el usuario propietario del fichero                                                                                                                                                                                                 |
|  `-group <usuario>`   | Especifica el grupo propietario del fichero                                                                                                                                                                                                   |
|    `-mmin [+/-]n`     | Los datos del fichero fueron modificados por última vez hace `n` minutos.                                                                                                                                                                     |
|    `-mtime [+/-]n`    | Los datos del fichero fueron modificados por última vez hace `n` días.                                                                                                                                                                        |
| `-maxdepth <niveles>` | Desciende el número entero de niveles positivos en la jerarquía del directorio.                                                                                                                                                               |
|        `-path`        | Buscar por toda la ruta de los elementos. Se pueden utilizar comodines.                                                                                                                                                                       |
|       `-regex`        | Igual que `-path`, pero usando expresiones regulares.                                                                                                                                                                                         |

| Modificadores | Descripción                                                                        |
| :-----------: | ---------------------------------------------------------------------------------- |
|    `-not`     | Niega la condición que pongamos a continuación                                     |
 o `$2y# Comandos de Linux: desde cero hasta programar Shell Script

## Índice
- [Games to practice commands](#games-to-practice-commands)
- [Comandos del sistema de ficheros](#comandos-del-sistema-de-ficheros)
  - [Crear, mover, copiar y borrar ficheros y directorios](#crear-mover-copiar-y-borrar-ficheros-y-directorios)
  - [Atajos en la Shell](#atajos-en-la-shell)
  - [Uso de comodines](#uso-de-comodines)
- [Usuarios y Permisos](#usuarios-y-permisos)
  - [Permisos](#permisos)
    - [Cambiar permisos](#cambiar-permisos)
    - [Permisos usando números](#permisos-usando-números)
- [Administración de software](#administración-de-software)
  - [Comandos de gestión de software](#comandos-de-gestión-de-software)
- [Manejo de texto](#manejo-de-texto)
  - [Mostrar y filtrar texto](#mostrar-y-filtrar-texto)
- [Acciones sobre texto](#acciones-sobre-texto)
  - [Ordenar con SORT](#ordenar-con-sort)
  - [Comando UNIQ](#comando-uniq)
  - [Comando WC - Contar](#comando-wc---contar)
  - [Comando REV](#comando-rev)
- [Otros comandos para ficheros](#otros-comandos-para-ficheros)
  - [File -> Tipo de archivo](#file---tipo-de-archivo)
  - [LN -> Crear enlaces](#ln---crear-enlaces)
  - [DU -> Disk Usage](#du---disk-usage)
  - [DF -> Disk Free](#df---disk-free)
- [Búsqueda en el sistema de ficheros](#búsqueda-en-el-sistema-de-ficheros)
  - [LOCATE - Una primera búsqueda](#locate---una-primera-búsqueda)
  - [Comando FIND](#comando-find)
- [Ampliando conocimientos del Shell BASH](#ampliando-conocimientos-del-shell-bash)
  - [ALIAS](#alias)
  - [Sustitución de comandos](#sustitución-de-comandos)
- [Backups](#backups)
  - [Compactar y comprimir TAR](#compactar-y-comprimir-tar)
  - [Copias de seguridad totales, diferenciales e incrementales usando TAR](#copias-de-seguridad-totales-diferenciales-e-incrementales-usando-tar)
- [Gestión de paquetes con dpkg](#gestión-de-paquetes-con-dpkg)
- [Administración de Usuarios, Grupos y Contraseñas](#administración-de-usuarios-grupos-y-contraseñas)
  - [Usuarios](#usuarios)
  - [Contraseñas](#contraseñas)
  - [Grupos](#grupos)
  - [Cambio de propietario o grupo de un elemento](#cambio-de-propietario-o-grupo-de-un-elemento)
  - [Seguridad](#seguridad)
  - [Permisos especiales](#permisos-especiales)
- [Manejo de texto avanzado](#manejo-de-texto-avanzado)
  - [Comando TR](#comando-tr)
  - [Comando SED](#comando-sed)
- [Expresiones Regulares (Regex)](#expresiones-regulares-regex)

---
## Vídeos en YouTube
[Scheduling Tasks with Cron](https://youtu.be/7cbP7fzn0D8?si=Gk-bly6b5wPeRtsB)
[AWK](https://youtu.be/oPEnvuj9QrI?si=QESU7XS5yAe_TvqO)

---
## Games to practice commands
[Over the wire](https://overthewire.org/wargames/bandit/bandit0.html)
[Linux Survival](https://linuxsurvival.com)
[Practice Linux](https://www.practicelinux.com/home)

---
## Comandos del sistema de ficheros

```shell
# Ir al home del usuario
cd
cd ~
# Subir un nivel
cd ..
# Dos niveles
cd ../..

# Mostrar el contenido de uno o varios directorios
ls
# Format llistat
ls -l
# Pes en format llegible
ls -h
# Ordenar por el archivo + pesado
ls -S
# Ordenado al revés
ls -r
# Ordenado por archivo editado primero
ls -t
# Mostrar archivos ocultos
ls -a
```

Conocer el tipo de comando que utilizamos: `type comando`
Posibles resultados:
- orden interna -> built-in, como `cd`
- ejecutable -> /usr/bin/comando
- alias -> apodo de un comando más extenso

Cambiar a superusuario en Debian -> `su -`

### Crear, mover, copiar y borrar ficheros y directorios
```shell
# Crear carpeta
mkdir carpeta
# Crear dentro de un directorio ya existente
mkdir /ruta/carpeta/
# Crear carpetas padre -> hijo
mkdir -p carpeta/carpeta/

# Crear un fichero vacío
touch mi_fichero
# Alternativa
> mi_fichero2

# Mover o renombrar
mv mi_fichero mi_carta  # Renombra
mv mi_carta Documentos/  # Mueve y se podría renombrar

# Copiar: necesita dos parámetros
cp archivo_ruta_origen archivo_ruta_destino
# Copiar directorio y todo su contenido
cp -r directorio_ruta_origen directorio_ruta_destino
# Copiar varios elementos
cp fichero fichero ~  # Pega en la carpeta home

# Borrar fichero
rm fichero
# Borrar directorio vacío
rmdir directorio
# Borrar directorio y su contenido
rm -r directorio

# Ver la jerarquía de archivos y documentos
tree
tree ruta/
```

>[!tip] Copia de archivos que pertenecen a root
>Archivos que pertenecen a root podrían llegar a ser copiados por otros usuarios y luego llegar a ver el contenido que antes no se les permitía.
>
>En cambio si se mueve, el propietario se respeta y con los respectivos permisos.

### Atajos en la Shell

| Atajo                   | Descripción                                    |
| ----------------------- | ---------------------------------------------- |
| Cursores arriba y abajo | Historial de instrucciones                     |
| Tab                     | Completar la ruta de un elemento del sistema   |
| Doble Tab               | Opciones para el auto completado               |
| Ctrl + r                | Búsqueda de instrucciones                      |
| Ctrl + c                | Terminar un comando ejecutado a medias         |
| `history`               | Listado de comandos ejecutados                 |
| `!número`               | Llamar a uno de los comandos de `history`      |
| `sudo !!`               | Ejecutar la última instrucción con privilegios |
| Ctrl + a                | Ir al inicio del comando escrito               |
| Ctrl + e                | Ir al final del comando escrito                |
| Ctrl + l                | borrar la pantalla                             |

### Uso de comodines
Asterisco -> "Cualquier cosa"
```shell
# Hacer referencia a los archivos que tienen una extensión en común, ejemplo:
rm *.gif
# Todo!
rm *

# Cualquier archivo o carpeta cuyo nombre contenga la letra 'u'
cp *u* seleccion/
# O que además en la extensión tengan una 'i'
cp *u*.*i* seleccion/
# Que por lo menos exista una extensión (ficheros)
cp *.*
```

Interrogación -> "Cualquier carácter y cualquier longitud de cadena de caracteres o ninguna"
```shell
# Caracteres obligatorios, cualquier carácter OBLIGATORIO en la posición y lo que siga O NO.
ls c?sa*
# Obligación de una cifra
ls informe-200?.txt
ls informe-19??.txt
# Cualquier longitud de nombre hasta el punto + obligación en el último carácter de la extensión
ls c*.tx?
```

Corchetes -> opciones obligatorias en una posición
```shell
# Elige cualquiera de los tres caracteres marcados en corchetes.
ls c[aio]s*

# Rangos
ls c[a-m]s*
# Mayúsculas
ls c[A-Z]s*
# Números
ls informe-200[1-9].txt
```

> [!tip]
> Resultará útil consultar `man bash` y buscar `Concordancia`:
> /Concordancia + n (seguir buscando hasta llegar a la sección)

[Regresar a índice](#índice)

---
## Usuarios y Permisos
Cuando se crea un usuario, si no se especifica, se crea un grupo con el mismo nombre.

Quién soy y a qué grupos pertenezco -> `id`

| Características | Descripción                                       |
| --------------- | ------------------------------------------------- |
| uid             | identificador del usuario                         |
| gid             | grupo principal al que pertenece el usuario       |
| grupos          | grupos secundarios a los que pertenece el usuario |

Comandos básicos de ejecución:

| Comando                | Descripción                   |
| ---------------------- | ----------------------------- |
| `su usuario`           | Cambio de usuario             |
| `su root -c "comando"` | Ejecutar un comando como root |
| `passwd`               | Cambiar la contraseña         |

Se necessita ser _root_ para crear usuarios y grupos:

| Comando                                      | Descripción                                       |
| -------------------------------------------- | ------------------------------------------------- |
| `groupadd nombre_grupo`                      | Crear grupo                                       |
| `useradd -m nombre_usuario`                  | Crear usuario con su propio grupo                 |
| `useradd -m -g grupo_asignar nombre_usuario` | Crear usuario y asignar a un grupo                |
| `adduser nombre_usuario`                     | Crear usuario con prompts administrativos         |
| `userdel -r nombre_usuario`                  | Eliminar usuario y su directorio home             |
| `groupdel nombre_grupo`                      | Eliminar el grupo (asegurarse que no está en uso) |

### Permisos

| Permisos | Descripción                         |
| -------- | ----------------------------------- |
| `-`      | No tiene permisos                   |
| `r`      | read -> puede ver pero no modificar |
| `w`      | write -> puede modificar            |
| `x`      | execute -> puede ejecutar           |

`-rwxrwxrwx`: permisos por posiciones
- 1r grupo: permisos de usuario
- 2o grupo: permisos del grupo propietario del fichero
- 3r grupo: permisos del resto de usuarios del sistema

|                |   R    |                   W                   |                  X                   |
| -------------- | :----: | :-----------------------------------: | :----------------------------------: |
| **Fichero**    |  Leer  |               Escribir                | Ejecutar<br>(programa, script, etc.) |
| **Directorio** | Listar | Modificar el contenido del directorio |               Acceder                |

#### Cambiar permisos
`chmod`
- Letra del permiso. Si son varios permisos, separarlas por comas.
	- Si no se pone nada, se realiza en todos.
- + para añadir permiso / - para quitar permiso
- Ruta del elemento a cambiar permisos
- Indicación de a quién asignarlo: u=User, g=Group, o=Others

Ejemplos:
```shell
# Añadir permiso de escritura al grupo
chmod g+w ruta

# Quitar el permiso de ejecución a todos
chmod -x ruta

# Añadir permiso de escritura a usuario y a otros
chmod u+w,o+w ruta

# Quitar el permiso de ejecución y escritura al usuario y poner el de escritura al grupo
chmod u-xw,g+w ruta
```

#### Permisos usando números

| Permission Type | Read (4) | Write (2) | Execute (1) |
| --------------- | -------- | --------- | ----------- |
| User            | 1 / 0    | 1 / 0     | 1 / 0       |
| Group           | 1 / 0    | 1 / 0     | 1 / 0       |
| Others          | 1 / 0    | 1 / 0     | 1 / 0       |

Si quiero poner permisos al usuario de lectura y escritura, al grupo sólo lectura y al resto nada:
- Binario -> `110 100 000`
- Decimal -> `640`
- Comando -> `chmod 640 fichero`
En este tipo de cambio, actuará en los tres grupos.

[Regresar a índice](#índice)

---

## Administración de software
En Debian 12, se ha realizado el cambio de que en una instalación limpia se consiguen los paquetes a través de la ISO.
Hasta el día de hoy Debian utilizaba un solo archivo para obtener los repositorios -> `/etc/apt/sources.list`.
A partir de Debian 12 se ha modularizado -> `/etc/sources.list.d/`
Y se debe de añadir un servicio CDN para que funcione el comando APT.

>[!info]
>Es importante conocer el nombre en clave de la distribución -> Debian 12 = Bookworm

**Cómo añadir las fuentes para el APT en un archivo**
[Instrucciones de la web oficial de Debian -> 4.3.1](https://www.debian.org/releases/trixie/release-notes/upgrading.es.html#adding-apt-internet-sources)
1. Nos dirigimos a `/etc/apt/sources.list`
2. Comentamos la linea que especifica que utilizará los paquetes del CD.
3. Añadimos la siguiente linea: `deb https://deb.debian.org/debian bookworm main contrib`

**Cómo añadir las fuentes APT de manera modularizada**
1. Comentamos `/etc/apt/sources.list`
2. `nano /etc/apt/sources.list.d/repo_oficial.list
3. Añadimos `deb https://deb.debian.org/debian bookworm main contrib`

### Comandos de gestión de software

| `apt`        | Descripción                                                                                                    |
| ------------ | -------------------------------------------------------------------------------------------------------------- |
| `update`     | Actualiza la lista de paquetes disponibles en los repositorios                                                 |
| `search`     | Busca paquetes disponibles en los repositorios que coincidan con un término de búsqueda.                       |
| `show`       | Muestra información detallada sobre un paquete específico.                                                     |
| `list`       | Lista paquetes según criterios, como los instalados, actualizables o que cumplen cierta condición de búsqueda. |
| `install`    | Instala un paquete de software en el sistema.                                                                  |
| `remove`     | Elimina un paquete del sistema, manteniendo los archivos de configuración.                                     |
| `purge`      | Elimina un paquete del sistema junto con sus archivos de configuración.                                        |
| `autoremove` | Elimina paquetes que no tienen dependencias.                                                                   |

[Regresar a índice](#índice)

---
## Manejo de texto
>[!warning]
>**¿Por qué es importante?**
>Configuraciones se guardan en ficheros de texto.
>Los logs también son en formato texto.
>Cualquier programa en modo consola que muestra resultados mediante texto.

>[!info]
>Dominar esto ayuda muchísimo como administrador del servidor.

### Mostrar y filtrar texto

**Mostrar texto**
```shell
# Mostrar texto que se recibe
echo "Hola Mundo"

# Interpretar ordenes:
# Salto de línea
echo -e "Hola \nMundo"
# Tabulación
echo -e "Hola\tMundo"

# Ver las variables del sistema
echo $PATH
echo "Soy el usuario: $USER"

# Las comillas simples no interpretarán variables:
echo 'La variable del sistema que guarda el usuario es: $USER'

# Mostrar un fichero de texto
cat /etc/issue
# Mostrar con número de línea en el archivo
cat -n /etc/fstab
```

**Mostrar contenido de ficheros de texto**
```shell
# Mostrar el texto poco a poco -> 'h' muestra la ayuda
more archivo.ext
# Su mejora para los archivos grandes y con una "ventana nueva"
less archivo.ext
```
Búsqueda dentro del comando 'less':
/palabra + enter -> buscar coincidencias hacia delante.
?palabra + enter -> buscar coincidencias hacia atrás.
g -> Ir al principio del documento.
G -> Ir al final del documento.
:10 + g -> Ir a un número de línea concreto.

**Primeras y últimas líneas**
```shell
# Mostrar las 10 primeras líneas
head file.txt
# Mostrar una cantidad de líneas (5 por ejemplo):
head -n5 file.txt

# Mostrar las 10 últimas líneas, funciones como head
tail file.txt

# Conocer quién ha sido el último usuario registrado
tail -1 /etc/passwd
# Quien fue el primer usuario
head -n1 /etc/passwd

# Ejemplo de como monitorizar un archivo log (paquetes en este caso)
tail -f /var/log/dpkg.log  # CTRL + c para salir de este
```

**Filtrar texto**
`grep` -> Muestra sólo las líneas que cumplen con un patrón.
- `-i` -> No distingue entre mayúscula y minúscula.
- `-w` -> El patrón tiene que ser una palabra exacta.
- `-v` -> Muestra las que NO coinciden con el patrón.
- `-r` -> Busca en los ficheros de forma recursiva.
- `-n` -> Indica el número de línea.
- `-l` -> Indica solamente el fichero donde ha encontrado alguna coincidencia.
- `-c` -> La cantidad de líneas que cumplen con el patrón.
- `-B n`, `-A n` -> Muestra una cantidad de líneas n, antes (before) o después (after) de encontrar el patrón.
- `--color` -> Destaca el patrón en color dentro de la línea seleccionada.
- `^a` -> Empieza la línea por...
- `u$` -> ...termina la línea por...

Patrón -> `grep -opción palabra /path/archivo`

```shell
# Buscar una palabra en TODO los archivos de un directorio sin ir a niveles por debajo
grep root /etc/* 2> /dev/null  # enviar errores a nulo

# Buscar teniendo presente los subdirectorios (recursivo)
grep -r contrib /etc/* 2> /dev/null

# Buscar también la siguiente línea de la coincidencia
grep -w -A 1 1000 /etc/passwd

# Buscar también la anterior línea de la coincidencia
grep -w -B 1 1000 /etc/passwd
```

`cut` -> Corte "vertical", sólo una parte de cada línea.
- `-c` -> Selecciona sólo los caracteres que le indiquemos. Se pueden hacer delimitaciones.
	- `cut -c 5,10 file.txt` // `cut -c 7-25 file.txt`
- `-d` -> Indica un carácter separador entre los distintos campos de una línea. Por defecto buscará el tabulador.
- `-f` -> Elegir los campos que queremos que se muestren.

[Regresar a índice](#índice)

---

## Tuberías y redirecciones
### Redirecciones
``` shell
# Redirigir la salida del comando hacia un texto
echo "Hola mundo" > texto.txt
# Comprobar
cat texto.txt

# Si el archivo ya existe, añadir la salida al final de este.
echo "Hello world" >> texto.txt
```

Redirigir mensajes de *error* -> `2>`
Redirigir *todos* los mensajes -> `&>`
Desechar los resultados aquí -> `/dev/null`

### Tuberías
Redirigir el resultado hacia otro comando. Se pueden ir concatenando.
Se le llama "tubería" (pipe) ya que establece un "canal" por el que pasará el texto de un comando a otro.
``` shell
# Ejemplos
echo "HOLA MUNDO" | wc

grep -w ana /etc/passwd | cut -d":" -f1 | sort

ls -lr /etc/ | less

ls -lR /etc/ 2> /dev/null | grep shadow

ps -e | grep as
```

[Regresar a índice](#índice)

---
## Acciones sobre texto
### Ordenar con SORT
- Ordena de forma alfabética por defecto.

| Opciones | Tipo orden                         | Ejemplo                                                                                            |
| :------: | ---------------------------------- | -------------------------------------------------------------------------------------------------- |
|   `-r`   | Inversa                            | `sort -r archivo.txt`                                                                              |
|   `-t`   | Carácter de separación de columnas | `sort -t "," 2 archivo.txt`<br>Divide en el número de columnas dependiendo de cuantas "," existen. |
|   `-k`   | Columnas específicas               | `sort -t "," -k 2 archivo.txt`<br>Escoger la segunda columna de la división.                       |
|   `-n`   | Numéricamente                      | `sort -t "," -k 2 -n archivo.txt`                                                                  |
|          |                                    |                                                                                                    |
|   `-u`   | Eliminar líneas duplicadas         | `sort -u archivo.txt`                                                                              |

### Comando UNIQ
- Elimina las líneas repetidas contiguas en un archivo o entrada estàndar.

| Opciones | Tipo orden                         | Ejemplo                                           |
| :------: | ---------------------------------- | ------------------------------------------------- |
|   `-i`   | Ignora mayúsculas y minúsculas     | `cut -d"," -f1 nombres.txt \| sort \| uniq -i`    |
|   `-c`   | Cuenta la cantidad de repeticiones | `cut -d"," -f1 nombres.txt \| sort \| uniq -i -c` |
|   `-u`   | Muestra las no duplicadas          | `cut -d"," -f1 nombres.txt \| sort \| uniq -i -u` |

### Comando WC - Contar
- Contar líneas, palabras y bytes en archivos de texto o entradas estándar:
	- {Líneas} {Palabras} {Bytes}
		- Hay quien dice que el apartado de bytes se refiere al número de caracteres.
		- No es cierto porque si hacemos la comprobación con la "ñ", veremos que aumentan los bytes. -> consultar la pág. man de wc

| Opciones | Tipo orden |
| :------: | ---------- |
|   `-l`   | Líneas     |
|   `-w`   | Palabras   |
|   `-c`   | Bytes      |
|   `-m`   | Caracteres |

Ejemplos:
```shell
# Entrada estándar
echo "Hola" | wc

# Fichero
wc /etc/passwd
```

### Comando REV
Invertir el order de los caracteres en cada línea de un archivo de texto o entrada estándar.
- Útil cuando una línea se puede dividir en columnas y quiero convertir la última en la primera.

Ejemplos:
```shell
# Listado de todos los directorios
ls -lR ficheros | grep '/' | grep -v ^l | rev | cut -d"/" -f1 | rev
```

[Regresar a índice](#índice)

---

## Otros comandos para ficheros

### File -> Tipo de archivo
```shell
# Mostrar solo la descripción del tipo de archivo, su codificación.
file -b <archivo>

# Muestra el MIME del archivo.
file -i <archivo>

# Lee los nombres de los archivos desde un fichero
file -f <archivo_con_nombres>

# Ver los archivos y los tipos de cada uno en el directorio
file *
```

### LN -> Crear enlaces
Dos tipos:
- **Duro** -> Un puntero a la información del disco duro.
	- Aunque veamos que su peso es idéntico al archivo original, no es así.
		- Es un puntero a un archivo original.
		- Si borramos los enlaces, el peso seguirá siendo el mismo.
	- Comando `ls -lh`, la columna que hay después de los permisos, nos muestra el número de enlaces duros que tiene el archivo.
- **Blando** -> Se sustituye por la ruta del fichero original. Parecido a tener un "alias". Como un acceso directo.
	- Cuidado con poner rutas relativas sobretodo al mover archivos.

```shell
# Enlace duro:
ln <archivo> <ruta_y_nombre_archivo>

# Comprobar detalles del archivo
stat <archivo>

# Crear un enlace blando
ln -s <ruta_absoluta_archivo_original> <ruta_acceso_directo_archivo>
```

### DU -> Disk Usage
Espacio de disco ocupado por un fichero o por un directorio. Si le pasamos un directorio mostrará recursivamente el tamaño de todos los subdirectorios.
```shell
# Mostrar el tamaño del directorio y sus subdirectorios
du <ruta_directorio>

# Mostrar el tamaño en lectura humana
du -h

# Mostrar la cantidad total de lo que ocupa el directorio
du -s

# Mostrar la cantidad de todo lo que hay dentro del directorio
du -sh *
# Se puede concatenar con sort
du -sh * | sort -hr
```

### DF -> Disk Free
Información sobre las particiones del sistema: tamaño total, usado y libre.
```shell
# Ver el espacio disponible en el disco y sus particiones
df -h

# Verlo por tipo
df -hT

# Ver el tamaño de la partición donde está ubicado un archivo
df -h <ruta_archivo>
```
**Alternativa a DF**
```shell
sudo apt install dfc
# Ejecución
dfc
```

[Regresar a índice](#índice)

---

## Búsqueda en el sistema de ficheros
### LOCATE - Una primera búsqueda
```shell
# Instalar el paquete
sudo apt install locate

# Actualizar el sistema de ficheros con una base de datos para utilizar locate
su -
# Si se crea un archivo, un directorio, etc., actualizar la BD
updatedb

# Sintaxis locate
locate [opciones] [patrón]
```

| Opciones | Descripción                                    |
| :------: | ---------------------------------------------- |
|   `-i`   | Ignorar mayúsculas y minúsculas en la búsqueda |
|   `-c`   | Contar el número de coincidencias encontradas  |
|   `-n`   | Limitar el número de resultados.               |
|   `-r`   | Usar expresiones regulares para la búsqueda.   |

Ejemplos:
```shell
# Ignorando may y minus
locate -i "sources.list"

# Utilizando regex
locate -r "/sources.list$"
```

### Comando FIND
>[!danger]
>Comando super importante de dominar como administrador.

- Busca de forma recursiva en un directorio todos los ficheros que cumplan ciertas condiciones.
Sintaxis:
```shell
find [ruta] [opciones] [acción]
```

|       Opciones        | Descripción                                                                                                                                                                                                                                   |
| :-------------------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|        `-name`        | Especificar patrones para los nombres de los ficheros a buscar.                                                                                                                                                                               |
|       `-iname`        | Igual que `name` pero sin diferencias mayúsculas y minúsculas.                                                                                                                                                                                |
|        `-type`        | Tipo de fichero a buscar:<br>`d`-> directorios<br>`f` -> ficheros<br>`l` -> enlaces simbólicos<br>`b` -> dispositivos de bloque<br>`c` -> dispositivos de carácter<br>`p` -> para tuberías<br>`s` -> para sockets.                            |
|    `-size +/- <n>`    | `c` -> bytes<br>`k` -> kilobytes<br>`M` -> megabytes<br>`G` -> gigabytes                                                                                                                                                                      |
|     `-perm <num>`     | Ficheros cuyos permisos sean los expresados exactamente por número.<br>`-720` -> El signo `-` actuará como un AND: quiero exactamente esos permisos.<br>`/111` -> El signo `/` actuará como un OR: uno de los tres números debe de coincidir. |
|   `-user <usuario>`   | Especifica el usuario propietario del fichero                                                                                                                                                                                                 |
|  `-group <usuario>`   | Especifica el grupo propietario del fichero                                                                                                                                                                                                   |
|    `-mmin [+/-]n`     | Los datos del fichero fueron modificados por última vez hace `n` minutos.                                                                                                                                                                     |
|    `-mtime [+/-]n`    | Los datos del fichero fueron modificados por última vez hace `n` días.                                                                                                                                                                        |
| `-maxdepth <niveles>` | Desciende el número entero de niveles positivos en la jerarquía del directorio.                                                                                                                                                               |
|        `-path`        | Buscar por toda la ruta de los elementos. Se pueden utilizar comodines.                                                                                                                                                                       |
|       `-regex`        | Igual que `-path`, pero usando expresiones regulares.                                                                                                                                                                                         |

| Modificadores | Descripción                                                                        |
| :-----------: | ---------------------------------------------------------------------------------- |
|    `-not`     | Niega la condición que pongamos a continuación                                     |
 -> Blowfish
		- `$5# Comandos de Linux: desde cero hasta programar Shell Script

## Índice
- [Games to practice commands](#games-to-practice-commands)
- [Comandos del sistema de ficheros](#comandos-del-sistema-de-ficheros)
  - [Crear, mover, copiar y borrar ficheros y directorios](#crear-mover-copiar-y-borrar-ficheros-y-directorios)
  - [Atajos en la Shell](#atajos-en-la-shell)
  - [Uso de comodines](#uso-de-comodines)
- [Usuarios y Permisos](#usuarios-y-permisos)
  - [Permisos](#permisos)
    - [Cambiar permisos](#cambiar-permisos)
    - [Permisos usando números](#permisos-usando-números)
- [Administración de software](#administración-de-software)
  - [Comandos de gestión de software](#comandos-de-gestión-de-software)
- [Manejo de texto](#manejo-de-texto)
  - [Mostrar y filtrar texto](#mostrar-y-filtrar-texto)
- [Acciones sobre texto](#acciones-sobre-texto)
  - [Ordenar con SORT](#ordenar-con-sort)
  - [Comando UNIQ](#comando-uniq)
  - [Comando WC - Contar](#comando-wc---contar)
  - [Comando REV](#comando-rev)
- [Otros comandos para ficheros](#otros-comandos-para-ficheros)
  - [File -> Tipo de archivo](#file---tipo-de-archivo)
  - [LN -> Crear enlaces](#ln---crear-enlaces)
  - [DU -> Disk Usage](#du---disk-usage)
  - [DF -> Disk Free](#df---disk-free)
- [Búsqueda en el sistema de ficheros](#búsqueda-en-el-sistema-de-ficheros)
  - [LOCATE - Una primera búsqueda](#locate---una-primera-búsqueda)
  - [Comando FIND](#comando-find)
- [Ampliando conocimientos del Shell BASH](#ampliando-conocimientos-del-shell-bash)
  - [ALIAS](#alias)
  - [Sustitución de comandos](#sustitución-de-comandos)
- [Backups](#backups)
  - [Compactar y comprimir TAR](#compactar-y-comprimir-tar)
  - [Copias de seguridad totales, diferenciales e incrementales usando TAR](#copias-de-seguridad-totales-diferenciales-e-incrementales-usando-tar)
- [Gestión de paquetes con dpkg](#gestión-de-paquetes-con-dpkg)
- [Administración de Usuarios, Grupos y Contraseñas](#administración-de-usuarios-grupos-y-contraseñas)
  - [Usuarios](#usuarios)
  - [Contraseñas](#contraseñas)
  - [Grupos](#grupos)
  - [Cambio de propietario o grupo de un elemento](#cambio-de-propietario-o-grupo-de-un-elemento)
  - [Seguridad](#seguridad)
  - [Permisos especiales](#permisos-especiales)
- [Manejo de texto avanzado](#manejo-de-texto-avanzado)
  - [Comando TR](#comando-tr)
  - [Comando SED](#comando-sed)
- [Expresiones Regulares (Regex)](#expresiones-regulares-regex)

---
## Vídeos en YouTube
[Scheduling Tasks with Cron](https://youtu.be/7cbP7fzn0D8?si=Gk-bly6b5wPeRtsB)
[AWK](https://youtu.be/oPEnvuj9QrI?si=QESU7XS5yAe_TvqO)

---
## Games to practice commands
[Over the wire](https://overthewire.org/wargames/bandit/bandit0.html)
[Linux Survival](https://linuxsurvival.com)
[Practice Linux](https://www.practicelinux.com/home)

---
## Comandos del sistema de ficheros

```shell
# Ir al home del usuario
cd
cd ~
# Subir un nivel
cd ..
# Dos niveles
cd ../..

# Mostrar el contenido de uno o varios directorios
ls
# Format llistat
ls -l
# Pes en format llegible
ls -h
# Ordenar por el archivo + pesado
ls -S
# Ordenado al revés
ls -r
# Ordenado por archivo editado primero
ls -t
# Mostrar archivos ocultos
ls -a
```

Conocer el tipo de comando que utilizamos: `type comando`
Posibles resultados:
- orden interna -> built-in, como `cd`
- ejecutable -> /usr/bin/comando
- alias -> apodo de un comando más extenso

Cambiar a superusuario en Debian -> `su -`

### Crear, mover, copiar y borrar ficheros y directorios
```shell
# Crear carpeta
mkdir carpeta
# Crear dentro de un directorio ya existente
mkdir /ruta/carpeta/
# Crear carpetas padre -> hijo
mkdir -p carpeta/carpeta/

# Crear un fichero vacío
touch mi_fichero
# Alternativa
> mi_fichero2

# Mover o renombrar
mv mi_fichero mi_carta  # Renombra
mv mi_carta Documentos/  # Mueve y se podría renombrar

# Copiar: necesita dos parámetros
cp archivo_ruta_origen archivo_ruta_destino
# Copiar directorio y todo su contenido
cp -r directorio_ruta_origen directorio_ruta_destino
# Copiar varios elementos
cp fichero fichero ~  # Pega en la carpeta home

# Borrar fichero
rm fichero
# Borrar directorio vacío
rmdir directorio
# Borrar directorio y su contenido
rm -r directorio

# Ver la jerarquía de archivos y documentos
tree
tree ruta/
```

>[!tip] Copia de archivos que pertenecen a root
>Archivos que pertenecen a root podrían llegar a ser copiados por otros usuarios y luego llegar a ver el contenido que antes no se les permitía.
>
>En cambio si se mueve, el propietario se respeta y con los respectivos permisos.

### Atajos en la Shell

| Atajo                   | Descripción                                    |
| ----------------------- | ---------------------------------------------- |
| Cursores arriba y abajo | Historial de instrucciones                     |
| Tab                     | Completar la ruta de un elemento del sistema   |
| Doble Tab               | Opciones para el auto completado               |
| Ctrl + r                | Búsqueda de instrucciones                      |
| Ctrl + c                | Terminar un comando ejecutado a medias         |
| `history`               | Listado de comandos ejecutados                 |
| `!número`               | Llamar a uno de los comandos de `history`      |
| `sudo !!`               | Ejecutar la última instrucción con privilegios |
| Ctrl + a                | Ir al inicio del comando escrito               |
| Ctrl + e                | Ir al final del comando escrito                |
| Ctrl + l                | borrar la pantalla                             |

### Uso de comodines
Asterisco -> "Cualquier cosa"
```shell
# Hacer referencia a los archivos que tienen una extensión en común, ejemplo:
rm *.gif
# Todo!
rm *

# Cualquier archivo o carpeta cuyo nombre contenga la letra 'u'
cp *u* seleccion/
# O que además en la extensión tengan una 'i'
cp *u*.*i* seleccion/
# Que por lo menos exista una extensión (ficheros)
cp *.*
```

Interrogación -> "Cualquier carácter y cualquier longitud de cadena de caracteres o ninguna"
```shell
# Caracteres obligatorios, cualquier carácter OBLIGATORIO en la posición y lo que siga O NO.
ls c?sa*
# Obligación de una cifra
ls informe-200?.txt
ls informe-19??.txt
# Cualquier longitud de nombre hasta el punto + obligación en el último carácter de la extensión
ls c*.tx?
```

Corchetes -> opciones obligatorias en una posición
```shell
# Elige cualquiera de los tres caracteres marcados en corchetes.
ls c[aio]s*

# Rangos
ls c[a-m]s*
# Mayúsculas
ls c[A-Z]s*
# Números
ls informe-200[1-9].txt
```

> [!tip]
> Resultará útil consultar `man bash` y buscar `Concordancia`:
> /Concordancia + n (seguir buscando hasta llegar a la sección)

[Regresar a índice](#índice)

---
## Usuarios y Permisos
Cuando se crea un usuario, si no se especifica, se crea un grupo con el mismo nombre.

Quién soy y a qué grupos pertenezco -> `id`

| Características | Descripción                                       |
| --------------- | ------------------------------------------------- |
| uid             | identificador del usuario                         |
| gid             | grupo principal al que pertenece el usuario       |
| grupos          | grupos secundarios a los que pertenece el usuario |

Comandos básicos de ejecución:

| Comando                | Descripción                   |
| ---------------------- | ----------------------------- |
| `su usuario`           | Cambio de usuario             |
| `su root -c "comando"` | Ejecutar un comando como root |
| `passwd`               | Cambiar la contraseña         |

Se necessita ser _root_ para crear usuarios y grupos:

| Comando                                      | Descripción                                       |
| -------------------------------------------- | ------------------------------------------------- |
| `groupadd nombre_grupo`                      | Crear grupo                                       |
| `useradd -m nombre_usuario`                  | Crear usuario con su propio grupo                 |
| `useradd -m -g grupo_asignar nombre_usuario` | Crear usuario y asignar a un grupo                |
| `adduser nombre_usuario`                     | Crear usuario con prompts administrativos         |
| `userdel -r nombre_usuario`                  | Eliminar usuario y su directorio home             |
| `groupdel nombre_grupo`                      | Eliminar el grupo (asegurarse que no está en uso) |

### Permisos

| Permisos | Descripción                         |
| -------- | ----------------------------------- |
| `-`      | No tiene permisos                   |
| `r`      | read -> puede ver pero no modificar |
| `w`      | write -> puede modificar            |
| `x`      | execute -> puede ejecutar           |

`-rwxrwxrwx`: permisos por posiciones
- 1r grupo: permisos de usuario
- 2o grupo: permisos del grupo propietario del fichero
- 3r grupo: permisos del resto de usuarios del sistema

|                |   R    |                   W                   |                  X                   |
| -------------- | :----: | :-----------------------------------: | :----------------------------------: |
| **Fichero**    |  Leer  |               Escribir                | Ejecutar<br>(programa, script, etc.) |
| **Directorio** | Listar | Modificar el contenido del directorio |               Acceder                |

#### Cambiar permisos
`chmod`
- Letra del permiso. Si son varios permisos, separarlas por comas.
	- Si no se pone nada, se realiza en todos.
- + para añadir permiso / - para quitar permiso
- Ruta del elemento a cambiar permisos
- Indicación de a quién asignarlo: u=User, g=Group, o=Others

Ejemplos:
```shell
# Añadir permiso de escritura al grupo
chmod g+w ruta

# Quitar el permiso de ejecución a todos
chmod -x ruta

# Añadir permiso de escritura a usuario y a otros
chmod u+w,o+w ruta

# Quitar el permiso de ejecución y escritura al usuario y poner el de escritura al grupo
chmod u-xw,g+w ruta
```

#### Permisos usando números

| Permission Type | Read (4) | Write (2) | Execute (1) |
| --------------- | -------- | --------- | ----------- |
| User            | 1 / 0    | 1 / 0     | 1 / 0       |
| Group           | 1 / 0    | 1 / 0     | 1 / 0       |
| Others          | 1 / 0    | 1 / 0     | 1 / 0       |

Si quiero poner permisos al usuario de lectura y escritura, al grupo sólo lectura y al resto nada:
- Binario -> `110 100 000`
- Decimal -> `640`
- Comando -> `chmod 640 fichero`
En este tipo de cambio, actuará en los tres grupos.

[Regresar a índice](#índice)

---

## Administración de software
En Debian 12, se ha realizado el cambio de que en una instalación limpia se consiguen los paquetes a través de la ISO.
Hasta el día de hoy Debian utilizaba un solo archivo para obtener los repositorios -> `/etc/apt/sources.list`.
A partir de Debian 12 se ha modularizado -> `/etc/sources.list.d/`
Y se debe de añadir un servicio CDN para que funcione el comando APT.

>[!info]
>Es importante conocer el nombre en clave de la distribución -> Debian 12 = Bookworm

**Cómo añadir las fuentes para el APT en un archivo**
[Instrucciones de la web oficial de Debian -> 4.3.1](https://www.debian.org/releases/trixie/release-notes/upgrading.es.html#adding-apt-internet-sources)
1. Nos dirigimos a `/etc/apt/sources.list`
2. Comentamos la linea que especifica que utilizará los paquetes del CD.
3. Añadimos la siguiente linea: `deb https://deb.debian.org/debian bookworm main contrib`

**Cómo añadir las fuentes APT de manera modularizada**
1. Comentamos `/etc/apt/sources.list`
2. `nano /etc/apt/sources.list.d/repo_oficial.list
3. Añadimos `deb https://deb.debian.org/debian bookworm main contrib`

### Comandos de gestión de software

| `apt`        | Descripción                                                                                                    |
| ------------ | -------------------------------------------------------------------------------------------------------------- |
| `update`     | Actualiza la lista de paquetes disponibles en los repositorios                                                 |
| `search`     | Busca paquetes disponibles en los repositorios que coincidan con un término de búsqueda.                       |
| `show`       | Muestra información detallada sobre un paquete específico.                                                     |
| `list`       | Lista paquetes según criterios, como los instalados, actualizables o que cumplen cierta condición de búsqueda. |
| `install`    | Instala un paquete de software en el sistema.                                                                  |
| `remove`     | Elimina un paquete del sistema, manteniendo los archivos de configuración.                                     |
| `purge`      | Elimina un paquete del sistema junto con sus archivos de configuración.                                        |
| `autoremove` | Elimina paquetes que no tienen dependencias.                                                                   |

[Regresar a índice](#índice)

---
## Manejo de texto
>[!warning]
>**¿Por qué es importante?**
>Configuraciones se guardan en ficheros de texto.
>Los logs también son en formato texto.
>Cualquier programa en modo consola que muestra resultados mediante texto.

>[!info]
>Dominar esto ayuda muchísimo como administrador del servidor.

### Mostrar y filtrar texto

**Mostrar texto**
```shell
# Mostrar texto que se recibe
echo "Hola Mundo"

# Interpretar ordenes:
# Salto de línea
echo -e "Hola \nMundo"
# Tabulación
echo -e "Hola\tMundo"

# Ver las variables del sistema
echo $PATH
echo "Soy el usuario: $USER"

# Las comillas simples no interpretarán variables:
echo 'La variable del sistema que guarda el usuario es: $USER'

# Mostrar un fichero de texto
cat /etc/issue
# Mostrar con número de línea en el archivo
cat -n /etc/fstab
```

**Mostrar contenido de ficheros de texto**
```shell
# Mostrar el texto poco a poco -> 'h' muestra la ayuda
more archivo.ext
# Su mejora para los archivos grandes y con una "ventana nueva"
less archivo.ext
```
Búsqueda dentro del comando 'less':
/palabra + enter -> buscar coincidencias hacia delante.
?palabra + enter -> buscar coincidencias hacia atrás.
g -> Ir al principio del documento.
G -> Ir al final del documento.
:10 + g -> Ir a un número de línea concreto.

**Primeras y últimas líneas**
```shell
# Mostrar las 10 primeras líneas
head file.txt
# Mostrar una cantidad de líneas (5 por ejemplo):
head -n5 file.txt

# Mostrar las 10 últimas líneas, funciones como head
tail file.txt

# Conocer quién ha sido el último usuario registrado
tail -1 /etc/passwd
# Quien fue el primer usuario
head -n1 /etc/passwd

# Ejemplo de como monitorizar un archivo log (paquetes en este caso)
tail -f /var/log/dpkg.log  # CTRL + c para salir de este
```

**Filtrar texto**
`grep` -> Muestra sólo las líneas que cumplen con un patrón.
- `-i` -> No distingue entre mayúscula y minúscula.
- `-w` -> El patrón tiene que ser una palabra exacta.
- `-v` -> Muestra las que NO coinciden con el patrón.
- `-r` -> Busca en los ficheros de forma recursiva.
- `-n` -> Indica el número de línea.
- `-l` -> Indica solamente el fichero donde ha encontrado alguna coincidencia.
- `-c` -> La cantidad de líneas que cumplen con el patrón.
- `-B n`, `-A n` -> Muestra una cantidad de líneas n, antes (before) o después (after) de encontrar el patrón.
- `--color` -> Destaca el patrón en color dentro de la línea seleccionada.
- `^a` -> Empieza la línea por...
- `u$` -> ...termina la línea por...

Patrón -> `grep -opción palabra /path/archivo`

```shell
# Buscar una palabra en TODO los archivos de un directorio sin ir a niveles por debajo
grep root /etc/* 2> /dev/null  # enviar errores a nulo

# Buscar teniendo presente los subdirectorios (recursivo)
grep -r contrib /etc/* 2> /dev/null

# Buscar también la siguiente línea de la coincidencia
grep -w -A 1 1000 /etc/passwd

# Buscar también la anterior línea de la coincidencia
grep -w -B 1 1000 /etc/passwd
```

`cut` -> Corte "vertical", sólo una parte de cada línea.
- `-c` -> Selecciona sólo los caracteres que le indiquemos. Se pueden hacer delimitaciones.
	- `cut -c 5,10 file.txt` // `cut -c 7-25 file.txt`
- `-d` -> Indica un carácter separador entre los distintos campos de una línea. Por defecto buscará el tabulador.
- `-f` -> Elegir los campos que queremos que se muestren.

[Regresar a índice](#índice)

---

## Tuberías y redirecciones
### Redirecciones
``` shell
# Redirigir la salida del comando hacia un texto
echo "Hola mundo" > texto.txt
# Comprobar
cat texto.txt

# Si el archivo ya existe, añadir la salida al final de este.
echo "Hello world" >> texto.txt
```

Redirigir mensajes de *error* -> `2>`
Redirigir *todos* los mensajes -> `&>`
Desechar los resultados aquí -> `/dev/null`

### Tuberías
Redirigir el resultado hacia otro comando. Se pueden ir concatenando.
Se le llama "tubería" (pipe) ya que establece un "canal" por el que pasará el texto de un comando a otro.
``` shell
# Ejemplos
echo "HOLA MUNDO" | wc

grep -w ana /etc/passwd | cut -d":" -f1 | sort

ls -lr /etc/ | less

ls -lR /etc/ 2> /dev/null | grep shadow

ps -e | grep as
```

[Regresar a índice](#índice)

---
## Acciones sobre texto
### Ordenar con SORT
- Ordena de forma alfabética por defecto.

| Opciones | Tipo orden                         | Ejemplo                                                                                            |
| :------: | ---------------------------------- | -------------------------------------------------------------------------------------------------- |
|   `-r`   | Inversa                            | `sort -r archivo.txt`                                                                              |
|   `-t`   | Carácter de separación de columnas | `sort -t "," 2 archivo.txt`<br>Divide en el número de columnas dependiendo de cuantas "," existen. |
|   `-k`   | Columnas específicas               | `sort -t "," -k 2 archivo.txt`<br>Escoger la segunda columna de la división.                       |
|   `-n`   | Numéricamente                      | `sort -t "," -k 2 -n archivo.txt`                                                                  |
|          |                                    |                                                                                                    |
|   `-u`   | Eliminar líneas duplicadas         | `sort -u archivo.txt`                                                                              |

### Comando UNIQ
- Elimina las líneas repetidas contiguas en un archivo o entrada estàndar.

| Opciones | Tipo orden                         | Ejemplo                                           |
| :------: | ---------------------------------- | ------------------------------------------------- |
|   `-i`   | Ignora mayúsculas y minúsculas     | `cut -d"," -f1 nombres.txt \| sort \| uniq -i`    |
|   `-c`   | Cuenta la cantidad de repeticiones | `cut -d"," -f1 nombres.txt \| sort \| uniq -i -c` |
|   `-u`   | Muestra las no duplicadas          | `cut -d"," -f1 nombres.txt \| sort \| uniq -i -u` |

### Comando WC - Contar
- Contar líneas, palabras y bytes en archivos de texto o entradas estándar:
	- {Líneas} {Palabras} {Bytes}
		- Hay quien dice que el apartado de bytes se refiere al número de caracteres.
		- No es cierto porque si hacemos la comprobación con la "ñ", veremos que aumentan los bytes. -> consultar la pág. man de wc

| Opciones | Tipo orden |
| :------: | ---------- |
|   `-l`   | Líneas     |
|   `-w`   | Palabras   |
|   `-c`   | Bytes      |
|   `-m`   | Caracteres |

Ejemplos:
```shell
# Entrada estándar
echo "Hola" | wc

# Fichero
wc /etc/passwd
```

### Comando REV
Invertir el order de los caracteres en cada línea de un archivo de texto o entrada estándar.
- Útil cuando una línea se puede dividir en columnas y quiero convertir la última en la primera.

Ejemplos:
```shell
# Listado de todos los directorios
ls -lR ficheros | grep '/' | grep -v ^l | rev | cut -d"/" -f1 | rev
```

[Regresar a índice](#índice)

---

## Otros comandos para ficheros

### File -> Tipo de archivo
```shell
# Mostrar solo la descripción del tipo de archivo, su codificación.
file -b <archivo>

# Muestra el MIME del archivo.
file -i <archivo>

# Lee los nombres de los archivos desde un fichero
file -f <archivo_con_nombres>

# Ver los archivos y los tipos de cada uno en el directorio
file *
```

### LN -> Crear enlaces
Dos tipos:
- **Duro** -> Un puntero a la información del disco duro.
	- Aunque veamos que su peso es idéntico al archivo original, no es así.
		- Es un puntero a un archivo original.
		- Si borramos los enlaces, el peso seguirá siendo el mismo.
	- Comando `ls -lh`, la columna que hay después de los permisos, nos muestra el número de enlaces duros que tiene el archivo.
- **Blando** -> Se sustituye por la ruta del fichero original. Parecido a tener un "alias". Como un acceso directo.
	- Cuidado con poner rutas relativas sobretodo al mover archivos.

```shell
# Enlace duro:
ln <archivo> <ruta_y_nombre_archivo>

# Comprobar detalles del archivo
stat <archivo>

# Crear un enlace blando
ln -s <ruta_absoluta_archivo_original> <ruta_acceso_directo_archivo>
```

### DU -> Disk Usage
Espacio de disco ocupado por un fichero o por un directorio. Si le pasamos un directorio mostrará recursivamente el tamaño de todos los subdirectorios.
```shell
# Mostrar el tamaño del directorio y sus subdirectorios
du <ruta_directorio>

# Mostrar el tamaño en lectura humana
du -h

# Mostrar la cantidad total de lo que ocupa el directorio
du -s

# Mostrar la cantidad de todo lo que hay dentro del directorio
du -sh *
# Se puede concatenar con sort
du -sh * | sort -hr
```

### DF -> Disk Free
Información sobre las particiones del sistema: tamaño total, usado y libre.
```shell
# Ver el espacio disponible en el disco y sus particiones
df -h

# Verlo por tipo
df -hT

# Ver el tamaño de la partición donde está ubicado un archivo
df -h <ruta_archivo>
```
**Alternativa a DF**
```shell
sudo apt install dfc
# Ejecución
dfc
```

[Regresar a índice](#índice)

---

## Búsqueda en el sistema de ficheros
### LOCATE - Una primera búsqueda
```shell
# Instalar el paquete
sudo apt install locate

# Actualizar el sistema de ficheros con una base de datos para utilizar locate
su -
# Si se crea un archivo, un directorio, etc., actualizar la BD
updatedb

# Sintaxis locate
locate [opciones] [patrón]
```

| Opciones | Descripción                                    |
| :------: | ---------------------------------------------- |
|   `-i`   | Ignorar mayúsculas y minúsculas en la búsqueda |
|   `-c`   | Contar el número de coincidencias encontradas  |
|   `-n`   | Limitar el número de resultados.               |
|   `-r`   | Usar expresiones regulares para la búsqueda.   |

Ejemplos:
```shell
# Ignorando may y minus
locate -i "sources.list"

# Utilizando regex
locate -r "/sources.list$"
```

### Comando FIND
>[!danger]
>Comando super importante de dominar como administrador.

- Busca de forma recursiva en un directorio todos los ficheros que cumplan ciertas condiciones.
Sintaxis:
```shell
find [ruta] [opciones] [acción]
```

|       Opciones        | Descripción                                                                                                                                                                                                                                   |
| :-------------------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|        `-name`        | Especificar patrones para los nombres de los ficheros a buscar.                                                                                                                                                                               |
|       `-iname`        | Igual que `name` pero sin diferencias mayúsculas y minúsculas.                                                                                                                                                                                |
|        `-type`        | Tipo de fichero a buscar:<br>`d`-> directorios<br>`f` -> ficheros<br>`l` -> enlaces simbólicos<br>`b` -> dispositivos de bloque<br>`c` -> dispositivos de carácter<br>`p` -> para tuberías<br>`s` -> para sockets.                            |
|    `-size +/- <n>`    | `c` -> bytes<br>`k` -> kilobytes<br>`M` -> megabytes<br>`G` -> gigabytes                                                                                                                                                                      |
|     `-perm <num>`     | Ficheros cuyos permisos sean los expresados exactamente por número.<br>`-720` -> El signo `-` actuará como un AND: quiero exactamente esos permisos.<br>`/111` -> El signo `/` actuará como un OR: uno de los tres números debe de coincidir. |
|   `-user <usuario>`   | Especifica el usuario propietario del fichero                                                                                                                                                                                                 |
|  `-group <usuario>`   | Especifica el grupo propietario del fichero                                                                                                                                                                                                   |
|    `-mmin [+/-]n`     | Los datos del fichero fueron modificados por última vez hace `n` minutos.                                                                                                                                                                     |
|    `-mtime [+/-]n`    | Los datos del fichero fueron modificados por última vez hace `n` días.                                                                                                                                                                        |
| `-maxdepth <niveles>` | Desciende el número entero de niveles positivos en la jerarquía del directorio.                                                                                                                                                               |
|        `-path`        | Buscar por toda la ruta de los elementos. Se pueden utilizar comodines.                                                                                                                                                                       |
|       `-regex`        | Igual que `-path`, pero usando expresiones regulares.                                                                                                                                                                                         |

| Modificadores | Descripción                                                                        |
| :-----------: | ---------------------------------------------------------------------------------- |
|    `-not`     | Niega la condición que pongamos a continuación                                     |
 -> SHA-256
		- `$6# Comandos de Linux: desde cero hasta programar Shell Script

## Índice
- [Games to practice commands](#games-to-practice-commands)
- [Comandos del sistema de ficheros](#comandos-del-sistema-de-ficheros)
  - [Crear, mover, copiar y borrar ficheros y directorios](#crear-mover-copiar-y-borrar-ficheros-y-directorios)
  - [Atajos en la Shell](#atajos-en-la-shell)
  - [Uso de comodines](#uso-de-comodines)
- [Usuarios y Permisos](#usuarios-y-permisos)
  - [Permisos](#permisos)
    - [Cambiar permisos](#cambiar-permisos)
    - [Permisos usando números](#permisos-usando-números)
- [Administración de software](#administración-de-software)
  - [Comandos de gestión de software](#comandos-de-gestión-de-software)
- [Manejo de texto](#manejo-de-texto)
  - [Mostrar y filtrar texto](#mostrar-y-filtrar-texto)
- [Acciones sobre texto](#acciones-sobre-texto)
  - [Ordenar con SORT](#ordenar-con-sort)
  - [Comando UNIQ](#comando-uniq)
  - [Comando WC - Contar](#comando-wc---contar)
  - [Comando REV](#comando-rev)
- [Otros comandos para ficheros](#otros-comandos-para-ficheros)
  - [File -> Tipo de archivo](#file---tipo-de-archivo)
  - [LN -> Crear enlaces](#ln---crear-enlaces)
  - [DU -> Disk Usage](#du---disk-usage)
  - [DF -> Disk Free](#df---disk-free)
- [Búsqueda en el sistema de ficheros](#búsqueda-en-el-sistema-de-ficheros)
  - [LOCATE - Una primera búsqueda](#locate---una-primera-búsqueda)
  - [Comando FIND](#comando-find)
- [Ampliando conocimientos del Shell BASH](#ampliando-conocimientos-del-shell-bash)
  - [ALIAS](#alias)
  - [Sustitución de comandos](#sustitución-de-comandos)
- [Backups](#backups)
  - [Compactar y comprimir TAR](#compactar-y-comprimir-tar)
  - [Copias de seguridad totales, diferenciales e incrementales usando TAR](#copias-de-seguridad-totales-diferenciales-e-incrementales-usando-tar)
- [Gestión de paquetes con dpkg](#gestión-de-paquetes-con-dpkg)
- [Administración de Usuarios, Grupos y Contraseñas](#administración-de-usuarios-grupos-y-contraseñas)
  - [Usuarios](#usuarios)
  - [Contraseñas](#contraseñas)
  - [Grupos](#grupos)
  - [Cambio de propietario o grupo de un elemento](#cambio-de-propietario-o-grupo-de-un-elemento)
  - [Seguridad](#seguridad)
  - [Permisos especiales](#permisos-especiales)
- [Manejo de texto avanzado](#manejo-de-texto-avanzado)
  - [Comando TR](#comando-tr)
  - [Comando SED](#comando-sed)
- [Expresiones Regulares (Regex)](#expresiones-regulares-regex)

---
## Vídeos en YouTube
[Scheduling Tasks with Cron](https://youtu.be/7cbP7fzn0D8?si=Gk-bly6b5wPeRtsB)
[AWK](https://youtu.be/oPEnvuj9QrI?si=QESU7XS5yAe_TvqO)

---
## Games to practice commands
[Over the wire](https://overthewire.org/wargames/bandit/bandit0.html)
[Linux Survival](https://linuxsurvival.com)
[Practice Linux](https://www.practicelinux.com/home)

---
## Comandos del sistema de ficheros

```shell
# Ir al home del usuario
cd
cd ~
# Subir un nivel
cd ..
# Dos niveles
cd ../..

# Mostrar el contenido de uno o varios directorios
ls
# Format llistat
ls -l
# Pes en format llegible
ls -h
# Ordenar por el archivo + pesado
ls -S
# Ordenado al revés
ls -r
# Ordenado por archivo editado primero
ls -t
# Mostrar archivos ocultos
ls -a
```

Conocer el tipo de comando que utilizamos: `type comando`
Posibles resultados:
- orden interna -> built-in, como `cd`
- ejecutable -> /usr/bin/comando
- alias -> apodo de un comando más extenso

Cambiar a superusuario en Debian -> `su -`

### Crear, mover, copiar y borrar ficheros y directorios
```shell
# Crear carpeta
mkdir carpeta
# Crear dentro de un directorio ya existente
mkdir /ruta/carpeta/
# Crear carpetas padre -> hijo
mkdir -p carpeta/carpeta/

# Crear un fichero vacío
touch mi_fichero
# Alternativa
> mi_fichero2

# Mover o renombrar
mv mi_fichero mi_carta  # Renombra
mv mi_carta Documentos/  # Mueve y se podría renombrar

# Copiar: necesita dos parámetros
cp archivo_ruta_origen archivo_ruta_destino
# Copiar directorio y todo su contenido
cp -r directorio_ruta_origen directorio_ruta_destino
# Copiar varios elementos
cp fichero fichero ~  # Pega en la carpeta home

# Borrar fichero
rm fichero
# Borrar directorio vacío
rmdir directorio
# Borrar directorio y su contenido
rm -r directorio

# Ver la jerarquía de archivos y documentos
tree
tree ruta/
```

>[!tip] Copia de archivos que pertenecen a root
>Archivos que pertenecen a root podrían llegar a ser copiados por otros usuarios y luego llegar a ver el contenido que antes no se les permitía.
>
>En cambio si se mueve, el propietario se respeta y con los respectivos permisos.

### Atajos en la Shell

| Atajo                   | Descripción                                    |
| ----------------------- | ---------------------------------------------- |
| Cursores arriba y abajo | Historial de instrucciones                     |
| Tab                     | Completar la ruta de un elemento del sistema   |
| Doble Tab               | Opciones para el auto completado               |
| Ctrl + r                | Búsqueda de instrucciones                      |
| Ctrl + c                | Terminar un comando ejecutado a medias         |
| `history`               | Listado de comandos ejecutados                 |
| `!número`               | Llamar a uno de los comandos de `history`      |
| `sudo !!`               | Ejecutar la última instrucción con privilegios |
| Ctrl + a                | Ir al inicio del comando escrito               |
| Ctrl + e                | Ir al final del comando escrito                |
| Ctrl + l                | borrar la pantalla                             |

### Uso de comodines
Asterisco -> "Cualquier cosa"
```shell
# Hacer referencia a los archivos que tienen una extensión en común, ejemplo:
rm *.gif
# Todo!
rm *

# Cualquier archivo o carpeta cuyo nombre contenga la letra 'u'
cp *u* seleccion/
# O que además en la extensión tengan una 'i'
cp *u*.*i* seleccion/
# Que por lo menos exista una extensión (ficheros)
cp *.*
```

Interrogación -> "Cualquier carácter y cualquier longitud de cadena de caracteres o ninguna"
```shell
# Caracteres obligatorios, cualquier carácter OBLIGATORIO en la posición y lo que siga O NO.
ls c?sa*
# Obligación de una cifra
ls informe-200?.txt
ls informe-19??.txt
# Cualquier longitud de nombre hasta el punto + obligación en el último carácter de la extensión
ls c*.tx?
```

Corchetes -> opciones obligatorias en una posición
```shell
# Elige cualquiera de los tres caracteres marcados en corchetes.
ls c[aio]s*

# Rangos
ls c[a-m]s*
# Mayúsculas
ls c[A-Z]s*
# Números
ls informe-200[1-9].txt
```

> [!tip]
> Resultará útil consultar `man bash` y buscar `Concordancia`:
> /Concordancia + n (seguir buscando hasta llegar a la sección)

[Regresar a índice](#índice)

---
## Usuarios y Permisos
Cuando se crea un usuario, si no se especifica, se crea un grupo con el mismo nombre.

Quién soy y a qué grupos pertenezco -> `id`

| Características | Descripción                                       |
| --------------- | ------------------------------------------------- |
| uid             | identificador del usuario                         |
| gid             | grupo principal al que pertenece el usuario       |
| grupos          | grupos secundarios a los que pertenece el usuario |

Comandos básicos de ejecución:

| Comando                | Descripción                   |
| ---------------------- | ----------------------------- |
| `su usuario`           | Cambio de usuario             |
| `su root -c "comando"` | Ejecutar un comando como root |
| `passwd`               | Cambiar la contraseña         |

Se necessita ser _root_ para crear usuarios y grupos:

| Comando                                      | Descripción                                       |
| -------------------------------------------- | ------------------------------------------------- |
| `groupadd nombre_grupo`                      | Crear grupo                                       |
| `useradd -m nombre_usuario`                  | Crear usuario con su propio grupo                 |
| `useradd -m -g grupo_asignar nombre_usuario` | Crear usuario y asignar a un grupo                |
| `adduser nombre_usuario`                     | Crear usuario con prompts administrativos         |
| `userdel -r nombre_usuario`                  | Eliminar usuario y su directorio home             |
| `groupdel nombre_grupo`                      | Eliminar el grupo (asegurarse que no está en uso) |

### Permisos

| Permisos | Descripción                         |
| -------- | ----------------------------------- |
| `-`      | No tiene permisos                   |
| `r`      | read -> puede ver pero no modificar |
| `w`      | write -> puede modificar            |
| `x`      | execute -> puede ejecutar           |

`-rwxrwxrwx`: permisos por posiciones
- 1r grupo: permisos de usuario
- 2o grupo: permisos del grupo propietario del fichero
- 3r grupo: permisos del resto de usuarios del sistema

|                |   R    |                   W                   |                  X                   |
| -------------- | :----: | :-----------------------------------: | :----------------------------------: |
| **Fichero**    |  Leer  |               Escribir                | Ejecutar<br>(programa, script, etc.) |
| **Directorio** | Listar | Modificar el contenido del directorio |               Acceder                |

#### Cambiar permisos
`chmod`
- Letra del permiso. Si son varios permisos, separarlas por comas.
	- Si no se pone nada, se realiza en todos.
- + para añadir permiso / - para quitar permiso
- Ruta del elemento a cambiar permisos
- Indicación de a quién asignarlo: u=User, g=Group, o=Others

Ejemplos:
```shell
# Añadir permiso de escritura al grupo
chmod g+w ruta

# Quitar el permiso de ejecución a todos
chmod -x ruta

# Añadir permiso de escritura a usuario y a otros
chmod u+w,o+w ruta

# Quitar el permiso de ejecución y escritura al usuario y poner el de escritura al grupo
chmod u-xw,g+w ruta
```

#### Permisos usando números

| Permission Type | Read (4) | Write (2) | Execute (1) |
| --------------- | -------- | --------- | ----------- |
| User            | 1 / 0    | 1 / 0     | 1 / 0       |
| Group           | 1 / 0    | 1 / 0     | 1 / 0       |
| Others          | 1 / 0    | 1 / 0     | 1 / 0       |

Si quiero poner permisos al usuario de lectura y escritura, al grupo sólo lectura y al resto nada:
- Binario -> `110 100 000`
- Decimal -> `640`
- Comando -> `chmod 640 fichero`
En este tipo de cambio, actuará en los tres grupos.

[Regresar a índice](#índice)

---

## Administración de software
En Debian 12, se ha realizado el cambio de que en una instalación limpia se consiguen los paquetes a través de la ISO.
Hasta el día de hoy Debian utilizaba un solo archivo para obtener los repositorios -> `/etc/apt/sources.list`.
A partir de Debian 12 se ha modularizado -> `/etc/sources.list.d/`
Y se debe de añadir un servicio CDN para que funcione el comando APT.

>[!info]
>Es importante conocer el nombre en clave de la distribución -> Debian 12 = Bookworm

**Cómo añadir las fuentes para el APT en un archivo**
[Instrucciones de la web oficial de Debian -> 4.3.1](https://www.debian.org/releases/trixie/release-notes/upgrading.es.html#adding-apt-internet-sources)
1. Nos dirigimos a `/etc/apt/sources.list`
2. Comentamos la linea que especifica que utilizará los paquetes del CD.
3. Añadimos la siguiente linea: `deb https://deb.debian.org/debian bookworm main contrib`

**Cómo añadir las fuentes APT de manera modularizada**
1. Comentamos `/etc/apt/sources.list`
2. `nano /etc/apt/sources.list.d/repo_oficial.list
3. Añadimos `deb https://deb.debian.org/debian bookworm main contrib`

### Comandos de gestión de software

| `apt`        | Descripción                                                                                                    |
| ------------ | -------------------------------------------------------------------------------------------------------------- |
| `update`     | Actualiza la lista de paquetes disponibles en los repositorios                                                 |
| `search`     | Busca paquetes disponibles en los repositorios que coincidan con un término de búsqueda.                       |
| `show`       | Muestra información detallada sobre un paquete específico.                                                     |
| `list`       | Lista paquetes según criterios, como los instalados, actualizables o que cumplen cierta condición de búsqueda. |
| `install`    | Instala un paquete de software en el sistema.                                                                  |
| `remove`     | Elimina un paquete del sistema, manteniendo los archivos de configuración.                                     |
| `purge`      | Elimina un paquete del sistema junto con sus archivos de configuración.                                        |
| `autoremove` | Elimina paquetes que no tienen dependencias.                                                                   |

[Regresar a índice](#índice)

---
## Manejo de texto
>[!warning]
>**¿Por qué es importante?**
>Configuraciones se guardan en ficheros de texto.
>Los logs también son en formato texto.
>Cualquier programa en modo consola que muestra resultados mediante texto.

>[!info]
>Dominar esto ayuda muchísimo como administrador del servidor.

### Mostrar y filtrar texto

**Mostrar texto**
```shell
# Mostrar texto que se recibe
echo "Hola Mundo"

# Interpretar ordenes:
# Salto de línea
echo -e "Hola \nMundo"
# Tabulación
echo -e "Hola\tMundo"

# Ver las variables del sistema
echo $PATH
echo "Soy el usuario: $USER"

# Las comillas simples no interpretarán variables:
echo 'La variable del sistema que guarda el usuario es: $USER'

# Mostrar un fichero de texto
cat /etc/issue
# Mostrar con número de línea en el archivo
cat -n /etc/fstab
```

**Mostrar contenido de ficheros de texto**
```shell
# Mostrar el texto poco a poco -> 'h' muestra la ayuda
more archivo.ext
# Su mejora para los archivos grandes y con una "ventana nueva"
less archivo.ext
```
Búsqueda dentro del comando 'less':
/palabra + enter -> buscar coincidencias hacia delante.
?palabra + enter -> buscar coincidencias hacia atrás.
g -> Ir al principio del documento.
G -> Ir al final del documento.
:10 + g -> Ir a un número de línea concreto.

**Primeras y últimas líneas**
```shell
# Mostrar las 10 primeras líneas
head file.txt
# Mostrar una cantidad de líneas (5 por ejemplo):
head -n5 file.txt

# Mostrar las 10 últimas líneas, funciones como head
tail file.txt

# Conocer quién ha sido el último usuario registrado
tail -1 /etc/passwd
# Quien fue el primer usuario
head -n1 /etc/passwd

# Ejemplo de como monitorizar un archivo log (paquetes en este caso)
tail -f /var/log/dpkg.log  # CTRL + c para salir de este
```

**Filtrar texto**
`grep` -> Muestra sólo las líneas que cumplen con un patrón.
- `-i` -> No distingue entre mayúscula y minúscula.
- `-w` -> El patrón tiene que ser una palabra exacta.
- `-v` -> Muestra las que NO coinciden con el patrón.
- `-r` -> Busca en los ficheros de forma recursiva.
- `-n` -> Indica el número de línea.
- `-l` -> Indica solamente el fichero donde ha encontrado alguna coincidencia.
- `-c` -> La cantidad de líneas que cumplen con el patrón.
- `-B n`, `-A n` -> Muestra una cantidad de líneas n, antes (before) o después (after) de encontrar el patrón.
- `--color` -> Destaca el patrón en color dentro de la línea seleccionada.
- `^a` -> Empieza la línea por...
- `u$` -> ...termina la línea por...

Patrón -> `grep -opción palabra /path/archivo`

```shell
# Buscar una palabra en TODO los archivos de un directorio sin ir a niveles por debajo
grep root /etc/* 2> /dev/null  # enviar errores a nulo

# Buscar teniendo presente los subdirectorios (recursivo)
grep -r contrib /etc/* 2> /dev/null

# Buscar también la siguiente línea de la coincidencia
grep -w -A 1 1000 /etc/passwd

# Buscar también la anterior línea de la coincidencia
grep -w -B 1 1000 /etc/passwd
```

`cut` -> Corte "vertical", sólo una parte de cada línea.
- `-c` -> Selecciona sólo los caracteres que le indiquemos. Se pueden hacer delimitaciones.
	- `cut -c 5,10 file.txt` // `cut -c 7-25 file.txt`
- `-d` -> Indica un carácter separador entre los distintos campos de una línea. Por defecto buscará el tabulador.
- `-f` -> Elegir los campos que queremos que se muestren.

[Regresar a índice](#índice)

---

## Tuberías y redirecciones
### Redirecciones
``` shell
# Redirigir la salida del comando hacia un texto
echo "Hola mundo" > texto.txt
# Comprobar
cat texto.txt

# Si el archivo ya existe, añadir la salida al final de este.
echo "Hello world" >> texto.txt
```

Redirigir mensajes de *error* -> `2>`
Redirigir *todos* los mensajes -> `&>`
Desechar los resultados aquí -> `/dev/null`

### Tuberías
Redirigir el resultado hacia otro comando. Se pueden ir concatenando.
Se le llama "tubería" (pipe) ya que establece un "canal" por el que pasará el texto de un comando a otro.
``` shell
# Ejemplos
echo "HOLA MUNDO" | wc

grep -w ana /etc/passwd | cut -d":" -f1 | sort

ls -lr /etc/ | less

ls -lR /etc/ 2> /dev/null | grep shadow

ps -e | grep as
```

[Regresar a índice](#índice)

---
## Acciones sobre texto
### Ordenar con SORT
- Ordena de forma alfabética por defecto.

| Opciones | Tipo orden                         | Ejemplo                                                                                            |
| :------: | ---------------------------------- | -------------------------------------------------------------------------------------------------- |
|   `-r`   | Inversa                            | `sort -r archivo.txt`                                                                              |
|   `-t`   | Carácter de separación de columnas | `sort -t "," 2 archivo.txt`<br>Divide en el número de columnas dependiendo de cuantas "," existen. |
|   `-k`   | Columnas específicas               | `sort -t "," -k 2 archivo.txt`<br>Escoger la segunda columna de la división.                       |
|   `-n`   | Numéricamente                      | `sort -t "," -k 2 -n archivo.txt`                                                                  |
|          |                                    |                                                                                                    |
|   `-u`   | Eliminar líneas duplicadas         | `sort -u archivo.txt`                                                                              |

### Comando UNIQ
- Elimina las líneas repetidas contiguas en un archivo o entrada estàndar.

| Opciones | Tipo orden                         | Ejemplo                                           |
| :------: | ---------------------------------- | ------------------------------------------------- |
|   `-i`   | Ignora mayúsculas y minúsculas     | `cut -d"," -f1 nombres.txt \| sort \| uniq -i`    |
|   `-c`   | Cuenta la cantidad de repeticiones | `cut -d"," -f1 nombres.txt \| sort \| uniq -i -c` |
|   `-u`   | Muestra las no duplicadas          | `cut -d"," -f1 nombres.txt \| sort \| uniq -i -u` |

### Comando WC - Contar
- Contar líneas, palabras y bytes en archivos de texto o entradas estándar:
	- {Líneas} {Palabras} {Bytes}
		- Hay quien dice que el apartado de bytes se refiere al número de caracteres.
		- No es cierto porque si hacemos la comprobación con la "ñ", veremos que aumentan los bytes. -> consultar la pág. man de wc

| Opciones | Tipo orden |
| :------: | ---------- |
|   `-l`   | Líneas     |
|   `-w`   | Palabras   |
|   `-c`   | Bytes      |
|   `-m`   | Caracteres |

Ejemplos:
```shell
# Entrada estándar
echo "Hola" | wc

# Fichero
wc /etc/passwd
```

### Comando REV
Invertir el order de los caracteres en cada línea de un archivo de texto o entrada estándar.
- Útil cuando una línea se puede dividir en columnas y quiero convertir la última en la primera.

Ejemplos:
```shell
# Listado de todos los directorios
ls -lR ficheros | grep '/' | grep -v ^l | rev | cut -d"/" -f1 | rev
```

[Regresar a índice](#índice)

---

## Otros comandos para ficheros

### File -> Tipo de archivo
```shell
# Mostrar solo la descripción del tipo de archivo, su codificación.
file -b <archivo>

# Muestra el MIME del archivo.
file -i <archivo>

# Lee los nombres de los archivos desde un fichero
file -f <archivo_con_nombres>

# Ver los archivos y los tipos de cada uno en el directorio
file *
```

### LN -> Crear enlaces
Dos tipos:
- **Duro** -> Un puntero a la información del disco duro.
	- Aunque veamos que su peso es idéntico al archivo original, no es así.
		- Es un puntero a un archivo original.
		- Si borramos los enlaces, el peso seguirá siendo el mismo.
	- Comando `ls -lh`, la columna que hay después de los permisos, nos muestra el número de enlaces duros que tiene el archivo.
- **Blando** -> Se sustituye por la ruta del fichero original. Parecido a tener un "alias". Como un acceso directo.
	- Cuidado con poner rutas relativas sobretodo al mover archivos.

```shell
# Enlace duro:
ln <archivo> <ruta_y_nombre_archivo>

# Comprobar detalles del archivo
stat <archivo>

# Crear un enlace blando
ln -s <ruta_absoluta_archivo_original> <ruta_acceso_directo_archivo>
```

### DU -> Disk Usage
Espacio de disco ocupado por un fichero o por un directorio. Si le pasamos un directorio mostrará recursivamente el tamaño de todos los subdirectorios.
```shell
# Mostrar el tamaño del directorio y sus subdirectorios
du <ruta_directorio>

# Mostrar el tamaño en lectura humana
du -h

# Mostrar la cantidad total de lo que ocupa el directorio
du -s

# Mostrar la cantidad de todo lo que hay dentro del directorio
du -sh *
# Se puede concatenar con sort
du -sh * | sort -hr
```

### DF -> Disk Free
Información sobre las particiones del sistema: tamaño total, usado y libre.
```shell
# Ver el espacio disponible en el disco y sus particiones
df -h

# Verlo por tipo
df -hT

# Ver el tamaño de la partición donde está ubicado un archivo
df -h <ruta_archivo>
```
**Alternativa a DF**
```shell
sudo apt install dfc
# Ejecución
dfc
```

[Regresar a índice](#índice)

---

## Búsqueda en el sistema de ficheros
### LOCATE - Una primera búsqueda
```shell
# Instalar el paquete
sudo apt install locate

# Actualizar el sistema de ficheros con una base de datos para utilizar locate
su -
# Si se crea un archivo, un directorio, etc., actualizar la BD
updatedb

# Sintaxis locate
locate [opciones] [patrón]
```

| Opciones | Descripción                                    |
| :------: | ---------------------------------------------- |
|   `-i`   | Ignorar mayúsculas y minúsculas en la búsqueda |
|   `-c`   | Contar el número de coincidencias encontradas  |
|   `-n`   | Limitar el número de resultados.               |
|   `-r`   | Usar expresiones regulares para la búsqueda.   |

Ejemplos:
```shell
# Ignorando may y minus
locate -i "sources.list"

# Utilizando regex
locate -r "/sources.list$"
```

### Comando FIND
>[!danger]
>Comando super importante de dominar como administrador.

- Busca de forma recursiva en un directorio todos los ficheros que cumplan ciertas condiciones.
Sintaxis:
```shell
find [ruta] [opciones] [acción]
```

|       Opciones        | Descripción                                                                                                                                                                                                                                   |
| :-------------------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|        `-name`        | Especificar patrones para los nombres de los ficheros a buscar.                                                                                                                                                                               |
|       `-iname`        | Igual que `name` pero sin diferencias mayúsculas y minúsculas.                                                                                                                                                                                |
|        `-type`        | Tipo de fichero a buscar:<br>`d`-> directorios<br>`f` -> ficheros<br>`l` -> enlaces simbólicos<br>`b` -> dispositivos de bloque<br>`c` -> dispositivos de carácter<br>`p` -> para tuberías<br>`s` -> para sockets.                            |
|    `-size +/- <n>`    | `c` -> bytes<br>`k` -> kilobytes<br>`M` -> megabytes<br>`G` -> gigabytes                                                                                                                                                                      |
|     `-perm <num>`     | Ficheros cuyos permisos sean los expresados exactamente por número.<br>`-720` -> El signo `-` actuará como un AND: quiero exactamente esos permisos.<br>`/111` -> El signo `/` actuará como un OR: uno de los tres números debe de coincidir. |
|   `-user <usuario>`   | Especifica el usuario propietario del fichero                                                                                                                                                                                                 |
|  `-group <usuario>`   | Especifica el grupo propietario del fichero                                                                                                                                                                                                   |
|    `-mmin [+/-]n`     | Los datos del fichero fueron modificados por última vez hace `n` minutos.                                                                                                                                                                     |
|    `-mtime [+/-]n`    | Los datos del fichero fueron modificados por última vez hace `n` días.                                                                                                                                                                        |
| `-maxdepth <niveles>` | Desciende el número entero de niveles positivos en la jerarquía del directorio.                                                                                                                                                               |
|        `-path`        | Buscar por toda la ruta de los elementos. Se pueden utilizar comodines.                                                                                                                                                                       |
|       `-regex`        | Igual que `-path`, pero usando expresiones regulares.                                                                                                                                                                                         |

| Modificadores | Descripción                                                                        |
| :-----------: | ---------------------------------------------------------------------------------- |
|    `-not`     | Niega la condición que pongamos a continuación                                     |
 -> SHA-512
3. Cuándo fue cambiada por última vez -> Después de X días.
4. Mínimo de días que deben transcurrir hasta que pueda volver a cambiarse.
5. Máximo de días de validez de la contraseña.
6. Número de días durante los que avisará de que la contraseña va a caducar.
7. Días que pasarán desde que la clave caduca hasta que se deshabilita la cuenta.

```shell
# Comprobar que los últimos usuarios tienen contraseñas
tail /etc/shadow

# Introducir contraseña (aparecerá un prompt)
passwd <usuario>

# Deshabilitar un usuario de manera temporal -> lock
# En el /etc/shadow aparecerá con un !
passwd -l <usuario>

# Habilitar un usuario -> unlock
passwd -u <usuario>
```

`chage [parámetros] [usuario]` -> muestra información o cambia la validez de las contraseñas (sin parámetro muestra un asistente).

| Parámetros | Descripción                                                                             |
| :--------: | --------------------------------------------------------------------------------------- |
|    `-d`    | Establece el día del último cambio de la contraseña (en pasado y formato 'YYYY-MM-DD'). |
|    `-E`    | Establece la fecha de caducidad de la cuenta o de la contraseña.                        |
|    `-I`    | Deshabilita la cuenta después de X días de la fecha de caducidad.                       |
|    `-l`    | Muestra la información de la cuenta                                                     |
|    `-m`    | Número mínimo de días antes de cambiar la contraseña                                    |
|    `-M`    | Número máximo de días antes de cambiar la contraseña                                    |
|    `-W`    | Días de aviso de expiración                                                             |

### Grupos
`/etc/group/` -> Fichero donde se guardarán los grupos y quienes pertenecen a ellos de forma secundaria

`{grupo}:x:1002:{usuario1},{usuario2}`
1. Nombre del grupo
2. Contraseña en gshadow.
3. GID
4. Lista de nombre de usuarios separados por comas que pertenecen al grupo de forma secundaria.

`groupadd nombre_grupo` -> Crea un nuevo grupo.

`/etc/skel` -> El directorio por defecto cuyo contenido se copia a los nuevos directorios personales de los usuarios.

```shell
# comando para obtener información sobre usuarios, grupos, contraseñas, etc.
getent

# Consulta el nombre del fichero y el usuario
getent passwd <usuario>
getent group <grupo>

# Crea un grupo
newgrp <group>

# Borra un grupo
groupdel <grupo>

# Modifica un grupo
groupmod <grupo>

# Modifica un usuario
usermod <usuario>
# Añadir un usuario a un grupo
usermod -aG <grupo> <usuario>
# Cambiar todos los grupos del usuario por otro grupo
usermod -G <grupo> <usuario>

# Borrar usuario con sus directorios home y de email
userdel -r <usuario>
```

Gestión:
1. Creación de los grupos con `groupadd`
2. Creación de los usuarios con `useradd` mencionando la shell, el grupo principal, grupos secundarios y el usuario.
3. (Opcional) Realizar modificaciones de usuario con `usermod`.
4. (Opcional) Eliminación de usuario con su home y mail con `userdel`.

### Cambio de propietario o grupo de un elemento
`chgrp`-> Cambia de grupo de un elemento (fichero o directorio) -> Deprecated
`chown` -> Cambia el usuario y el grupo de uno o más elementos.
- La opción `-R` hace que sea recursivo.
```shell
# Cambia usuario y grupo de un elemento
chown usuario:grupo elemento

# Cambia solamente el usuario de un elemento
chown usuario elemento

# Cambia solamente el grupo de un elemento
chown :grupo elemento
```

### Seguridad
`sudo` -> Permite ejecutar ordenes con permisos de administrador.
`visudo` -> Gestionar los permisos que concede sudo (archivo `sudoers`).
Archivo `sudoers`:
- Sintaxis: `usuario_destino host=(usuario:grupo) comando/s`
	- Podemos usar diversos alias para agrupar usuarios, grupos, comandos, etc.
	- `ALL` -> Hacer referencia a "todos", por ejemplos usuarios y grupos.
`su -` -> Cambiar al usuario root con todos sus parámetros.

Logins:
- `who` -> Indica quién está identificado en el sistema.
- `w` -> Muestra quién hay y qué está ejecutando.
- `last` -> lista de los últimos accesos que ha tenido el sistema.

Ejemplos:
```shell
# Instalar sudo como root si no está disponible
apt install sudo

# Conceder permiso de sudo a un usuario (como root)
visudo

# Dentro del sudoers, editar
# Allow members of group sudo to execute any command
usuario ALL=(ALL:ALL) ALL

# Cambiar al grupo sudoers de manera temporal
newgrp sudo
```

### Permisos especiales
`SetUID` -> El programa se ejecutará con los permisos del usuario propietario del fichero y no con los permisos de quién invoca al programa.
- Se representa por la letra **s** como permiso de ejecución.
	- `chmod u+s archivo`

`SetGID` -> Igual que setUID pero con los permisos del grupo.
- En caso de ser directorio los elementos creados pertenecerán al grupo del directorio y no al grupo del usuario que crea el elemento.
	- `chmod g+s directorio`

`Sticky Bit` -> El directorio que lo tenga activado, los ficheros que contenga sólo podrán ser borrados por sus propietarios.
- Se representa por **t** en el permiso de ejecución de "los otros".
	- `chmod o+t directorio`

[Regresar a índice](#índice)

---
## Manejo de texto avanzado
### Comando TR
```shell
# Sustitución de caracteres
tr a e
# Input
casa
# Output
cese

# Ejemplo con un archivo
echo casa > fichero.txt
tr ac en < fichero.txt
# Output
nese
```

**Rangos**: `caracter_inicio-caracter_final`
`a-z` -> incluye todas las minúsculas
`d-g` -> incluye todas las letras `defg`

**Repeticiones**: `[caracter*numero]`
`[a*3]` -> indica la cadena `aaa`

| Caracteres especiales | Función                         |
| --------------------- | ------------------------------- |
| `\\`                  | Barra invertida.                |
| `\a`                  | Caracter audible (Campana/Bell) |
| `\b`                  | Retroceso                       |
| `\f`                  | Salto de página                 |
| `\n`                  | Nueva línea                     |
| `\r`                  | Retorno de carro                |
| `\t`                  | Tabulador                       |
| `\v`                  | Tabulador vertical              |

**Clases**: Representan un conjunto predefinido de caracteres.

| Clases         | Descripción                                              |
| -------------- | -------------------------------------------------------- |
| `[ :alnum: ]`  | Letras y dígitos                                         |
| `[ :alpha: ]`  | Letras                                                   |
| `[ :blank: ]`  | Espacios en Blanco                                       |
| `[ :cntrl: ]`  | Caracteres de control                                    |
| `[ :space: ]`  | Los Espacios en Blanco verticales y horizontales         |
| `[ :graph: ]`  | Caracteres imprimibles, sin incluir el Espacio en Blanco |
| `[ :print: ]`  | Caracteres imprimibles, incluyendo el Espacio en Blanco  |
| `[ :digit: ]`  | Dígitos                                                  |
| `[ :lower: ]`  | Letras minúsculas                                        |
| `[ :upper: ]`  | Letras mayúsculas                                        |
| `[ :punct: ]`  | Signos de puntuación                                     |
| `[ :xdigit: ]` | Dígitos Hexadecimales                                    |

Utilidades:
```shell
# Solamente quiero el nombre del fichero cuando ejecuto ls
# -s para tener presente las repeticiones.
ls -lh . | tr -s " " | cut -d" " -f9

# "Eliminar" las vocales de un archivo de texto en la salida
tr -d aeiou < texto.txt

# Pasar lo que esté en minúscula a mayúscula
tr [:lower:] [:upper:] < texto.txt
```

### Comando SED
Es un editor de flujo de texto -> `sed [-n] [-e'script'] [-f archivo] archivo1 archivo2

| Opciones | Descripción                                                                  |
| -------- | ---------------------------------------------------------------------------- |
| `-n`     | Indica que se suprima la salida estándar                                     |
| `-e`     | Indica que se ejecute el script que viene a continuación con el comando `-f` |
| `-f`     | Indica que las órdenes se tomarán de un archivo                              |
| `-r`     | Se utilizan expresiones regulares EXTENDIDAS                                 |

**Script**
```shell
# [inicio_ubicacion [,fin_ubicacion]] instruccion [argumentos]
sed '2,5p' texto.txt
```

**Ubicación**
- Si no se especifica se verán afectadas todas las lineas.
- Dos formas de especificación:
	1. Números: Los números de linea de principio y fin ("$" significa la última línea).
		`1,5` -> de la linea 1 a la 5
	2. Patrones de texto: son las líneas que cumplen con la expresión regular especificada /EXPRESIÓN/
		`/pepe/,# Comandos de Linux: desde cero hasta programar Shell Script

## Índice
- [Games to practice commands](#games-to-practice-commands)
- [Comandos del sistema de ficheros](#comandos-del-sistema-de-ficheros)
  - [Crear, mover, copiar y borrar ficheros y directorios](#crear-mover-copiar-y-borrar-ficheros-y-directorios)
  - [Atajos en la Shell](#atajos-en-la-shell)
  - [Uso de comodines](#uso-de-comodines)
- [Usuarios y Permisos](#usuarios-y-permisos)
  - [Permisos](#permisos)
    - [Cambiar permisos](#cambiar-permisos)
    - [Permisos usando números](#permisos-usando-números)
- [Administración de software](#administración-de-software)
  - [Comandos de gestión de software](#comandos-de-gestión-de-software)
- [Manejo de texto](#manejo-de-texto)
  - [Mostrar y filtrar texto](#mostrar-y-filtrar-texto)
- [Acciones sobre texto](#acciones-sobre-texto)
  - [Ordenar con SORT](#ordenar-con-sort)
  - [Comando UNIQ](#comando-uniq)
  - [Comando WC - Contar](#comando-wc---contar)
  - [Comando REV](#comando-rev)
- [Otros comandos para ficheros](#otros-comandos-para-ficheros)
  - [File -> Tipo de archivo](#file---tipo-de-archivo)
  - [LN -> Crear enlaces](#ln---crear-enlaces)
  - [DU -> Disk Usage](#du---disk-usage)
  - [DF -> Disk Free](#df---disk-free)
- [Búsqueda en el sistema de ficheros](#búsqueda-en-el-sistema-de-ficheros)
  - [LOCATE - Una primera búsqueda](#locate---una-primera-búsqueda)
  - [Comando FIND](#comando-find)
- [Ampliando conocimientos del Shell BASH](#ampliando-conocimientos-del-shell-bash)
  - [ALIAS](#alias)
  - [Sustitución de comandos](#sustitución-de-comandos)
- [Backups](#backups)
  - [Compactar y comprimir TAR](#compactar-y-comprimir-tar)
  - [Copias de seguridad totales, diferenciales e incrementales usando TAR](#copias-de-seguridad-totales-diferenciales-e-incrementales-usando-tar)
- [Gestión de paquetes con dpkg](#gestión-de-paquetes-con-dpkg)
- [Administración de Usuarios, Grupos y Contraseñas](#administración-de-usuarios-grupos-y-contraseñas)
  - [Usuarios](#usuarios)
  - [Contraseñas](#contraseñas)
  - [Grupos](#grupos)
  - [Cambio de propietario o grupo de un elemento](#cambio-de-propietario-o-grupo-de-un-elemento)
  - [Seguridad](#seguridad)
  - [Permisos especiales](#permisos-especiales)
- [Manejo de texto avanzado](#manejo-de-texto-avanzado)
  - [Comando TR](#comando-tr)
  - [Comando SED](#comando-sed)
- [Expresiones Regulares (Regex)](#expresiones-regulares-regex)

---
## Vídeos en YouTube
[Scheduling Tasks with Cron](https://youtu.be/7cbP7fzn0D8?si=Gk-bly6b5wPeRtsB)
[AWK](https://youtu.be/oPEnvuj9QrI?si=QESU7XS5yAe_TvqO)

---
## Games to practice commands
[Over the wire](https://overthewire.org/wargames/bandit/bandit0.html)
[Linux Survival](https://linuxsurvival.com)
[Practice Linux](https://www.practicelinux.com/home)

---
## Comandos del sistema de ficheros

```shell
# Ir al home del usuario
cd
cd ~
# Subir un nivel
cd ..
# Dos niveles
cd ../..

# Mostrar el contenido de uno o varios directorios
ls
# Format llistat
ls -l
# Pes en format llegible
ls -h
# Ordenar por el archivo + pesado
ls -S
# Ordenado al revés
ls -r
# Ordenado por archivo editado primero
ls -t
# Mostrar archivos ocultos
ls -a
```

Conocer el tipo de comando que utilizamos: `type comando`
Posibles resultados:
- orden interna -> built-in, como `cd`
- ejecutable -> /usr/bin/comando
- alias -> apodo de un comando más extenso

Cambiar a superusuario en Debian -> `su -`

### Crear, mover, copiar y borrar ficheros y directorios
```shell
# Crear carpeta
mkdir carpeta
# Crear dentro de un directorio ya existente
mkdir /ruta/carpeta/
# Crear carpetas padre -> hijo
mkdir -p carpeta/carpeta/

# Crear un fichero vacío
touch mi_fichero
# Alternativa
> mi_fichero2

# Mover o renombrar
mv mi_fichero mi_carta  # Renombra
mv mi_carta Documentos/  # Mueve y se podría renombrar

# Copiar: necesita dos parámetros
cp archivo_ruta_origen archivo_ruta_destino
# Copiar directorio y todo su contenido
cp -r directorio_ruta_origen directorio_ruta_destino
# Copiar varios elementos
cp fichero fichero ~  # Pega en la carpeta home

# Borrar fichero
rm fichero
# Borrar directorio vacío
rmdir directorio
# Borrar directorio y su contenido
rm -r directorio

# Ver la jerarquía de archivos y documentos
tree
tree ruta/
```

>[!tip] Copia de archivos que pertenecen a root
>Archivos que pertenecen a root podrían llegar a ser copiados por otros usuarios y luego llegar a ver el contenido que antes no se les permitía.
>
>En cambio si se mueve, el propietario se respeta y con los respectivos permisos.

### Atajos en la Shell

| Atajo                   | Descripción                                    |
| ----------------------- | ---------------------------------------------- |
| Cursores arriba y abajo | Historial de instrucciones                     |
| Tab                     | Completar la ruta de un elemento del sistema   |
| Doble Tab               | Opciones para el auto completado               |
| Ctrl + r                | Búsqueda de instrucciones                      |
| Ctrl + c                | Terminar un comando ejecutado a medias         |
| `history`               | Listado de comandos ejecutados                 |
| `!número`               | Llamar a uno de los comandos de `history`      |
| `sudo !!`               | Ejecutar la última instrucción con privilegios |
| Ctrl + a                | Ir al inicio del comando escrito               |
| Ctrl + e                | Ir al final del comando escrito                |
| Ctrl + l                | borrar la pantalla                             |

### Uso de comodines
Asterisco -> "Cualquier cosa"
```shell
# Hacer referencia a los archivos que tienen una extensión en común, ejemplo:
rm *.gif
# Todo!
rm *

# Cualquier archivo o carpeta cuyo nombre contenga la letra 'u'
cp *u* seleccion/
# O que además en la extensión tengan una 'i'
cp *u*.*i* seleccion/
# Que por lo menos exista una extensión (ficheros)
cp *.*
```

Interrogación -> "Cualquier carácter y cualquier longitud de cadena de caracteres o ninguna"
```shell
# Caracteres obligatorios, cualquier carácter OBLIGATORIO en la posición y lo que siga O NO.
ls c?sa*
# Obligación de una cifra
ls informe-200?.txt
ls informe-19??.txt
# Cualquier longitud de nombre hasta el punto + obligación en el último carácter de la extensión
ls c*.tx?
```

Corchetes -> opciones obligatorias en una posición
```shell
# Elige cualquiera de los tres caracteres marcados en corchetes.
ls c[aio]s*

# Rangos
ls c[a-m]s*
# Mayúsculas
ls c[A-Z]s*
# Números
ls informe-200[1-9].txt
```

> [!tip]
> Resultará útil consultar `man bash` y buscar `Concordancia`:
> /Concordancia + n (seguir buscando hasta llegar a la sección)

[Regresar a índice](#índice)

---
## Usuarios y Permisos
Cuando se crea un usuario, si no se especifica, se crea un grupo con el mismo nombre.

Quién soy y a qué grupos pertenezco -> `id`

| Características | Descripción                                       |
| --------------- | ------------------------------------------------- |
| uid             | identificador del usuario                         |
| gid             | grupo principal al que pertenece el usuario       |
| grupos          | grupos secundarios a los que pertenece el usuario |

Comandos básicos de ejecución:

| Comando                | Descripción                   |
| ---------------------- | ----------------------------- |
| `su usuario`           | Cambio de usuario             |
| `su root -c "comando"` | Ejecutar un comando como root |
| `passwd`               | Cambiar la contraseña         |

Se necessita ser _root_ para crear usuarios y grupos:

| Comando                                      | Descripción                                       |
| -------------------------------------------- | ------------------------------------------------- |
| `groupadd nombre_grupo`                      | Crear grupo                                       |
| `useradd -m nombre_usuario`                  | Crear usuario con su propio grupo                 |
| `useradd -m -g grupo_asignar nombre_usuario` | Crear usuario y asignar a un grupo                |
| `adduser nombre_usuario`                     | Crear usuario con prompts administrativos         |
| `userdel -r nombre_usuario`                  | Eliminar usuario y su directorio home             |
| `groupdel nombre_grupo`                      | Eliminar el grupo (asegurarse que no está en uso) |

### Permisos

| Permisos | Descripción                         |
| -------- | ----------------------------------- |
| `-`      | No tiene permisos                   |
| `r`      | read -> puede ver pero no modificar |
| `w`      | write -> puede modificar            |
| `x`      | execute -> puede ejecutar           |

`-rwxrwxrwx`: permisos por posiciones
- 1r grupo: permisos de usuario
- 2o grupo: permisos del grupo propietario del fichero
- 3r grupo: permisos del resto de usuarios del sistema

|                |   R    |                   W                   |                  X                   |
| -------------- | :----: | :-----------------------------------: | :----------------------------------: |
| **Fichero**    |  Leer  |               Escribir                | Ejecutar<br>(programa, script, etc.) |
| **Directorio** | Listar | Modificar el contenido del directorio |               Acceder                |

#### Cambiar permisos
`chmod`
- Letra del permiso. Si son varios permisos, separarlas por comas.
	- Si no se pone nada, se realiza en todos.
- + para añadir permiso / - para quitar permiso
- Ruta del elemento a cambiar permisos
- Indicación de a quién asignarlo: u=User, g=Group, o=Others

Ejemplos:
```shell
# Añadir permiso de escritura al grupo
chmod g+w ruta

# Quitar el permiso de ejecución a todos
chmod -x ruta

# Añadir permiso de escritura a usuario y a otros
chmod u+w,o+w ruta

# Quitar el permiso de ejecución y escritura al usuario y poner el de escritura al grupo
chmod u-xw,g+w ruta
```

#### Permisos usando números

| Permission Type | Read (4) | Write (2) | Execute (1) |
| --------------- | -------- | --------- | ----------- |
| User            | 1 / 0    | 1 / 0     | 1 / 0       |
| Group           | 1 / 0    | 1 / 0     | 1 / 0       |
| Others          | 1 / 0    | 1 / 0     | 1 / 0       |

Si quiero poner permisos al usuario de lectura y escritura, al grupo sólo lectura y al resto nada:
- Binario -> `110 100 000`
- Decimal -> `640`
- Comando -> `chmod 640 fichero`
En este tipo de cambio, actuará en los tres grupos.

[Regresar a índice](#índice)

---

## Administración de software
En Debian 12, se ha realizado el cambio de que en una instalación limpia se consiguen los paquetes a través de la ISO.
Hasta el día de hoy Debian utilizaba un solo archivo para obtener los repositorios -> `/etc/apt/sources.list`.
A partir de Debian 12 se ha modularizado -> `/etc/sources.list.d/`
Y se debe de añadir un servicio CDN para que funcione el comando APT.

>[!info]
>Es importante conocer el nombre en clave de la distribución -> Debian 12 = Bookworm

**Cómo añadir las fuentes para el APT en un archivo**
[Instrucciones de la web oficial de Debian -> 4.3.1](https://www.debian.org/releases/trixie/release-notes/upgrading.es.html#adding-apt-internet-sources)
1. Nos dirigimos a `/etc/apt/sources.list`
2. Comentamos la linea que especifica que utilizará los paquetes del CD.
3. Añadimos la siguiente linea: `deb https://deb.debian.org/debian bookworm main contrib`

**Cómo añadir las fuentes APT de manera modularizada**
1. Comentamos `/etc/apt/sources.list`
2. `nano /etc/apt/sources.list.d/repo_oficial.list
3. Añadimos `deb https://deb.debian.org/debian bookworm main contrib`

### Comandos de gestión de software

| `apt`        | Descripción                                                                                                    |
| ------------ | -------------------------------------------------------------------------------------------------------------- |
| `update`     | Actualiza la lista de paquetes disponibles en los repositorios                                                 |
| `search`     | Busca paquetes disponibles en los repositorios que coincidan con un término de búsqueda.                       |
| `show`       | Muestra información detallada sobre un paquete específico.                                                     |
| `list`       | Lista paquetes según criterios, como los instalados, actualizables o que cumplen cierta condición de búsqueda. |
| `install`    | Instala un paquete de software en el sistema.                                                                  |
| `remove`     | Elimina un paquete del sistema, manteniendo los archivos de configuración.                                     |
| `purge`      | Elimina un paquete del sistema junto con sus archivos de configuración.                                        |
| `autoremove` | Elimina paquetes que no tienen dependencias.                                                                   |

[Regresar a índice](#índice)

---
## Manejo de texto
>[!warning]
>**¿Por qué es importante?**
>Configuraciones se guardan en ficheros de texto.
>Los logs también son en formato texto.
>Cualquier programa en modo consola que muestra resultados mediante texto.

>[!info]
>Dominar esto ayuda muchísimo como administrador del servidor.

### Mostrar y filtrar texto

**Mostrar texto**
```shell
# Mostrar texto que se recibe
echo "Hola Mundo"

# Interpretar ordenes:
# Salto de línea
echo -e "Hola \nMundo"
# Tabulación
echo -e "Hola\tMundo"

# Ver las variables del sistema
echo $PATH
echo "Soy el usuario: $USER"

# Las comillas simples no interpretarán variables:
echo 'La variable del sistema que guarda el usuario es: $USER'

# Mostrar un fichero de texto
cat /etc/issue
# Mostrar con número de línea en el archivo
cat -n /etc/fstab
```

**Mostrar contenido de ficheros de texto**
```shell
# Mostrar el texto poco a poco -> 'h' muestra la ayuda
more archivo.ext
# Su mejora para los archivos grandes y con una "ventana nueva"
less archivo.ext
```
Búsqueda dentro del comando 'less':
/palabra + enter -> buscar coincidencias hacia delante.
?palabra + enter -> buscar coincidencias hacia atrás.
g -> Ir al principio del documento.
G -> Ir al final del documento.
:10 + g -> Ir a un número de línea concreto.

**Primeras y últimas líneas**
```shell
# Mostrar las 10 primeras líneas
head file.txt
# Mostrar una cantidad de líneas (5 por ejemplo):
head -n5 file.txt

# Mostrar las 10 últimas líneas, funciones como head
tail file.txt

# Conocer quién ha sido el último usuario registrado
tail -1 /etc/passwd
# Quien fue el primer usuario
head -n1 /etc/passwd

# Ejemplo de como monitorizar un archivo log (paquetes en este caso)
tail -f /var/log/dpkg.log  # CTRL + c para salir de este
```

**Filtrar texto**
`grep` -> Muestra sólo las líneas que cumplen con un patrón.
- `-i` -> No distingue entre mayúscula y minúscula.
- `-w` -> El patrón tiene que ser una palabra exacta.
- `-v` -> Muestra las que NO coinciden con el patrón.
- `-r` -> Busca en los ficheros de forma recursiva.
- `-n` -> Indica el número de línea.
- `-l` -> Indica solamente el fichero donde ha encontrado alguna coincidencia.
- `-c` -> La cantidad de líneas que cumplen con el patrón.
- `-B n`, `-A n` -> Muestra una cantidad de líneas n, antes (before) o después (after) de encontrar el patrón.
- `--color` -> Destaca el patrón en color dentro de la línea seleccionada.
- `^a` -> Empieza la línea por...
- `u$` -> ...termina la línea por...

Patrón -> `grep -opción palabra /path/archivo`

```shell
# Buscar una palabra en TODO los archivos de un directorio sin ir a niveles por debajo
grep root /etc/* 2> /dev/null  # enviar errores a nulo

# Buscar teniendo presente los subdirectorios (recursivo)
grep -r contrib /etc/* 2> /dev/null

# Buscar también la siguiente línea de la coincidencia
grep -w -A 1 1000 /etc/passwd

# Buscar también la anterior línea de la coincidencia
grep -w -B 1 1000 /etc/passwd
```

`cut` -> Corte "vertical", sólo una parte de cada línea.
- `-c` -> Selecciona sólo los caracteres que le indiquemos. Se pueden hacer delimitaciones.
	- `cut -c 5,10 file.txt` // `cut -c 7-25 file.txt`
- `-d` -> Indica un carácter separador entre los distintos campos de una línea. Por defecto buscará el tabulador.
- `-f` -> Elegir los campos que queremos que se muestren.

[Regresar a índice](#índice)

---

## Tuberías y redirecciones
### Redirecciones
``` shell
# Redirigir la salida del comando hacia un texto
echo "Hola mundo" > texto.txt
# Comprobar
cat texto.txt

# Si el archivo ya existe, añadir la salida al final de este.
echo "Hello world" >> texto.txt
```

Redirigir mensajes de *error* -> `2>`
Redirigir *todos* los mensajes -> `&>`
Desechar los resultados aquí -> `/dev/null`

### Tuberías
Redirigir el resultado hacia otro comando. Se pueden ir concatenando.
Se le llama "tubería" (pipe) ya que establece un "canal" por el que pasará el texto de un comando a otro.
``` shell
# Ejemplos
echo "HOLA MUNDO" | wc

grep -w ana /etc/passwd | cut -d":" -f1 | sort

ls -lr /etc/ | less

ls -lR /etc/ 2> /dev/null | grep shadow

ps -e | grep as
```

[Regresar a índice](#índice)

---
## Acciones sobre texto
### Ordenar con SORT
- Ordena de forma alfabética por defecto.

| Opciones | Tipo orden                         | Ejemplo                                                                                            |
| :------: | ---------------------------------- | -------------------------------------------------------------------------------------------------- |
|   `-r`   | Inversa                            | `sort -r archivo.txt`                                                                              |
|   `-t`   | Carácter de separación de columnas | `sort -t "," 2 archivo.txt`<br>Divide en el número de columnas dependiendo de cuantas "," existen. |
|   `-k`   | Columnas específicas               | `sort -t "," -k 2 archivo.txt`<br>Escoger la segunda columna de la división.                       |
|   `-n`   | Numéricamente                      | `sort -t "," -k 2 -n archivo.txt`                                                                  |
|          |                                    |                                                                                                    |
|   `-u`   | Eliminar líneas duplicadas         | `sort -u archivo.txt`                                                                              |

### Comando UNIQ
- Elimina las líneas repetidas contiguas en un archivo o entrada estàndar.

| Opciones | Tipo orden                         | Ejemplo                                           |
| :------: | ---------------------------------- | ------------------------------------------------- |
|   `-i`   | Ignora mayúsculas y minúsculas     | `cut -d"," -f1 nombres.txt \| sort \| uniq -i`    |
|   `-c`   | Cuenta la cantidad de repeticiones | `cut -d"," -f1 nombres.txt \| sort \| uniq -i -c` |
|   `-u`   | Muestra las no duplicadas          | `cut -d"," -f1 nombres.txt \| sort \| uniq -i -u` |

### Comando WC - Contar
- Contar líneas, palabras y bytes en archivos de texto o entradas estándar:
	- {Líneas} {Palabras} {Bytes}
		- Hay quien dice que el apartado de bytes se refiere al número de caracteres.
		- No es cierto porque si hacemos la comprobación con la "ñ", veremos que aumentan los bytes. -> consultar la pág. man de wc

| Opciones | Tipo orden |
| :------: | ---------- |
|   `-l`   | Líneas     |
|   `-w`   | Palabras   |
|   `-c`   | Bytes      |
|   `-m`   | Caracteres |

Ejemplos:
```shell
# Entrada estándar
echo "Hola" | wc

# Fichero
wc /etc/passwd
```

### Comando REV
Invertir el order de los caracteres en cada línea de un archivo de texto o entrada estándar.
- Útil cuando una línea se puede dividir en columnas y quiero convertir la última en la primera.

Ejemplos:
```shell
# Listado de todos los directorios
ls -lR ficheros | grep '/' | grep -v ^l | rev | cut -d"/" -f1 | rev
```

[Regresar a índice](#índice)

---

## Otros comandos para ficheros

### File -> Tipo de archivo
```shell
# Mostrar solo la descripción del tipo de archivo, su codificación.
file -b <archivo>

# Muestra el MIME del archivo.
file -i <archivo>

# Lee los nombres de los archivos desde un fichero
file -f <archivo_con_nombres>

# Ver los archivos y los tipos de cada uno en el directorio
file *
```

### LN -> Crear enlaces
Dos tipos:
- **Duro** -> Un puntero a la información del disco duro.
	- Aunque veamos que su peso es idéntico al archivo original, no es así.
		- Es un puntero a un archivo original.
		- Si borramos los enlaces, el peso seguirá siendo el mismo.
	- Comando `ls -lh`, la columna que hay después de los permisos, nos muestra el número de enlaces duros que tiene el archivo.
- **Blando** -> Se sustituye por la ruta del fichero original. Parecido a tener un "alias". Como un acceso directo.
	- Cuidado con poner rutas relativas sobretodo al mover archivos.

```shell
# Enlace duro:
ln <archivo> <ruta_y_nombre_archivo>

# Comprobar detalles del archivo
stat <archivo>

# Crear un enlace blando
ln -s <ruta_absoluta_archivo_original> <ruta_acceso_directo_archivo>
```

### DU -> Disk Usage
Espacio de disco ocupado por un fichero o por un directorio. Si le pasamos un directorio mostrará recursivamente el tamaño de todos los subdirectorios.
```shell
# Mostrar el tamaño del directorio y sus subdirectorios
du <ruta_directorio>

# Mostrar el tamaño en lectura humana
du -h

# Mostrar la cantidad total de lo que ocupa el directorio
du -s

# Mostrar la cantidad de todo lo que hay dentro del directorio
du -sh *
# Se puede concatenar con sort
du -sh * | sort -hr
```

### DF -> Disk Free
Información sobre las particiones del sistema: tamaño total, usado y libre.
```shell
# Ver el espacio disponible en el disco y sus particiones
df -h

# Verlo por tipo
df -hT

# Ver el tamaño de la partición donde está ubicado un archivo
df -h <ruta_archivo>
```
**Alternativa a DF**
```shell
sudo apt install dfc
# Ejecución
dfc
```

[Regresar a índice](#índice)

---

## Búsqueda en el sistema de ficheros
### LOCATE - Una primera búsqueda
```shell
# Instalar el paquete
sudo apt install locate

# Actualizar el sistema de ficheros con una base de datos para utilizar locate
su -
# Si se crea un archivo, un directorio, etc., actualizar la BD
updatedb

# Sintaxis locate
locate [opciones] [patrón]
```

| Opciones | Descripción                                    |
| :------: | ---------------------------------------------- |
|   `-i`   | Ignorar mayúsculas y minúsculas en la búsqueda |
|   `-c`   | Contar el número de coincidencias encontradas  |
|   `-n`   | Limitar el número de resultados.               |
|   `-r`   | Usar expresiones regulares para la búsqueda.   |

Ejemplos:
```shell
# Ignorando may y minus
locate -i "sources.list"

# Utilizando regex
locate -r "/sources.list$"
```

### Comando FIND
>[!danger]
>Comando super importante de dominar como administrador.

- Busca de forma recursiva en un directorio todos los ficheros que cumplan ciertas condiciones.
Sintaxis:
```shell
find [ruta] [opciones] [acción]
```

|       Opciones        | Descripción                                                                                                                                                                                                                                   |
| :-------------------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|        `-name`        | Especificar patrones para los nombres de los ficheros a buscar.                                                                                                                                                                               |
|       `-iname`        | Igual que `name` pero sin diferencias mayúsculas y minúsculas.                                                                                                                                                                                |
|        `-type`        | Tipo de fichero a buscar:<br>`d`-> directorios<br>`f` -> ficheros<br>`l` -> enlaces simbólicos<br>`b` -> dispositivos de bloque<br>`c` -> dispositivos de carácter<br>`p` -> para tuberías<br>`s` -> para sockets.                            |
|    `-size +/- <n>`    | `c` -> bytes<br>`k` -> kilobytes<br>`M` -> megabytes<br>`G` -> gigabytes                                                                                                                                                                      |
|     `-perm <num>`     | Ficheros cuyos permisos sean los expresados exactamente por número.<br>`-720` -> El signo `-` actuará como un AND: quiero exactamente esos permisos.<br>`/111` -> El signo `/` actuará como un OR: uno de los tres números debe de coincidir. |
|   `-user <usuario>`   | Especifica el usuario propietario del fichero                                                                                                                                                                                                 |
|  `-group <usuario>`   | Especifica el grupo propietario del fichero                                                                                                                                                                                                   |
|    `-mmin [+/-]n`     | Los datos del fichero fueron modificados por última vez hace `n` minutos.                                                                                                                                                                     |
|    `-mtime [+/-]n`    | Los datos del fichero fueron modificados por última vez hace `n` días.                                                                                                                                                                        |
| `-maxdepth <niveles>` | Desciende el número entero de niveles positivos en la jerarquía del directorio.                                                                                                                                                               |
|        `-path`        | Buscar por toda la ruta de los elementos. Se pueden utilizar comodines.                                                                                                                                                                       |
|       `-regex`        | Igual que `-path`, pero usando expresiones regulares.                                                                                                                                                                                         |

| Modificadores | Descripción                                                                        |
| :-----------: | ---------------------------------------------------------------------------------- |
|    `-not`     | Niega la condición que pongamos a continuación                                     |
 -> desde la línea que contiene la palabra hasta la última linea.

| Instrucciones | Función                                                             |
| :-----------: | ------------------------------------------------------------------- |
|     `i\`      | Insertar línea antes de la línea actual                             |
|     `a\`      | Insertar línea después de la línea actual                           |
|     `c\`      | Sustituye la línea actual por la especificada a continuación        |
|      `d`      | Borrar la línea actual.                                             |
|      `p`      | Imprimir línea actual en salida estándar.                           |
|      `s`      | Sustituir cadena en línea actual.                                   |
|      `!`      | Aplicar instrucción a las líneas no seleccionadas por la condición. |
|      `q`      | Abandona el proceso cuando se alcanza la línea especificada.        |

[Regresar a índice](#índice)

---
## Expresiones Regulares (Regex)

| Caracter especial | Descripción                                                                             |
| ----------------- | --------------------------------------------------------------------------------------- |
| `^`               | Inicio de línea                                                                         |
| `# Comandos de Linux: desde cero hasta programar Shell Script

## Índice
- [Games to practice commands](#games-to-practice-commands)
- [Comandos del sistema de ficheros](#comandos-del-sistema-de-ficheros)
  - [Crear, mover, copiar y borrar ficheros y directorios](#crear-mover-copiar-y-borrar-ficheros-y-directorios)
  - [Atajos en la Shell](#atajos-en-la-shell)
  - [Uso de comodines](#uso-de-comodines)
- [Usuarios y Permisos](#usuarios-y-permisos)
  - [Permisos](#permisos)
    - [Cambiar permisos](#cambiar-permisos)
    - [Permisos usando números](#permisos-usando-números)
- [Administración de software](#administración-de-software)
  - [Comandos de gestión de software](#comandos-de-gestión-de-software)
- [Manejo de texto](#manejo-de-texto)
  - [Mostrar y filtrar texto](#mostrar-y-filtrar-texto)
- [Acciones sobre texto](#acciones-sobre-texto)
  - [Ordenar con SORT](#ordenar-con-sort)
  - [Comando UNIQ](#comando-uniq)
  - [Comando WC - Contar](#comando-wc---contar)
  - [Comando REV](#comando-rev)
- [Otros comandos para ficheros](#otros-comandos-para-ficheros)
  - [File -> Tipo de archivo](#file---tipo-de-archivo)
  - [LN -> Crear enlaces](#ln---crear-enlaces)
  - [DU -> Disk Usage](#du---disk-usage)
  - [DF -> Disk Free](#df---disk-free)
- [Búsqueda en el sistema de ficheros](#búsqueda-en-el-sistema-de-ficheros)
  - [LOCATE - Una primera búsqueda](#locate---una-primera-búsqueda)
  - [Comando FIND](#comando-find)
- [Ampliando conocimientos del Shell BASH](#ampliando-conocimientos-del-shell-bash)
  - [ALIAS](#alias)
  - [Sustitución de comandos](#sustitución-de-comandos)
- [Backups](#backups)
  - [Compactar y comprimir TAR](#compactar-y-comprimir-tar)
  - [Copias de seguridad totales, diferenciales e incrementales usando TAR](#copias-de-seguridad-totales-diferenciales-e-incrementales-usando-tar)
- [Gestión de paquetes con dpkg](#gestión-de-paquetes-con-dpkg)
- [Administración de Usuarios, Grupos y Contraseñas](#administración-de-usuarios-grupos-y-contraseñas)
  - [Usuarios](#usuarios)
  - [Contraseñas](#contraseñas)
  - [Grupos](#grupos)
  - [Cambio de propietario o grupo de un elemento](#cambio-de-propietario-o-grupo-de-un-elemento)
  - [Seguridad](#seguridad)
  - [Permisos especiales](#permisos-especiales)
- [Manejo de texto avanzado](#manejo-de-texto-avanzado)
  - [Comando TR](#comando-tr)
  - [Comando SED](#comando-sed)
- [Expresiones Regulares (Regex)](#expresiones-regulares-regex)

---
## Vídeos en YouTube
[Scheduling Tasks with Cron](https://youtu.be/7cbP7fzn0D8?si=Gk-bly6b5wPeRtsB)
[AWK](https://youtu.be/oPEnvuj9QrI?si=QESU7XS5yAe_TvqO)

---
## Games to practice commands
[Over the wire](https://overthewire.org/wargames/bandit/bandit0.html)
[Linux Survival](https://linuxsurvival.com)
[Practice Linux](https://www.practicelinux.com/home)

---
## Comandos del sistema de ficheros

```shell
# Ir al home del usuario
cd
cd ~
# Subir un nivel
cd ..
# Dos niveles
cd ../..

# Mostrar el contenido de uno o varios directorios
ls
# Format llistat
ls -l
# Pes en format llegible
ls -h
# Ordenar por el archivo + pesado
ls -S
# Ordenado al revés
ls -r
# Ordenado por archivo editado primero
ls -t
# Mostrar archivos ocultos
ls -a
```

Conocer el tipo de comando que utilizamos: `type comando`
Posibles resultados:
- orden interna -> built-in, como `cd`
- ejecutable -> /usr/bin/comando
- alias -> apodo de un comando más extenso

Cambiar a superusuario en Debian -> `su -`

### Crear, mover, copiar y borrar ficheros y directorios
```shell
# Crear carpeta
mkdir carpeta
# Crear dentro de un directorio ya existente
mkdir /ruta/carpeta/
# Crear carpetas padre -> hijo
mkdir -p carpeta/carpeta/

# Crear un fichero vacío
touch mi_fichero
# Alternativa
> mi_fichero2

# Mover o renombrar
mv mi_fichero mi_carta  # Renombra
mv mi_carta Documentos/  # Mueve y se podría renombrar

# Copiar: necesita dos parámetros
cp archivo_ruta_origen archivo_ruta_destino
# Copiar directorio y todo su contenido
cp -r directorio_ruta_origen directorio_ruta_destino
# Copiar varios elementos
cp fichero fichero ~  # Pega en la carpeta home

# Borrar fichero
rm fichero
# Borrar directorio vacío
rmdir directorio
# Borrar directorio y su contenido
rm -r directorio

# Ver la jerarquía de archivos y documentos
tree
tree ruta/
```

>[!tip] Copia de archivos que pertenecen a root
>Archivos que pertenecen a root podrían llegar a ser copiados por otros usuarios y luego llegar a ver el contenido que antes no se les permitía.
>
>En cambio si se mueve, el propietario se respeta y con los respectivos permisos.

### Atajos en la Shell

| Atajo                   | Descripción                                    |
| ----------------------- | ---------------------------------------------- |
| Cursores arriba y abajo | Historial de instrucciones                     |
| Tab                     | Completar la ruta de un elemento del sistema   |
| Doble Tab               | Opciones para el auto completado               |
| Ctrl + r                | Búsqueda de instrucciones                      |
| Ctrl + c                | Terminar un comando ejecutado a medias         |
| `history`               | Listado de comandos ejecutados                 |
| `!número`               | Llamar a uno de los comandos de `history`      |
| `sudo !!`               | Ejecutar la última instrucción con privilegios |
| Ctrl + a                | Ir al inicio del comando escrito               |
| Ctrl + e                | Ir al final del comando escrito                |
| Ctrl + l                | borrar la pantalla                             |

### Uso de comodines
Asterisco -> "Cualquier cosa"
```shell
# Hacer referencia a los archivos que tienen una extensión en común, ejemplo:
rm *.gif
# Todo!
rm *

# Cualquier archivo o carpeta cuyo nombre contenga la letra 'u'
cp *u* seleccion/
# O que además en la extensión tengan una 'i'
cp *u*.*i* seleccion/
# Que por lo menos exista una extensión (ficheros)
cp *.*
```

Interrogación -> "Cualquier carácter y cualquier longitud de cadena de caracteres o ninguna"
```shell
# Caracteres obligatorios, cualquier carácter OBLIGATORIO en la posición y lo que siga O NO.
ls c?sa*
# Obligación de una cifra
ls informe-200?.txt
ls informe-19??.txt
# Cualquier longitud de nombre hasta el punto + obligación en el último carácter de la extensión
ls c*.tx?
```

Corchetes -> opciones obligatorias en una posición
```shell
# Elige cualquiera de los tres caracteres marcados en corchetes.
ls c[aio]s*

# Rangos
ls c[a-m]s*
# Mayúsculas
ls c[A-Z]s*
# Números
ls informe-200[1-9].txt
```

> [!tip]
> Resultará útil consultar `man bash` y buscar `Concordancia`:
> /Concordancia + n (seguir buscando hasta llegar a la sección)

[Regresar a índice](#índice)

---
## Usuarios y Permisos
Cuando se crea un usuario, si no se especifica, se crea un grupo con el mismo nombre.

Quién soy y a qué grupos pertenezco -> `id`

| Características | Descripción                                       |
| --------------- | ------------------------------------------------- |
| uid             | identificador del usuario                         |
| gid             | grupo principal al que pertenece el usuario       |
| grupos          | grupos secundarios a los que pertenece el usuario |

Comandos básicos de ejecución:

| Comando                | Descripción                   |
| ---------------------- | ----------------------------- |
| `su usuario`           | Cambio de usuario             |
| `su root -c "comando"` | Ejecutar un comando como root |
| `passwd`               | Cambiar la contraseña         |

Se necessita ser _root_ para crear usuarios y grupos:

| Comando                                      | Descripción                                       |
| -------------------------------------------- | ------------------------------------------------- |
| `groupadd nombre_grupo`                      | Crear grupo                                       |
| `useradd -m nombre_usuario`                  | Crear usuario con su propio grupo                 |
| `useradd -m -g grupo_asignar nombre_usuario` | Crear usuario y asignar a un grupo                |
| `adduser nombre_usuario`                     | Crear usuario con prompts administrativos         |
| `userdel -r nombre_usuario`                  | Eliminar usuario y su directorio home             |
| `groupdel nombre_grupo`                      | Eliminar el grupo (asegurarse que no está en uso) |

### Permisos

| Permisos | Descripción                         |
| -------- | ----------------------------------- |
| `-`      | No tiene permisos                   |
| `r`      | read -> puede ver pero no modificar |
| `w`      | write -> puede modificar            |
| `x`      | execute -> puede ejecutar           |

`-rwxrwxrwx`: permisos por posiciones
- 1r grupo: permisos de usuario
- 2o grupo: permisos del grupo propietario del fichero
- 3r grupo: permisos del resto de usuarios del sistema

|                |   R    |                   W                   |                  X                   |
| -------------- | :----: | :-----------------------------------: | :----------------------------------: |
| **Fichero**    |  Leer  |               Escribir                | Ejecutar<br>(programa, script, etc.) |
| **Directorio** | Listar | Modificar el contenido del directorio |               Acceder                |

#### Cambiar permisos
`chmod`
- Letra del permiso. Si son varios permisos, separarlas por comas.
	- Si no se pone nada, se realiza en todos.
- + para añadir permiso / - para quitar permiso
- Ruta del elemento a cambiar permisos
- Indicación de a quién asignarlo: u=User, g=Group, o=Others

Ejemplos:
```shell
# Añadir permiso de escritura al grupo
chmod g+w ruta

# Quitar el permiso de ejecución a todos
chmod -x ruta

# Añadir permiso de escritura a usuario y a otros
chmod u+w,o+w ruta

# Quitar el permiso de ejecución y escritura al usuario y poner el de escritura al grupo
chmod u-xw,g+w ruta
```

#### Permisos usando números

| Permission Type | Read (4) | Write (2) | Execute (1) |
| --------------- | -------- | --------- | ----------- |
| User            | 1 / 0    | 1 / 0     | 1 / 0       |
| Group           | 1 / 0    | 1 / 0     | 1 / 0       |
| Others          | 1 / 0    | 1 / 0     | 1 / 0       |

Si quiero poner permisos al usuario de lectura y escritura, al grupo sólo lectura y al resto nada:
- Binario -> `110 100 000`
- Decimal -> `640`
- Comando -> `chmod 640 fichero`
En este tipo de cambio, actuará en los tres grupos.

[Regresar a índice](#índice)

---

## Administración de software
En Debian 12, se ha realizado el cambio de que en una instalación limpia se consiguen los paquetes a través de la ISO.
Hasta el día de hoy Debian utilizaba un solo archivo para obtener los repositorios -> `/etc/apt/sources.list`.
A partir de Debian 12 se ha modularizado -> `/etc/sources.list.d/`
Y se debe de añadir un servicio CDN para que funcione el comando APT.

>[!info]
>Es importante conocer el nombre en clave de la distribución -> Debian 12 = Bookworm

**Cómo añadir las fuentes para el APT en un archivo**
[Instrucciones de la web oficial de Debian -> 4.3.1](https://www.debian.org/releases/trixie/release-notes/upgrading.es.html#adding-apt-internet-sources)
1. Nos dirigimos a `/etc/apt/sources.list`
2. Comentamos la linea que especifica que utilizará los paquetes del CD.
3. Añadimos la siguiente linea: `deb https://deb.debian.org/debian bookworm main contrib`

**Cómo añadir las fuentes APT de manera modularizada**
1. Comentamos `/etc/apt/sources.list`
2. `nano /etc/apt/sources.list.d/repo_oficial.list
3. Añadimos `deb https://deb.debian.org/debian bookworm main contrib`

### Comandos de gestión de software

| `apt`        | Descripción                                                                                                    |
| ------------ | -------------------------------------------------------------------------------------------------------------- |
| `update`     | Actualiza la lista de paquetes disponibles en los repositorios                                                 |
| `search`     | Busca paquetes disponibles en los repositorios que coincidan con un término de búsqueda.                       |
| `show`       | Muestra información detallada sobre un paquete específico.                                                     |
| `list`       | Lista paquetes según criterios, como los instalados, actualizables o que cumplen cierta condición de búsqueda. |
| `install`    | Instala un paquete de software en el sistema.                                                                  |
| `remove`     | Elimina un paquete del sistema, manteniendo los archivos de configuración.                                     |
| `purge`      | Elimina un paquete del sistema junto con sus archivos de configuración.                                        |
| `autoremove` | Elimina paquetes que no tienen dependencias.                                                                   |

[Regresar a índice](#índice)

---
## Manejo de texto
>[!warning]
>**¿Por qué es importante?**
>Configuraciones se guardan en ficheros de texto.
>Los logs también son en formato texto.
>Cualquier programa en modo consola que muestra resultados mediante texto.

>[!info]
>Dominar esto ayuda muchísimo como administrador del servidor.

### Mostrar y filtrar texto

**Mostrar texto**
```shell
# Mostrar texto que se recibe
echo "Hola Mundo"

# Interpretar ordenes:
# Salto de línea
echo -e "Hola \nMundo"
# Tabulación
echo -e "Hola\tMundo"

# Ver las variables del sistema
echo $PATH
echo "Soy el usuario: $USER"

# Las comillas simples no interpretarán variables:
echo 'La variable del sistema que guarda el usuario es: $USER'

# Mostrar un fichero de texto
cat /etc/issue
# Mostrar con número de línea en el archivo
cat -n /etc/fstab
```

**Mostrar contenido de ficheros de texto**
```shell
# Mostrar el texto poco a poco -> 'h' muestra la ayuda
more archivo.ext
# Su mejora para los archivos grandes y con una "ventana nueva"
less archivo.ext
```
Búsqueda dentro del comando 'less':
/palabra + enter -> buscar coincidencias hacia delante.
?palabra + enter -> buscar coincidencias hacia atrás.
g -> Ir al principio del documento.
G -> Ir al final del documento.
:10 + g -> Ir a un número de línea concreto.

**Primeras y últimas líneas**
```shell
# Mostrar las 10 primeras líneas
head file.txt
# Mostrar una cantidad de líneas (5 por ejemplo):
head -n5 file.txt

# Mostrar las 10 últimas líneas, funciones como head
tail file.txt

# Conocer quién ha sido el último usuario registrado
tail -1 /etc/passwd
# Quien fue el primer usuario
head -n1 /etc/passwd

# Ejemplo de como monitorizar un archivo log (paquetes en este caso)
tail -f /var/log/dpkg.log  # CTRL + c para salir de este
```

**Filtrar texto**
`grep` -> Muestra sólo las líneas que cumplen con un patrón.
- `-i` -> No distingue entre mayúscula y minúscula.
- `-w` -> El patrón tiene que ser una palabra exacta.
- `-v` -> Muestra las que NO coinciden con el patrón.
- `-r` -> Busca en los ficheros de forma recursiva.
- `-n` -> Indica el número de línea.
- `-l` -> Indica solamente el fichero donde ha encontrado alguna coincidencia.
- `-c` -> La cantidad de líneas que cumplen con el patrón.
- `-B n`, `-A n` -> Muestra una cantidad de líneas n, antes (before) o después (after) de encontrar el patrón.
- `--color` -> Destaca el patrón en color dentro de la línea seleccionada.
- `^a` -> Empieza la línea por...
- `u$` -> ...termina la línea por...

Patrón -> `grep -opción palabra /path/archivo`

```shell
# Buscar una palabra en TODO los archivos de un directorio sin ir a niveles por debajo
grep root /etc/* 2> /dev/null  # enviar errores a nulo

# Buscar teniendo presente los subdirectorios (recursivo)
grep -r contrib /etc/* 2> /dev/null

# Buscar también la siguiente línea de la coincidencia
grep -w -A 1 1000 /etc/passwd

# Buscar también la anterior línea de la coincidencia
grep -w -B 1 1000 /etc/passwd
```

`cut` -> Corte "vertical", sólo una parte de cada línea.
- `-c` -> Selecciona sólo los caracteres que le indiquemos. Se pueden hacer delimitaciones.
	- `cut -c 5,10 file.txt` // `cut -c 7-25 file.txt`
- `-d` -> Indica un carácter separador entre los distintos campos de una línea. Por defecto buscará el tabulador.
- `-f` -> Elegir los campos que queremos que se muestren.

[Regresar a índice](#índice)

---

## Tuberías y redirecciones
### Redirecciones
``` shell
# Redirigir la salida del comando hacia un texto
echo "Hola mundo" > texto.txt
# Comprobar
cat texto.txt

# Si el archivo ya existe, añadir la salida al final de este.
echo "Hello world" >> texto.txt
```

Redirigir mensajes de *error* -> `2>`
Redirigir *todos* los mensajes -> `&>`
Desechar los resultados aquí -> `/dev/null`

### Tuberías
Redirigir el resultado hacia otro comando. Se pueden ir concatenando.
Se le llama "tubería" (pipe) ya que establece un "canal" por el que pasará el texto de un comando a otro.
``` shell
# Ejemplos
echo "HOLA MUNDO" | wc

grep -w ana /etc/passwd | cut -d":" -f1 | sort

ls -lr /etc/ | less

ls -lR /etc/ 2> /dev/null | grep shadow

ps -e | grep as
```

[Regresar a índice](#índice)

---
## Acciones sobre texto
### Ordenar con SORT
- Ordena de forma alfabética por defecto.

| Opciones | Tipo orden                         | Ejemplo                                                                                            |
| :------: | ---------------------------------- | -------------------------------------------------------------------------------------------------- |
|   `-r`   | Inversa                            | `sort -r archivo.txt`                                                                              |
|   `-t`   | Carácter de separación de columnas | `sort -t "," 2 archivo.txt`<br>Divide en el número de columnas dependiendo de cuantas "," existen. |
|   `-k`   | Columnas específicas               | `sort -t "," -k 2 archivo.txt`<br>Escoger la segunda columna de la división.                       |
|   `-n`   | Numéricamente                      | `sort -t "," -k 2 -n archivo.txt`                                                                  |
|          |                                    |                                                                                                    |
|   `-u`   | Eliminar líneas duplicadas         | `sort -u archivo.txt`                                                                              |

### Comando UNIQ
- Elimina las líneas repetidas contiguas en un archivo o entrada estàndar.

| Opciones | Tipo orden                         | Ejemplo                                           |
| :------: | ---------------------------------- | ------------------------------------------------- |
|   `-i`   | Ignora mayúsculas y minúsculas     | `cut -d"," -f1 nombres.txt \| sort \| uniq -i`    |
|   `-c`   | Cuenta la cantidad de repeticiones | `cut -d"," -f1 nombres.txt \| sort \| uniq -i -c` |
|   `-u`   | Muestra las no duplicadas          | `cut -d"," -f1 nombres.txt \| sort \| uniq -i -u` |

### Comando WC - Contar
- Contar líneas, palabras y bytes en archivos de texto o entradas estándar:
	- {Líneas} {Palabras} {Bytes}
		- Hay quien dice que el apartado de bytes se refiere al número de caracteres.
		- No es cierto porque si hacemos la comprobación con la "ñ", veremos que aumentan los bytes. -> consultar la pág. man de wc

| Opciones | Tipo orden |
| :------: | ---------- |
|   `-l`   | Líneas     |
|   `-w`   | Palabras   |
|   `-c`   | Bytes      |
|   `-m`   | Caracteres |

Ejemplos:
```shell
# Entrada estándar
echo "Hola" | wc

# Fichero
wc /etc/passwd
```

### Comando REV
Invertir el order de los caracteres en cada línea de un archivo de texto o entrada estándar.
- Útil cuando una línea se puede dividir en columnas y quiero convertir la última en la primera.

Ejemplos:
```shell
# Listado de todos los directorios
ls -lR ficheros | grep '/' | grep -v ^l | rev | cut -d"/" -f1 | rev
```

[Regresar a índice](#índice)

---

## Otros comandos para ficheros

### File -> Tipo de archivo
```shell
# Mostrar solo la descripción del tipo de archivo, su codificación.
file -b <archivo>

# Muestra el MIME del archivo.
file -i <archivo>

# Lee los nombres de los archivos desde un fichero
file -f <archivo_con_nombres>

# Ver los archivos y los tipos de cada uno en el directorio
file *
```

### LN -> Crear enlaces
Dos tipos:
- **Duro** -> Un puntero a la información del disco duro.
	- Aunque veamos que su peso es idéntico al archivo original, no es así.
		- Es un puntero a un archivo original.
		- Si borramos los enlaces, el peso seguirá siendo el mismo.
	- Comando `ls -lh`, la columna que hay después de los permisos, nos muestra el número de enlaces duros que tiene el archivo.
- **Blando** -> Se sustituye por la ruta del fichero original. Parecido a tener un "alias". Como un acceso directo.
	- Cuidado con poner rutas relativas sobretodo al mover archivos.

```shell
# Enlace duro:
ln <archivo> <ruta_y_nombre_archivo>

# Comprobar detalles del archivo
stat <archivo>

# Crear un enlace blando
ln -s <ruta_absoluta_archivo_original> <ruta_acceso_directo_archivo>
```

### DU -> Disk Usage
Espacio de disco ocupado por un fichero o por un directorio. Si le pasamos un directorio mostrará recursivamente el tamaño de todos los subdirectorios.
```shell
# Mostrar el tamaño del directorio y sus subdirectorios
du <ruta_directorio>

# Mostrar el tamaño en lectura humana
du -h

# Mostrar la cantidad total de lo que ocupa el directorio
du -s

# Mostrar la cantidad de todo lo que hay dentro del directorio
du -sh *
# Se puede concatenar con sort
du -sh * | sort -hr
```

### DF -> Disk Free
Información sobre las particiones del sistema: tamaño total, usado y libre.
```shell
# Ver el espacio disponible en el disco y sus particiones
df -h

# Verlo por tipo
df -hT

# Ver el tamaño de la partición donde está ubicado un archivo
df -h <ruta_archivo>
```
**Alternativa a DF**
```shell
sudo apt install dfc
# Ejecución
dfc
```

[Regresar a índice](#índice)

---

## Búsqueda en el sistema de ficheros
### LOCATE - Una primera búsqueda
```shell
# Instalar el paquete
sudo apt install locate

# Actualizar el sistema de ficheros con una base de datos para utilizar locate
su -
# Si se crea un archivo, un directorio, etc., actualizar la BD
updatedb

# Sintaxis locate
locate [opciones] [patrón]
```

| Opciones | Descripción                                    |
| :------: | ---------------------------------------------- |
|   `-i`   | Ignorar mayúsculas y minúsculas en la búsqueda |
|   `-c`   | Contar el número de coincidencias encontradas  |
|   `-n`   | Limitar el número de resultados.               |
|   `-r`   | Usar expresiones regulares para la búsqueda.   |

Ejemplos:
```shell
# Ignorando may y minus
locate -i "sources.list"

# Utilizando regex
locate -r "/sources.list$"
```

### Comando FIND
>[!danger]
>Comando super importante de dominar como administrador.

- Busca de forma recursiva en un directorio todos los ficheros que cumplan ciertas condiciones.
Sintaxis:
```shell
find [ruta] [opciones] [acción]
```

|       Opciones        | Descripción                                                                                                                                                                                                                                   |
| :-------------------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|        `-name`        | Especificar patrones para los nombres de los ficheros a buscar.                                                                                                                                                                               |
|       `-iname`        | Igual que `name` pero sin diferencias mayúsculas y minúsculas.                                                                                                                                                                                |
|        `-type`        | Tipo de fichero a buscar:<br>`d`-> directorios<br>`f` -> ficheros<br>`l` -> enlaces simbólicos<br>`b` -> dispositivos de bloque<br>`c` -> dispositivos de carácter<br>`p` -> para tuberías<br>`s` -> para sockets.                            |
|    `-size +/- <n>`    | `c` -> bytes<br>`k` -> kilobytes<br>`M` -> megabytes<br>`G` -> gigabytes                                                                                                                                                                      |
|     `-perm <num>`     | Ficheros cuyos permisos sean los expresados exactamente por número.<br>`-720` -> El signo `-` actuará como un AND: quiero exactamente esos permisos.<br>`/111` -> El signo `/` actuará como un OR: uno de los tres números debe de coincidir. |
|   `-user <usuario>`   | Especifica el usuario propietario del fichero                                                                                                                                                                                                 |
|  `-group <usuario>`   | Especifica el grupo propietario del fichero                                                                                                                                                                                                   |
|    `-mmin [+/-]n`     | Los datos del fichero fueron modificados por última vez hace `n` minutos.                                                                                                                                                                     |
|    `-mtime [+/-]n`    | Los datos del fichero fueron modificados por última vez hace `n` días.                                                                                                                                                                        |
| `-maxdepth <niveles>` | Desciende el número entero de niveles positivos en la jerarquía del directorio.                                                                                                                                                               |
|        `-path`        | Buscar por toda la ruta de los elementos. Se pueden utilizar comodines.                                                                                                                                                                       |
|       `-regex`        | Igual que `-path`, pero usando expresiones regulares.                                                                                                                                                                                         |

| Modificadores | Descripción                                                                        |
| :-----------: | ---------------------------------------------------------------------------------- |
|    `-not`     | Niega la condición que pongamos a continuación                                     |
               | Fin de línea                                                                            |
| `.`               | Cualquier carácter.<br>Buscar "punto", delante: \                                       |
| `[aeiou]`         | Pueden aparecer un conjunto determinado de caracteres en una posición. -> `c[aei]s[ao]` |

### Exclusión y Rangos

| Corchetes                  | Descripción                                                                                                           |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| Exclusión / Negación `[^]` | Una posición puede encontrarse cualquier carácter **excepto** los que se encuentran entre corchetes = `c[^aei]s[^ao]` |
| Rangos `[-]`               | Indicar todos los valores intermedios entre un inicio y un final.<br> `c[a-d]s[0-5]` == `c[abcd]s[012345]`            |
| Mezcla                     | `[3-8[ :upper: ]mty]` == Admiten números del 3 al 8 o cualquier mayúscula o las minúsculas de m, t, y.                |

| Clases         | Descripción                                              |
| -------------- | -------------------------------------------------------- |
| `[ :alnum: ]`  | Letras y dígitos                                         |
| `[ :alpha: ]`  | Letras                                                   |
| `[ :blank: ]`  | Espacios en Blanco                                       |
| `[ :cntrl: ]`  | Caracteres de control                                    |
| `[ :space: ]`  | Los Espacios en Blanco verticales y horizontales         |
| `[ :graph: ]`  | Caracteres imprimibles, sin incluir el Espacio en Blanco |
| `[ :print: ]`  | Caracteres imprimibles, incluyendo el Espacio en Blanco  |
| `[ :digit: ]`  | Dígitos                                                  |
| `[ :lower: ]`  | Letras minúsculas                                        |
| `[ :upper: ]`  | Letras mayúsculas                                        |
| `[ :punct: ]`  | Signos de puntuación                                     |
| `[ :xdigit: ]` | Dígitos Hexadecimales                                    |

Ejemplos:
```bash
# Empieza por "a":
grep ^a /usr/share/dict/american-english| tail

# Termina por "o"
grep o$ /usr/share/dict/american-english| tail

# Palabras que coincidan
grep 'c[aei]s[ao]' /usr/share/dict/british-english

# Encontrar la palabra exacta
grep '^c[aei]s[aeo] /usr/share/dict/british-english

# Caracteres genéricos con .
grep '^c[aei]s..[aeiourt] /usr/share/dict/british-english

# Contar saltos de linea en todos los archivos
grep ^$ * | wc -l
```

### Repeticiones

| Repeticiones | Descripción                                                              |
| ------------ | ------------------------------------------------------------------------ |
| `X*`         | Concuerda con cero o más repeticiones de la regex que le precede (izq)   |
| `X?`         | Concuerda con cero o una parición de la regex que le precede             |
| `X+`         | Concuerda con una o más repeticiones de la regex que le precede          |
| `X{n}`       | Concuerda con n repeticiones exactas de X                                |
| `X{n,}`      | Concuerda con n o más repeticiones de X                                  |
| `X{,n}`      | Concuerda con cero o a lo sumo n repeticiones de X                       |
| `X{n, m}`    | Concuerda con al menos n repeticiones de X, o como mucho m repeticiones. |

Ejemplos
```bash
# palabra de 8 caracteres
grep -E '^[h-q]{8} /usr/share/dict/british-english

# palabra entre 6 y 9 caracteres
grep -E '^[h-q]{6,9} /usr/share/dict/british-english

# Mínimo 6 caracteres hasta sin límite
grep -E '^[h-q]{6,} /usr/share/dict/british-english

# Máximo hasta 9 caracteres, sin mínimo
grep -E '^[h-q]{,9} /usr/share/dict/british-english
```

[Regresar a índice](#índice)