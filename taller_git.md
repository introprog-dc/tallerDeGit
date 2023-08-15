# git

git es una herramienta para la gestion de versiones de archivos. Esto quiere decir que nos facilita el versionado de nuestros archivos, permitiéndonos entre otras cosas:

 1. entender cual es la última versión de un archivo
 2. entender cual fue la cadena de modificaciones que le fuimos haciendo a lo largo del tiempo
 3. trabajar en equipo con otros sobre los mismos archivos de manera ordenada

Antes de ponernos a usar git vamos a ver primero otro comando: `diff`.

## diff

Supongamos que estoy trabajando en una presentacion para dar una clase. Y que estoy trabajando en un archivo que se llama `presentacion.txt` donde voy anotando el contenido de la presentacion.

Para que el ejemplo se entienda más fácilmente, lo vamos a ir haciendo juntos.

Desde una sesión de Ubuntu, abrir la aplicacion "Editor de Textos" y crear un archivo de texto llamado `presentacion.txt` en el Escritorio. Agregar el siguiente contenido y guardarlo:

```
Esta es mi presentacion. La presentacion tiene estos temas:
* tema 1
* tema 2
```

Supongamos ahora que sigo trabajando en mi presentación, pero como voy a hacer cambios, primero hago un backup de mi archivo.

Abrir una terminal (CTRL + ALT + T). Nos tiene que aparecer un prompt, que nos dice básicamente el nombre del usuario que estamos usando y la carpeta donde estamos posicionados:

```
clinux01@pc1:~$
```

En este caso el usuario se llama `clinux01`, es un usuario de la máquina que se llama `pc1` y la carpeta donde estamos posicionados se llama `~` que es una forma abreviada de decir que estamos en la carpeta "home" del usuario (es decir, `/home/clinux01`).

Podemos cambiar de carpeta usando el comando `cd` (change directory). Como queremos que nuestro archivo quede en el escritorio vamos a hacer `cd Escritorio`:

```
clinux01@pc1:~$ cd Escritorio/
clinux01@pc1:~/Escritorio$

```

Notar que el prompt cambio de `~` a `~/Escritorio`. Esto quiere decir que ahora estamos "parados" en la carpeta del Escritorio.

Confirmamos que nuestro archivo existe en el escitorio:
```
clinux01@pc1:~/Escritorio$ ls -l
total 60
-rw-rw-r-- 1 clinux01 clinux01   78 may  1 13:14  presentacion.txt
```

Hacemos una copia de nuestra presentación usando el comando `cp` (copy):

```
cp presentacion.txt presentacion_v1.txt
```

Verificamos que tenemos los dos archivos y que ambos tienen el mismo tamano (78 bytes):

```
clinux01@pc1:~/Escritorio$ ls -l
total 60
-rw-rw-r-- 1 clinux01 clinux01   78 may  1 13:14  presentacion.txt
-rw-rw-r-- 1 clinux01 clinux01   78 may  1 13:14  presentacion_v1.txt
```

Ahora que tenemos backupeado nuestro archivo lo vamos a modificar. Desde el editor de texto que teníamos abierto, agregamos un nuevo tema:

```
Esta es mi presentacion. La presentacion tiene estos temas:
* tema 1
* tema 2
* tema 3
```

Guardamos el archivo.

Si listamos los archivos del Escritorio vamos a poder comprobar que efectivamente los tamaños de los archivos ahora son distintos (87 bytes vs 78 bytes):

```
ls -l
clinux01@pc1:~/Escritorio$ ls -l
total 64
-rw-rw-r-- 1 clinux01 clinux01   87 may  1 13:15  presentacion.txt
-rw-rw-r-- 1 clinux01 clinux01   78 may  1 13:15  presentacion_v1.txt
```

En este momento podemos utilizar el comando `diff` para ver las diferencias entre las 2 versiones de nuestra presentación:

```
clinux01@pc1:~/Escritorio$ diff presentacion.txt  presentacion_v1.txt
4d3
< * tema 3
```

La salida de `diff` nos confirma que efectivamente los archivos son idénticos salvo por 1 línea que es diferente.
Si miramos la salida de `diff` vamos a ver el signo '<' que indica que esa línea está en el archivo "de la izquierda", o el primer parámetro que le pasamos a `diff` (`presentacion.txt` en nuestro caso).

Veamos qué pasa si ahora modificamos el otro archivo y le agregamos un tema 4. El archivo `presentacion_v1.txt` nos debería quedar asi:

```
Esta es mi presentacion. La presentacion tiene estos temas:
* tema 1
* tema 2
* tema 4
```

Grabamos el archivo presentacion_v1.txt y volvemos a hacer diff:

```
clinux01@pc1:~/Escritorio$ diff presentacion.txt presentacion_v1.txt
4c4
< * tema 3
---
> * tema 4
```

Ahora es mas claro que `diff` nos esta avisando que la línea que menciona al tema 3 sólo está en el primer archivo, y la que menciona al tema 4 está sólo en el segundo archivo.

NOTA: `diff` tambien nos está diciendo `4c4`, que significa que la línea 4 del primer archivo cambió (c=changed) respecto de la línea 4 del segundo archivo. En el caso anterior nos decía 4d3, lo que significa que la línea 4 del archivo de la izquierda fue eliminada (d=deleted) en el segundo archivo.

NOTA: la mayoría de los comandos de linux tienen una ayuda disponible en la forma de un "manual de instrucciones". Para ver el de `diff` ejecutar desde una terminal `man diff`

Ahora que podemos ver los cambios entre las diferentes versiones del archivo podemos trabajar con un poco más de tranquilidad ya que podemos entender más facilmente cuáles son las diferencias entre ellos.

Por ejemplo, podría darse el caso de que quieren trabajar en en la presentación en una computadora de la facultad durante el día y en mi casa por la noche. Entonces cada vez que copio los archivos podria hacer un diff para verificar que tengo todos los cambios y no estoy pisando por error cambios que me interesan.


## usando git

git nos va a permitir ir entendiendo las diferencias entre nuestros archivos de forma más sencilla.

NOTA: como las máquinas de los laboratorios son compartidas vamos a ejecutar una limpieza de archivos y comandos que puedan haber quedado guardados de antes:

```
clinux01@pc1:~/Escritorio$ rm -rf ~/Escritorio/.git
clinux01@pc1:~/Escritorio$ git config --global --unset-all user.email
clinux01@pc1:~/Escritorio$ git config --global --unset-all user.name
```

Ahoa sí, vamos a inicializar un "repositorio" que es el nombre que le da git a una colección de archivos:

```
clinux01@pc1:~/Escritorio$ git init
Inicializado repositorio Git vacío en /home/clinux01/Escritorio/.git/
```

Si listamos el contenido de la carpeta vamos a seguir viendo nuestros archivos:

```
clinux01@pc01:~/Escritorio$ ls -l
total 64
-rw-rw-r-- 1 clinux01 clinux01   87 may  1 13:15  presentacion.txt
-rw-rw-r-- 1 clinux01 clinux01   87 may  1 13:22  presentacion_v1.txt
```

Pero si agregamos la opcion `-a` vamos a poder ver una carpeta oculta llamada `.git`:

```
clinux01@pc1:~/Escritorio$ ls -l -a
total 76
drwxr-xr-x  6 clinux01 clinux01 4096 may  1 13:38  .
drwxr-xr-x 97 clinux01 clinux01 4096 may  1 13:22  ..
drwxrwxr-x  7 clinux01 clinux01 4096 may  1 13:38  .git
-rw-rw-r--  1 clinux01 clinux01   87 may  1 13:15  presentacion.txt
-rw-rw-r--  1 clinux01 clinux01   87 may  1 13:22  presentacion_v1.txt
```

NOTA: también aparecen 2 carpetas más: '.', que representa la carpeta actual y '..' que representa a la carpeta "padre" de la actual. Es por esto que si hacemos `cd ..` cambiamos de carpeta a la que esta en el nivel superior. Y de la misma forma si hacemos `cd .` nos quedamos posicionados en la misma carpeta donde estamos.

Podemos ver que en la carpeta `.git` hay varios archivos y carpetas:

```
clinux01@pc1:~/Escritorio$ ls -l .git
total 32
drwxrwxr-x 2 clinux01 clinux01 4096 may  1 13:38 branches
-rw-rw-r-- 1 clinux01 clinux01   92 may  1 13:38 config
-rw-rw-r-- 1 clinux01 clinux01   73 may  1 13:38 description
-rw-rw-r-- 1 clinux01 clinux01   23 may  1 13:38 HEAD
drwxrwxr-x 2 clinux01 clinux01 4096 may  1 13:38 hooks
drwxrwxr-x 2 clinux01 clinux01 4096 may  1 13:38 info
drwxrwxr-x 4 clinux01 clinux01 4096 may  1 13:38 objects
drwxrwxr-x 4 clinux01 clinux01 4096 may  1 13:38 refs

```

Estos archivos y carpetas son internos a git y son los que va a usar para llevar la cuenta de las modificaciones que vamos realizando, por lo que no los vamos a modificar.

Ahora podemos preguntarle a git cuál es el estado de los archivos a través del comando `git status`:

```
clinux01@pc1:~/Escritorio$ git status
En la rama master

No hay commits todavía

Archivos sin seguimiento:
  (usa "git add <archivo>..." para incluirlo a lo que se será confirmado)
	presentacion.txt
	presentacion_v1.txt

no hay nada agregado al commit pero hay archivos sin seguimiento presentes (usa "git add" para hacerles seguimiento)
```

Lo que nos está diciendo git es que:
 1. Existen 2 archivos de texto (nuestras presentaciones) pero que no están bajo el control de git (es decir, que git los desconoce)
 2. Que podemos usar el comando `git add` para empezar a trackearlos

Como nos interesa seguir trabajando en nuestra presentación vamos a pedirle a git que lo empiece a "seguir". Para esto usamos `git add`:

```
clinux01@pc1:~/Escritorio$ git add presentacion.txt
clinux01@pc1:~/Escritorio$
```

Vemos que el comando no genera ningun mensaje por pantalla. Así que la forma de confirmar que hubo algun cambio es volver a pedirle el status:

```
clinux01@pc1:~/Escritorio$ git status
En la rama master

No hay commits todavía

Cambios a ser confirmados:
  (usa "git rm --cached <archivo>..." para sacar del área de stage)
	nuevos archivos: presentacion.txt

Archivos sin seguimiento:
  (usa "git add <archivo>..." para incluirlo a lo que se será confirmado)
	presentacion_v1.txt
```

Esto nos confirma que `presentacion.txt` esta ahora bajo el radar de git, pero sin embargo necesitamos confirmarle a git que realmente queremos que lo siga. Esta confirmación (commitment) lo hacemos a través del comando `git commit`:

```
clinux01@pc1:~/Escritorio$ git commit -m "version inicial de la presentacion"
[master (root-commit) f1b6010] version inicial de la presentacion
 1 file changed, 4 insertions(+)
 create mode 100644 presentacion.txt
```

NOTA: si es nuestro primer commit git nos va a pedir que nos identifiquemos:

```
clinux01@pc1:~/Escritorio$ git commit -m "version inicial de la presentacion"

*** Por favor cuéntame quién eres.

Ejecuta

  git config --global user.email "you@example.com"
  git config --global user.name "Tu Nombre"

para configurar la identidad por defecto de tu cuenta.
Omite --global para configurar tu identidad solo en este repositorio.

fatal: no se puede tener un nombre de identidad vacío (para <clinux01@pc1.lab1103>)
```

Como las máquinas de los laboratorios son compartidas vamos a realizar la configuracion de manera local (omitiendo el parametro `--global`):

```
clinux01@pc1:~/Escritorio$ git config user.email [direccion de mail]
clinux01@pc1:~/Escritorio$ git config user.name [Nombre y Apellido entre comillas]
```

NOTA: podemos ver la configuracion de git ejecutando `git config --list`.


NOTA: git nos pide que cada vez que confirmemos un cambio (commit) pongamos un mensaje breve que nos permita entender (a nosotros mismos o a otros) de qué se trata el cambio que estamos haciendo. Este mensaje es el que le pasamos cuando pusimos la opcion `-m mensaje` como parametro de `git commit`:

```
clinux01@pc1:~/Escritorio$ git commit -m "version inicial de la presentacion"
[master (root-commit) f1b6010] version inicial de la presentacion
 1 file changed, 4 insertions(+)
 create mode 100644 presentacion.txt
```

Si volvemos a pedir el status vamos a ver que ahora git ya no nos dice nada sobre `presentacion.txt` (aunque nos sigue diciendo que existe `presentacion_v1.txt`, pero que no le esta haciendo seguimiento):

```
clinux01@pc1:~/Escritorio$ git status
En la rama master
Archivos sin seguimiento:
  (usa "git add <archivo>..." para incluirlo a lo que se será confirmado)
	presentacion_v1.txt

no hay nada agregado al commit pero hay archivos sin seguimiento presentes (usa "git add" para hacerles seguimiento)
```

Ahora que hemos confirmado el cambio sobre `presentacion.txt`, esto significa que podemos preguntarle a git específicamente por su historial de cambios:

```
clinux01@pc1:~/Escritorio$ git log presentacion.txt
commit f1b601081919422cd86112a137bad52c9c0dce61 (HEAD -> master)
Author: Usuario generico <usuario@dc.uba.ar>
Date:   Mon May 1 13:52:14 2023 -0300

    version inicial de la presentacion
```

NOTA: podemos ver que nuestro commit tiene un valor hexadecimal asociado que es el resultado de aplicar una función de hash sobre nuestros cambios. Cada vez que hacemos un commit, git calcula un nuevo hash y lo asocia al commit. Esto le permite identificar de manera unívoca a cada uno de los commits que vamos haciendo.

Ahora podemos seguir trabajando en nuestra presentación y seguirle sumando cambios. Por ejemplo, podemos agregar un tema 5 y eliminar el tema 1:

```
Esta es mi presentacion. La presentacion tiene estos temas:
* tema 2
* tema 3
* tema 5
```

Dado que `presentacion.txt`` está bajo la gestión de git, entonces podemos preguntarle cuáles son las diferencias con la versión que git conoce. Para esto usamos el comando `git diff`:

```
clinux01@pc1:~/Escritorio$ git diff presentacion.txt
diff --git a/presentacion.txt b/presentacion.txt
index 7f110c6..c13e96b 100644
--- a/presentacion.txt
+++ b/presentacion.txt
@@ -1,4 +1,4 @@
 Esta es mi presentacion. La presentacion tiene estos temas:
-* tema 1
 * tema 2
 * tema 3
+* tema 5
```

La salida es parecida a la que vimos cuando ejecutabamos `diff` a mano, pero es un poco más detallada:

 * nos dice que hay una línea que fue eliminada (-)
 * nos dice que hay una línea que fue agregada (+)
 * nos dice que los cambios estan entre las lineas 1 y 4 del archivo

Es decir, que además de darnos las diferencias, nos muestra un poco más de contexto para que nos resulte más fácil entenderlas.

Si estamos satisfechos con los cambios que hicimos, entonces le decimos a git que queremos agregar los cambios del archivo (`git add`) y se lo confirmamos (`git commit`):

```
clinux01@pc1:~/Escritorio$ git add presentacion.txt
clinux01@pc1:~/Escritorio$ git commit -m "agregue tema. saque tema"
[master 2cc3337] agregue tema. saque tema
 1 file changed, 1 insertion(+), 1 deletion(-)
```

Nuevamente, en la última línea del mensaje, git nos está resumiendo los cambios que detectó desde la última vez que le mandamos el archivo (1 inserción, 1 borrado).

Si pedimos la historia del archivo vamos a ver los 2 cambios/versiones, ordenados de más reciente a menos reciente:

```
clinux01@pc1:~/Escritorio$ git log
commit 2cc3337d5efbe503a3f69af0084eab5b51475262 (HEAD -> master)
Author: Usuario generico <usuario@dc.uba.ar>
Date:   Mon May 1 14:04:22 2023 -0300

    agregue tema. saque tema

commit f1b601081919422cd86112a137bad52c9c0dce61
Author: Usuario generico <usuario@dc.uba.ar>
Date:   Mon May 1 13:52:14 2023 -0300

    version inicial de la presentacion
```

De está forma evitamos tener que ir recordando los cambios que vamos haciendo, ya que git lo hace por nosotros. También nos permite volver a versiones anteriores y muchas cosas más. Pero antes de verlas, vamos a ver un poco más en detalle por qué el proceso de cambios se hace en 2 etapas.

## stages

Podemos pensar que en principio hay 2 mundos: uno donde nosotros somos dueños de los archivos y sobre los que git no sabe nada ni tiene potestad, y otro donde git es el responsable de gestionar los cambios de los archivos que conoce.

Entonces lo que queremos hacer es ir avisándole sobre los archivos que queremos incluir bajo su control de versiones. Podemos pensar que la forma en que vamos pasando archivos de un mundo al otro es a traves de `git add`.

La gran ventaja que tiene git es que no se olvida de nada de lo que va pasando. Es decir, cada modificación de un archivo la registra. Esto puede ser un problema, por ejemplo, si queremos que se "olvide" de algunos archivos que ya no nos sirven más. El proceso para eliminar un archivo del control de versiones es:

 1. git rm [archivo]
 2. git commit [archivo] -m [mesaje]

El inconveniente es que si bien git nos va a confirmar que ya no sabe nada de nuestro archivo borrado la realidad es que sigue guardando toda la historia hasta ese momento. Es decir, hay archivos dentro de la carpeta `.git` que siguen guardando la información de cambios histórica del archivo.
Esto puede ser un problema si somos ansiosos y agregamos un montón de archivos y luego nos arrepentimos, ya que igual quedarian en la historia de git.

Para evitar ese problema (y otros) git usa un tercer "mundo" que es un intermedio entre el mundo de git y el de nuestros archivos no-versionados. Esta area de pruebas o `stage` es donde se van agregando los cambios hasta que se confirman (commit). El comando commit es el que termina pasando los cambios al mundo de git.

![](https://github.com/introprog-dc/tallerDeGit/blob/main/images/stage.png?raw=true)


Es por esta razón que cada vez que hacemos un add, tenemos que hacer un commit si queremos que nuestro cambio sea tomado efectivamente por git.

NOTA: podría darse el caso de que hayamos agregado algun archivo por error. Esto es muy común si hicimos `git add .` o `git add presentacion*` porque en esos casos git va a interepretar que queremos incorporar un grupo de archivos. Entonces podría ser que se haya agregado algun archivo por error. En estos casos, la salida de `git status` nos dice como solucionarlo:

```
clinux01@pc1:~/Escritorio$ ls -l presen*
-rw-rw-r-- 1 clinux01 clinux01 96 may  7 13:40 presentacion.txt
-rw-rw-r-- 1 clinux01 clinux01 87 may  1 13:22 presentacion_v1.txt
clinux01@pc1:~/Escritorio$ git add presen*
clinux01@pc1:~/Escritorio$ git status .
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)   <---- ACA
	modified:   presentacion.txt
	new file:   presentacion_v1.txt
```

Entonces si queremos sacar `presentacion_v1.txt`, lo hacemos con `git restore --staged`:


```
clinux01@pc1:~/Escritorio$ git restore --staged presentacion_v1.txt
clinux01@pc1:~/Escritorio$ git status .
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   presentacion.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	presentacion_v1.txt
```

Nuevamente, esto es posible porque existe el stage intermedio que permite "arrepentirse" antes de confirmar con `git commit`.

## trabajo en grupo

Hasta ahora todos los ejemplos que hicimos fueron considerando el trabajo individual de 1 sola persona. Sin embargo, esto no es un escenario realista en el contexto de un proyecto de desarrollo de software, donde hay grupos de programadores trabajando sobre el mismo código.
Sin ir más lejos, el trabajo práctico de la materia se realiza de manera grupal.

Cuando hay varias personas trabajando sobre el mismo grupo de archivos es necesario que haya algun tipo de acuerdo en la forma en que se va a trabajar, para lograr que los cambios se hagan de manera ordenada.

Retomando el ejemplo de la presentación, podría pedirle a mis compañeros de equipo que me ayuden a completar los slides para avanzar más rápido. Para esto vamos a necesitar que todos accedan al mismo repositorio (hasta ahora mi repositorio era local, por lo que solo podía ser usado por mí en la computadora donde lo cree usando `git init`).

### repositorio remoto

Para poder compartir el repositorio entre varias personas lo que vamos a hacer es armar un repositorio de git remoto, es decir, que va a estar corriendo como un servicio en alguna máquina de la red que sea accesible por todos los integrantes del proyecto. Una posibilidad seria instalar y configurar ese servicio en una máquina del laboratorio, de esa forma seria accessible desde cualquier otra máquina en la red del laboratorio.

Generalmente lo que se suele hacer es hacer que ese servicio este disponible a través de un servicio web, de la misma forma que una pagina web. Por ejemplo, los siguientes son servicios que ofrecen git y pueden ser utilizados por cualquiera que tenga acceso a Internet:

 * http://www.github.com
 * http://www.gitlab.com

Por ejemplo, si la decision es usar github entonces todos los integrantes del proyecto deberían crearse una cuenta en ese servicio y a partir de ese momento podrian utilizar los mismos comandos de git que estuvimos viendo (más algunos comandos adicionales que vamos a ver en breve).

La idea principal del repositorio remoto es que va funcionar como la "verdad última" (`ultimate source of truth`) respecto de cuál es la última versión de cada uno de los archivos que este bajo la administración de git.

### flujo de trabajo

Para hacer esta parte vamos a usar este repositorio: https://github.com/introprog-dc/tallerDeGit

La idea es usar este proyecto es entender mejor cómo funciona un flujo de trabajo de múltiples personas colaborando en un mismo repositorio.

Lo primero es que cada uno de los integrantes del proyecto obtenga una copia del repositorio. Esto se puede hacer de varias maneras, pero nosotros lo vamos a hacer a traves de un `fork` para evitar pisarnos con otros alumnos haciendo las mismas pruebas.

Entonces lo primero que vamos a hacer es clickear en el botón `fork`:

![](https://github.com/introprog-dc/tallerDeGit/blob/main/images/git1.png?raw=true)

Para poder realizar esta acción github nos va a pedir que nos logueemos con una cuenta de usuario válida (o que creemos una nueva).

Una vez logueados, confirmamos el nombre que va a tener nuestro fork, y finalmente clickeamos en `create fork`

![](https://github.com/introprog-dc/tallerDeGit/blob/main/images/git2.png?raw=true)

Ahora vemos una copia (fork) del repositorio original. Esta copia es el repositorio que vamos a usar como proyecto.

Entonces cada uno de los integrantes del grupo debe clonar usando `git clone`. Para esto podemos clickear en el botón que dice `code` y copiar la URL que figura ahí. Como siempre, abrimos una terminal en nuestra computadora y hacemos `git clone`.

NOTA: antes de poder clonar, vamos a generar un token: https://github.com/settings/tokens. Esto lo vamos a hacer para simplificar nuestros ejemplos. En la vida real sería más prolijo configurar una clave SSH para interactuar con el repositorio remoto.

![](https://github.com/introprog-dc/tallerDeGit/blob/main/images/git4.png?raw=true)

![](https://github.com/introprog-dc/tallerDeGit/blob/main/images/git5.png?raw=true)

NOTA: es MUY importante anotar el token en algun lugar seguro (idealmente un password manager) ya que github nos lo va a mostrar 1 sola vez. Si nos olvidamos el token vamos a tener que generar uno nuevo.

NOTA: también es muy importante que al finalizar nuestro trabajo en las máquinas de los laboratorios borremos el repositorio local, ya que el token queda guardado en los archivos internos de git, por lo que otra persona podria hacer cambios en nuestro fork (el repositorio original desde donde forkeamos se mantendria inalterado).

![](https://github.com/introprog-dc/tallerDeGit/blob/main/images/git6.png?raw=true)

Ahora sí, usando nuestro nombre de usuario y nuestro token, vamos a clonar el repositorio:

```
clinux01@pc1:$ git clone https://miNombreDeUsuario:miToken@github.com/miNombreDeUsuario/tallerDeGit
Cloning into 'tallerDeGit'...
remote: Enumerating objects: 9, done.
remote: Counting objects: 100% (9/9), done.
remote: Compressing objects: 100% (7/7), done.
remote: Total 9 (delta 1), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (9/9), 9.82 KiB | 2.45 MiB/s, done.
Resolving deltas: 100% (1/1), done.

```

Si hacemos `ls -l` vemos que se creó un nuevo directorio llamado `tallerDeGit`:

```
clinux01@pc1:~/Escritorio$ ls -l
drwxrwxr-x 3 clinux01 clinux01 4096 may  3 20:41 tallerDeGit/
```

Para poder trabajar más cómodos vamos a posicionarnos dentro de la carpeta, haciendo `cd`:

```
clinux01@pc1:~/Escritorio$ cd tallerDeGit
clinux01@pc1:~/Escritorio/tallerDeGit$
```

En este momento podemos usar los comandos que vimos anteriormente. Por ejemplo, podemos ejecutar `git log` para ver los cambios que se hicieron hasta el momento en el repositorio original.

NOTA: la salida de `git log` puede ser larga, por lo que tiene incoporado un mecanismo que va mostrando las siguientes lineas a medida que vamos presionando ENTER. Para volver a tomar control de la terminal hay que apretar la tecla `q`.

En este momento vamos a modificar uno de los archivos. Podemos agregar, borrar o cambiar el contenido existente. Por ejemplo, podemos modificar el archivo `README.md`.

Como siempre, vamos a llevar el archivo al área de stage con `git add` y vamos a confirmar el cambio con `git commit -m [mensaje]`.
La diferencia principal es que los cambios que tenemos commiteados solo están por ahora en nuestra máquina. Para que el repositorio remoto se entere de que hicimos cambios se lo tenemos que notificar usando el comando `git push`:

```
clinux01@pc1:~/Escritorio/tallerDeGit$ git push
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 314 bytes | 314.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/miNombreDeUsuario/tallerDeGit
   c1dd4ca..721f61f  main -> main
```

Ahora sí, si vamos a la interfaz web del proyecto, vveremos que se registró el cambio que acabamos de confirmar.

El resto de las personas que trabajan con nosotros en el proyecto van a estar realizando cambios tambien en sus copias locales, y pusheandolas eventualmente al repositorio remoto. Para obtener esos cambios realizados por otros (si los hubiera) es que vamos a realizar `git pull`:

```
clinux01pc1:~/Escritorio/tallerDeGit$ git pull
Already up to date.
```

Básicamente lo que hace `git pull` es hacer un diff de nuestro repositorio local contra el remoto, y aplicar las diferencias (patches) en el orden correcto para que ambos queden sincronizados.
La mayor parte del tiempo es probable que los cambios realizados por distintos colaboradores no se interfieran entre sí. En esos casos alcanza con el flujo `pull -> add -> commit -> push`. Sin embargo, pueden darse situaciones donde se generen conflictos. Veamos como manejarnos en esos casos.

### resolviendo conflictos

Supongamos el siguiente escenario:

 1. los usuarios U1 y U2 trabajan en el mismo proyecto
 2. el usuario U1 clona el repositorio remoto. el usuario U2 hace lo mismo al mismo tiempo. Es decir, que los repositorios locales de U1 y U2 comienzan sincronizados
 3. U1 modifica un archivo (README.md). commitea y pushea el cambio al repositorio remoto
 4. U2 tambien modifica el mismo archivo, pero lo hace despues que U1. Entonces cuando U2 hace commit y push, git le avisa que su repositorio no está al día, por lo que debe hacer primero un `git pull`
 5. U2 hace un `git pull` pero dado que hay una nueva versión del README.md en el repositorio remoto, git intenta fusionar (mergear) ambas copias en el repositorio local de U2. Pueden darse 2 escenarios:
    i. los cambios de U1 y U2 son compatibles. por ejemplo: U1 cambio la línea 1 y U2 la línea 2 del README.md. En este caso git aplica ambos cambios y U2 termina con una versión que incluye tanto sus cambios como los del repositorio remoto. En este caso puede subir la versión final haciendo `git add` y `git push`
   ii. los cambios de U1 y U2 son conflictivos. Por ejemplo: ambos U1 y U2 modificaron la misma línea de maneras diferentes. En este punto git no puede resolver automáticamente el conflicto y le pide a U2 que lo resuelva manualmente. Una vez que U2 decide que hacer (si dejar su cambio o tomar el de U1) confirma el cambio y hace push

Es decir, git va a intentar aplicar todos los cambios posibles siempre que no sean conflictivos. Y si hay conflicto le va a pedir al usuario que lo resuelva.

Probemos resolver un conflicto. Para esto vamos a simular que somos 2 usuarios diferentes clonando el mismo repositorio en 2 carpetas diferentes de la misma máquina:

```
cd Escritorio
mkdir repo1
cd repo1
git clone https://miNombreDeUsuario:miToken@github.com/miNombreDeUsuario/tallerDeGit
cd ..
mkdir repo2
cd repo2
git clone https://miNombreDeUsuario:miToken@github.com/miNombreDeUsuario/tallerDeGit
cd..
```

En este momento tenemos 2 copias del mismo proyecto. Podemos ver que los contenidos de repo1 y repo2 tienen la misma estructura:

```
clinux01@pc1:/tmp$ ls -lR repo*
repo1:
total 4
drwxrwxr-x 3 clinux01 clinux01 4096 may  3 22:00 tallerDeGit

repo1/tallerDeGit:
total 32
-rw-rw-r-- 1 clinux01 clinux01    34 may  3 22:00 README.md
-rw-rw-r-- 1 clinux01 clinux01 25272 may  3 21:59 taller_git.md

repo2:
total 4
drwxrwxr-x 3 clinux01 clinux01 4096 may  3 22:10 tallerDeGit

repo2/tallerDeGit:
total 32
-rw-rw-r-- 1 clinux01 clinux01    34 may  3 22:10 README.md
-rw-rw-r-- 1 clinux01 clinux01 25272 may  3 22:10 taller_git.md
```

Modificamos el README.md del repo1 usando el Editor de Textos y guardamos el archivo. Luego desde la terminal pusheamos el cambio:

```
cd repo1
cd tallerDeGit
git status
git add README.md
git commit -m "hice un cambio en la primer línea"
git push
```

El push funciona correctamente.

Hacemos ahora una modificación en el README.md del repo2 y guardamos el archivo teniendo en cuenta modificar la misma línea pero con diferente contenido. E intentamos pushear con los mismos comandos:


```
cd ..
cd ..
cd repo2
cd tallerDeGit
git status
git add README.md
git commit -m "hice otro cambio en la primer línea"
git push
```

En este punto el push va a fallar y git nos va a decir que hay cambios en el repositorio remoto que no tenemos:

```
clinux01@pc1:~/Escritorio/repo2/tallerDeGit$ git push
To https://github.com/miNombreDeUsuario/tallerDeGit
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/miNombreDeUsuario/tallerDeGit'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

Hacemos `git pull`, pero nos vuelve a aparecer un mensaje donde git nos pide que configuremos una estrategia de merge:

```
clinux01@pc1:~/Escritorio/repo2/tallerDeGit$ git pull
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 307 bytes | 307.00 KiB/s, done.
From https://github.com/miNombreDeUsuario/tallerDeGit
   721f61f..1ec3bed  main       -> origin/main
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint:
hint:   git config pull.rebase false  # merge (the default strategy)
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint:
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
fatal: Need to specify how to reconcile divergent branches.
```

Vamos a usar la estrategia default:

```
git config pull.rebase false
```

Y ahora volvemos a hacer pull:

```
clinux01@pc1:~/Escritorio/repo2/tallerDeGit$ git pull
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

Si vemos el mensaje de error git nos dice que tenemos que editar el README.md de repo2 y solucionar el conflicto que se generó.

Si abrimos el archivo en el Editor de Textos vamos a ver que hay bloques de texto que no estaban en el archivo original (fueron agregados por git):

```
# tallerDeGit
Taller de Git
Hola!
<<<<<<< HEAD
Manola!
=======
Chau!
>>>>>>> 1ec3bedd04acd9bfc842a8b2ee466f73f138d473
```

En este punto tenemos que decidir cuál de los dos cambios dejamos. No hay una regla general sobre cómo hacerlo y muchas veces probablemente tenga sentido comunicarnos con el usuario que hizo el otro cambio para ponernos de acuerdo en cuál es la mejor version.

Y ahora sí podemos pushear

```
git add README.md
git commit -m "hice otro cambio en la primer línea"
git push
```

Si ahora volvemos a la carpeta repo1 y hacemos `git diff` no vamos a ver ninguna diferencia todavia. Esto es porque para tomar los nuevos cambios tenemos que hacer `git pull`:

```
cd ..
cd repo1
git diff README.md
```

Para sincronizar repo2 podemos hacer `git pull`:

```
git pull
```

y ahora sí ambas carpetas están sincronizadas con el repositorio remoto.

## antes de finalizar

Dado que estamos usando computadoras compartidas, vamos a dejar el ambiente ordenado y listo para que otra persona pueda realizar las mismas pruebas que hicimos nosotros. Para esto vamos a:

1. eliminar los archivos `presentacion.txt` y `presentacion_v1.txt` que hayamos creado en el Escritorio del usuario `clinux01`
1. eliminar la carpeta `.git` que quedo en el Escritorio: `rm -rf ~/Escritorio/.git`
1. eliminar las copias de nuestros repositorios locales: `rm -rf ~/Escritorio/repo1 && rm -rf ~/Escritorio/repo2`
1. eliminar el historial de la terminal: `history -c && history -w`

En el caso en que nos interese seguir trabajando sólo tenemos que continuar usando nuestro repositorio remoto con el token correspondiente.

Si bien en los ejemplos que vimos usamos la versión de línea de comando, existen aplicaciones con interfaz gráfica para simplificar la experiencia de uso. Ahora que tienen conocimientos básicos de cómo utilizar git, van a poder entender fácilmente cuáles son los comandos que se están ejecutando por detras.

Recuerden que todos los comandos de git tienen una ayuda en línea a la que pueden acceder ejecutando el comando con el agregado de la opcion `-h` o usando el comando `help`. Por ejemplo: `git add -h` da la versión de ayuda corta y `git help add` la versión larga.

Por ultimo, hay mucho material disponible en la red sobre git, sus comandos y las múltiples formas de usarlo. Estan más que invitados a seguir explorando git por su cuenta :)


## links

- git (pagina oficial): https://git-scm.com/
- github + git: https://docs.github.com/es/get-started/using-git/about-git
