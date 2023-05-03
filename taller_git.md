#Git

git es una herramienta para la gestion de versiones de archivos. Esto quiere decir que nos facilita el versionado de nuestros archivos, permitiendonos entre otras cosas:

 1. entender cual es la ultima version de un archivo
 2. entender cual fue la cadena de modificaciones que le fuimos haciendo a lo largo del tiempo
 3. trabajar en equipo con otros sobre los mismos archivos de manera ordenada
 
Antes de ponernos a usar git vamos a ver primero otro comando: diff

## diff

Supongamos que estoy trabajando en una presentacion para dar una clase. Y que estoy trabajando en un archivo que se llama `presentacion.txt` donde voy anotando el contenido de la presentacion.

Para que el ejemplo se entienda mas facil, lo vamos a ir haciendo juntos. 

Desde una sesion de Ubuntu, abrir la aplicacion "Editor de Textos" y crear un archivo de texto llamado `presentacion.txt` en el Escritorio. Agregar el siguiente contenido y guardarlo:

```
Esta es mi presentacion. La presentacion tiene estos temas:
* tema 1
* tema 2
```

Supongamos ahora que sigo trabajando en mi presentacion, pero como voy a hacer cambios, hago un backup de mi archivo primero:

Abrir una terminal (CTRL + ALT + T). Nos tiene que aparecer un prompt, que nos dice basicamente el nombre del usuario que estamos usando y la carpeta donde estamos posicionados:

```
clinux01@pc1:~$ cd Escritorio/
```

En este caso el usuario se llama `clinux`, es un usuario de la maquina que se llama `pc1` y la carpeta donde estamos posicionados se llama `~` que es una forma abreviada de decir que estamos en la carpeta "home" del usuario (es decir, `/home/clinux`).

Podemos cambiar de carpeta usando el comando `cd` (change directory). Como queremos que nuestro archivo quede en el escritorio vamos a hacer `cd Escritorio`:

```
clinux01@pc1:~$ cd Escritorio/
clinux01@pc1:~/Escritorio$ 

```

Notar que el prompt cambio de `~` a `~/Escritorio`. Esto quiere decir que ahora estamos "parados" en la carpeta del Escritorio.
<!-- 
#Como las maquinas de los laboratorios son compartidas, vamos a crear una nueva carpeta usando el comando `mkdir` (make directory). Vamos a usar un nombre unico. Por ejemplo: `taller1`, `taller2`, etc.
#Listar los archivos con `ls -l` para confirmar que no existe una carpeta previamente creada con el nombre que elegimos:
#
#```
#clinux01@pc1:~/Escritorio$ ls -l
#total 60
#```
#
#Ahora que estamos seguros, creamos nuestra carpeta:
#
#```
#clinux01@pc1:~/Escritorio$ mkdir taller1
#```
#
#Y confirmamos la creacion de la carpeta usando nuevamente `ls -l`:
#```
#clinux01@pc1:~/Escritorio$ ls -l
#total 60
#drwxrwxr-x 2 clinux clinux       4096 may  1 23:46  taller1
#``` -->

Confirmamos que nuestro archivo existe en el escitorio:
```
clinux01@pc1:~/Escritorio$ ls -l
total 60
-rw-rw-r-- 1 clinux clinux   78 may  1 13:14  presentacion.txt
```

Hacer una copia de nuestra presentacion usando el comando `cp` (copy):

```
cp presentacion.txt presentacion_v1.txt
```

Verificar que tenemos los dos archivos y que tienen el mismo tamano (78 bytes):

```
clinux01@pc1:~/Escritorio$ ls -l
total 60
-rw-rw-r-- 1 clinux clinux   78 may  1 13:14  presentacion.txt
-rw-rw-r-- 1 clinux clinux   78 may  1 13:14  presentacion_v1.txt
```

Ahora que tengo backupeado mi archivo lo voy a modificar. Desde el editor de texto que teniamos abierto, agregamos un nuevp tema:

```
Esta es mi presentacion. La presentacion tiene estos temas:
* tema 1
* tema 2
* tema 3
```

Guardamos el archivo. 

Si listamos los archivos del Escritorio vamos a poder comprobar que efectivamente los tamanos de los archivos ahora son distintos (87 bytes vs 78 bytes):

```
ls -l
clinux01@pc1:~/Escritorio$ ls -l
total 64
-rw-rw-r-- 1 clinux clinux   87 may  1 13:15  presentacion.txt
-rw-rw-r-- 1 clinux clinux   78 may  1 13:15  presentacion_v1.txt
```

En este momento podemos utilizar el comando de linux `diff` para ver las diferencias entre las 2 versioes de nuestra presentacion:

```
clinux01@pc1:~/Escritorio$ diff presentacion.txt  presentacion_v1.txt 
4d3
< * tema 3
```

La salida de `diff` nos confirma que efectivamente los archivos son identicos salvo por 1 linea que es diferente. 
Si miramos la salida de diff vamos a ver el signo '<' que indica que esa linea esta en el archivo "de la izquierda", o el primer parametro que le pasamos a diff (`presentacion.txt` en nuestro caso).

Veamos que pasa si ahora modificamos el otro archivo y le agregamos un tema 4. El archivo `presentacion_v1.txt` nos deberia quedar asi:

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

Ahora es mas claro que diff nos esta avisando que la linea que menciona al tema 3 solo esta en el primer archivo, y la que menciona al tema 4 esta solo en el segundo archivo.

NOTA: diff tambien nos esta diciendo `4c4`, que significa que la linea 4 del primer archivo cambio (c=changed) respecto de la linea 4 del segundo archivo. En el caso anterior nos decia 4d3, lo que significa que la linea 4 del archivo de la izquierda fue eliminada (d=deleted) en el segundo archivo.

NOTA: la mayoria de los comandos de linux tienen una ayuda disponible en la forma de un "manual de instrucciones". Para ver el de `diff` ejecutar desde una terminal `man diff`

Ahora que podemos ver los cambios entre las diferentes versiones del archivo podemos trabajar con un poco mas de tranquilidad ya que podemos entender mas facilmente cuales son las diferencias entre ellos.

Por ejemplo, podria darse el caso de que quiere trabajaen en la presentacion en una compitadora de la facultad durante el dia y en mi casa por la noche. Entonces cada vez que copio los archivos podria hacer un diff para verificar que tengo todos los cambios y no estoy pisando por error cambios que me interesan.


## usando git

git nos va a permitir ir entendiendo las diferencias entre nuestros archivos de forma mas sencilla.

Lo primero que vamos a hacer es inicializar un "repositorio" que es el nombre que le da git a una coleccion de archivos:

```

clinux01@pc1:~/Escritorio$ git init
Inicializado repositorio Git vacío en /home/clinux01/Escritorio/.git/
```

Si listamos el contenido de la carpeta vamos a seguir viendo nuestros archivos:

```
clinux01@ASUS:~/Escritorio$ ls -l
total 64
-rw-rw-r-- 1 clinux clinux   87 may  1 13:15  presentacion.txt
-rw-rw-r-- 1 clinux clinux   87 may  1 13:22  presentacion_v1.txt
```

Pero si agregamos la opcion `-a` vamos a poder ver una carpeta oculta llamada `.git`:

```
total 76
drwxr-xr-x  6 clinux clinux 4096 may  1 13:38  .
drwxr-xr-x 97 clinux clinux 4096 may  1 13:22  ..
drwxrwxr-x  7 clinux clinux 4096 may  1 13:38  .git
-rw-rw-r--  1 clinux clinux   87 may  1 13:15  presentacion.txt
-rw-rw-r--  1 clinux clinux   87 may  1 13:22  presentacion_v1.txt
```

NOTA: tambien aparecen 2 carpetas mas: '.', que representa la carpeta actual y '..' que representa a la carpeta "padre" de la actual. Es por esto que si hacemos `cd ..` cambiamos de carpeta a la que esta en el nivel superior. Y de la misma forma si hacemos `cd .` nos quedamos posicionados en la misma carpeta donde estamos.

Podemos ver que en la carpeta `.git` hay varios archivos y carpetas:

```
clinux01@pc1:~/Escritorio$ ls -l .git
total 32
drwxrwxr-x 2 clinux clinux 4096 may  1 13:38 branches
-rw-rw-r-- 1 clinux clinux   92 may  1 13:38 config
-rw-rw-r-- 1 clinux clinux   73 may  1 13:38 description
-rw-rw-r-- 1 clinux clinux   23 may  1 13:38 HEAD
drwxrwxr-x 2 clinux clinux 4096 may  1 13:38 hooks
drwxrwxr-x 2 clinux clinux 4096 may  1 13:38 info
drwxrwxr-x 4 clinux clinux 4096 may  1 13:38 objects
drwxrwxr-x 4 clinux clinux 4096 may  1 13:38 refs

```

Estos archivos y carpetas son internos a git y son los que va a usar para llevar la cuenta de las modificaciones que vamos realizando, asi que no los vamos a modificar.

Ahora podemos preguntarle a git cual es el estado de los archivos a traves del comando `git status`:

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

Lo que nos esta diciendo git es que:
 1. existen 2 archivos de texto (nuestras presentaciones) pero que no estan bajo el control de git (es decir, que git los desconoce)
 2. que podemos usar el comando `git add` para empezar a trackearlos

Como nos interesa seguir trabajando en nuestra presentacion vamos a pedirle a git que lo empiece a "seguir". Para esto usamos `git add`:

```
clinux01@pc1:~/Escritorio$ git add presentacion.txt
clinux01@pc1:~/Escritorio$ 
```

Vemos que el comando no genera ningun mensaje por pantalla. Asi que la forma de confirmar que hubo algun cambio es volver a pedirle el status:

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

Esto nos confirma que presentacion.txt esta ahora bajo el radar de git, pero sin embargo necesitamos confirmarle a git que realmente queremos que lo siga. Esta confirmacion (commitment) lo hacemos a traves del comando `git commit`:

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

Como las maquinas de los laboratorios son compartidas vamos a realizar la configuracion de manera local (omitiendo el parametro `--global`):

```
clinux01@pc1:~/Escritorio$ git config user.email [direccion de mail]
clinux01@pc1:~/Escritorio$ git config user.name [Nombre y Apellido entre comillas]
 
```

NOTA: podemos ver la configuracion de git ejecutando `git config --list`.


NOTA: git nos pide que cada vez que confirmemos un cambio (commit) pongamos un mensaje breve que nos permita entender (a nosotros mismos o a otros) de que se trata el cambio que estamos haciendo. Este mensaje es el que le pasamos cuando pusimos la opcion `-m mensaje` como parametro de `git commit`:

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

Ahora que hemos confirmado el cambio sobre `presentacion.txt`, esto significa que podemos preguntarle a git especificamente por su historial de cambios:

```
clinux01@pc1:~/Escritorio$ git log presentacion.txt
commit f1b601081919422cd86112a137bad52c9c0dce61 (HEAD -> master)
Author: Usuario generico <usuario@dc.uba.ar>
Date:   Mon May 1 13:52:14 2023 -0300

    version inicial de la presentacion
```

NOTA: podemos ver que nuestro commit tiene un valor hexadecimal asociado que es el resultado de aplicar una funcion de hash sobre nuestros cambios. Cada vez que hacemos un commit, git calcula un nuevo hash y lo asocia al commit. Esto le permite identificar de manera univoca a cada uno de los commits que vamos haciendo.

Ahora podemos seguir trabajando en nuestra presentacion y seguirle sumando cambios. Por ejemplo, podemos agregar un tema 5 y eliminar el tema 1:

```
Esta es mi presentacion. La presentacion tiene estos temas:
* tema 2
* tema 3
* tema 5
```

Dado que presentacion.txt esta bajo la gestion de git, entonces podemos preguntarle cuales son las diferencias con la version que git conoce. Para esto usamos el comando `git diff`:

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

La salida es parecida a la que vimos cuando ejecutabamos `diff` a mano, pero es un poco mas detallada:

 * nos dice que hay una linea que fue eliminada (-)
 * nos dice que hay una linea que fue agregada (+)
 * nos dice que los cambios estan enre las lineas 1 y 4 del archivo

Es decir, que ademas de darnos las diferencias, nos muestra un poco mas de contexto para que nos resulte mas facil entenderlas.

Si estamos satisfechos con los cambios que hicimos, entonces le decimos a git que queremos agregar los cambios del archivo (`git add`) y se lo confirmamos (`git commit`):

```
clinux01@pc1:~/Escritorio$ git commit -m "agregue tema. saque tema"
[master 2cc3337] agregue tema. saque tema
 1 file changed, 1 insertion(+), 1 deletion(-)
```

Nuevamente, en la ultima linea del mensaje, git nos esta resumiendo los cambios que detecto desde la ultima vez que le mandamos el archivo (1 insercion, 1 borrado).

Si pedimos la historia del archivo vamos a ver los 2 cambios/versiones, ordenados de mas receiente a menos reciente:

```
commit 2cc3337d5efbe503a3f69af0084eab5b51475262 (HEAD -> master)
Author: Usuario generico <usuario@dc.uba.ar>
Date:   Mon May 1 14:04:22 2023 -0300

    agregue tema. saque tema

commit f1b601081919422cd86112a137bad52c9c0dce61
Author: Usuario generico <usuario@dc.uba.ar>
Date:   Mon May 1 13:52:14 2023 -0300

    version inicial de la presentacion
```

De esta forma evitamos tener que ir recordando los cambios que vamos haciendo, ya que git lo hace por nosotros. Tambien nos permite volver a versiones anteriores y muchas cosas mas. Pero antes de verlas, vamos a ver un poco mas en detalle por que el proceso de cambios se hace en 2 etapas.

## stages

Podemos pensar que en principio hay 2 mundos: uno donde nosotros somos due~nos de los archivos y sobre los que git no sabe nada ni tiene potestad, y otro donde git es el responsable de gestionar los cambios de los archivos que conoce.

Entonces lo que queremos hacer es ir avisandole sobre los archivos que queremos incluir bajo su control de versiones. Podemos pensar que la forma en que vamos pasando archivos de un mundo al otro es a traves de `git add`. 

La gran ventaja que tiene git es que no se olvida de nada de lo que va pasando. Es decir, cada modificacion de un archivo la registra. Esto puede ser un problema, por ejemplo, si queremos que se "olvide" de algunos archivos que ya no nos sriven mas. El proceso para eliminar un archivo del control de versiones es:

 1. git rm [archivo]
 2. git commit [archivo] -m [mesaje]

El inconveniente es que si bien git nos va a confirmar que ya no sabe nada de nuestro archivo borrado la realidad es que sigue guardando toda la historia hasta ese momento. Es decir, hay archivos dentro de la carpeta `.git` que siguen guardando la informacion de cambios historica del archivo.
Esto puede ser un problema si somos ansiosos y agregamos un monton de archivos y luego nos arrepentimos, ya que igual quedarian en la historia de git.

Para evitar ese problema (y otros) git usa un tercer "mundo" que es un intermedio entre el mundo de git y el de nuestros archivos no-versionados. Esta area de pruebas o `stage` es donde se van agregando los cambios hasta que se confirman (commit). El comando commit es el que termina pasando los cambios al mundo de git.

[diagrama]

Es por esta razon que cada vez que hacemos un add, tenemos que hacer un commit si queremos que nuestro cambio sea tomado efectivamente por git.

## trabajo en grupo

Hasta ahora todos los ejemplos que hicimos fueron considerando el trabajo individual de 1 sola persona. Sin embargo, esto no es un escenario realista en el contexto de un proyecto de desarrollo de software, donde hay grupos de programadores trabajando sobre el mismo codigo. 
Sin ir mas lejos, el trabajo practico de la materia se realiza de manera grupal.

Cuando hay varias personas trabajando sobre el mismo grupo de archivos es necesario que haya algun tipo de acuerdo en la forma en que se va a trabajar, para lograr que los cambios se hagan de manera ordenada. 

Retomando el ejemplo de la presentacion, podria pedirle a mis compa~neros de equipo que me ayuden a completar los slides para avanzar mas rapido. Para esto vamos a necesitar que todos accedan al mismo repositorio (hasta ahora mi repositorio era local, por lo que solo podia ser usado por mi en la computadora donde lo cree usando `git init`). 

### repositorio remoto

Para poder compartir el repositorio entre varias personas lo que vamos a hacer es armar un repositorio de git remoto, es decir, que va a estar corriendo como un servicio en alguna maquina de la red que sea accesible por todos los integrantes del proyecto. Una posibilidad seria instalar y configurar ese servicio en una maquina del laboratorio, de esa forma seria accessible desde cualquier otra maquina en la red del laboratorio. 

Generalmente lo que se suele hacer es hacer que ese servicio este disponible a traves de un servicio web, de la misma forma que una pagina web. Por ejemplo, los siguientes son servicios que ofrecen git y pueden ser utilizados por cualquiera que tenga acceso a Internet:

 * http://www.github.com
 * http://www.gitlab.com

Por ejemplo, si la decision es usar github entoncrs todos los integrantes del proyecto deberian crearse una cuenta en ese servicio y a partir de ese momento podrian utilizar los mismos comandos de git que estuvimos viendo (mas algunos comandos adicionales que vamos a ver en breve).

La idea principal del repositorio remoto es que va funcionar como la "verdad ultima" (`ultimate source of truth`) respecto de cual es la ultima version de cada uno de los archivos que este bajo la administracion de git.

### flujo de trabajo

De la misma forma que habiamos inicializado nuestro repositorio local a traves del comando `git init` en el caso de un repositorio remoto lo vamos a hacer a traves de la interfaz web del servicio que estemos utilizando. 

Esto se hace por unica vez al comienzo del proyecto. A partir de ese momento cada uno de los integrantes del proyecto tiene que acceder al repositorio. Esto se puede hacer de varias maneras, pero nosotros lo vamos a hacer a traves de un `fork` para evitar pisarnos con otros alumnos haciendo las mismas pruebas:

```
```

Desde una terminal vamos a clonar nuestro repositorio remoto:

```
git clone https://
```

Si hacemos `ls -l` vamos a ver que hay una nueva carpeta que contiene los archivos del proyecto que acabamos de clonar:

```
```

Tenemos entonces una copia local del repositorio remoto. 

En este momento vamos a modificar uno de los archivos. Podemos agregar, borrar o cambiar el contenido existente.
Como siempre, vamos a llevar el archivo al area de stage con `git add` y vamos a confirmar el cambio con `git commit -m [mensaje]`. 
La diferencia principal es que los cambios que tenemos commiteados solo estan por ahora en nuestra maquina. Para que el repositorio remoto se entere de que hicimos cambios se lo tenemos que notificar usando el comando `git push`:

```
git push
```

NOTA: en realidad git nos va a pedir que nos identifiquemos antes de subir codigo al repositorio. Para esto vamos a tener que decirle nuestro nombre y nuestra direccion de mail:

```
git config...
```
Por suerte esto lo vamos a tener que hacer solo la primera vez.

Ahora si, si vamos a la interfaz web del proyecto, vamos a ver que se registro el cambio que acabamos de confirmar.

El resto de las personas que trabajan con nosotros en el proyecto van a estar realizando cambios tambien en sus copias locales, y psuheandolas eventualmente al repositorio remoto. Para obtener esos cambios realizados por otros (si los hubiera) es que vamos a realizar `git pull`:

```
git pull
```

Basicamente lo que hace `git pull` es hacer un diff de nuestro repositorio local contra el remoto, y aplicar las diferencias (patches) en el orden correcto para que ambos queden sincronizados. 
La mayor parte del tiempo es probable que los cambios realizados por distintos colaboradores no se interfieran entre si. En esos casos alcanza con el flujo `pull -> add -> commit -> push`. Sin embargo, pueden darse situaciones donde se generen conflictos. Veamos como manejarnos en esos casos.

### resolviendo conflictos

Supongamos el siguiente escenario:

 1. los usuarios U1 y U2 trabajan en el mismo proyecto
 2. el usuario U1 clona el repositorio remoto. el usuario U2 hace lo mismo al mismo tiempo. Es decir, que los repositorios locales de U1 y U2 comienzan sincronizados
 3. U1 modifica un archivo (README.md). commitea y pushea el cambio al repositorio remoto
 4. U2 tambien modifica el mismo archivo, pero lo hace mas lento que U1. Entonces cuando U2 hace commuit y push, git le avisa que su repositorio no esta al dia, por lo que debe hacer primero un `git pull`
 5. U2 hace un `git pull` pero dado que hay una nueva version del README.md en el repositorio remoto, git intenta fusionar (mergear) ambas copias en el repositorio local de U2. Pueden darse 2 escenarios:
    i. los cambios de U1 y U2 son compatibles. por ejemplo: U1 cambio la linea 1 y U2 la linea 2 del README.md. En este caso git aplica ambos cambios y U2 termina con una version que incluye tanto sus cambios como los del repositorio remoto. En este caso puede subir la version final haciendo `git add` y `git push`
   ii. los cambios de U1 y U2 son conflictivos. por ejemplo: ambos U1 y U2 modificaron la misma linea de maneras diferentes. En este punto git no puede resolver automaticamente el conflicto y le pide a U2 que lo resuelva manualmente. Una vez que U2 decide que hacer (si dejar su cambio o tomar el de U1) confirma el cambio y hace push

Es decir, git va a intentar aplicar todos los cambios posibles siempre que no sean conflictivos. Y si hay conflicto le va a pedir al usuario que lo resuelva. 

Probemos resolver un conflicto. Para esto vamos a simular que somos 2 usuarios diferentes clonando el mismo repositorio en 2 carpetas diferentes de la misma maquina:

```
cd Escritorio
mkdir repo1
git clone https://
mkdir repo2
git clone https://
ls -lR
```

En este momento tenemos 2 copias del mismo proyecto. Modificamos el README.md del repo1 usando el Editor de Textos y guardamos el archivo. Luego desde la terminal pusheamos el cambio:

```
cd repo1
git status
git add README.md
git commit -m "hice un cambio en la primer linea"
git push
```

El push funciona correctamente.

Hacemos ahora una modificacion en el README.md del repo2 y guardamos el archivo teniendo en cuenta modificar la misma linea pero con diferente contenido. E intentamos pushear con los mismos comandos:


```
cd ..
cd repo2
git status
git add README.md
git commit -m "hice otro cambio en la primer linea"
git push
```

En este punto el push va a fallar. Si vemos el mensaje de error nos dice que 
 1. tenemos que editar el README.md de repo2 y solucionar el conflicto
 2. una vez que hayamos decidido como resolverlo, tenemos que pushear la version final


Si abrimos el archivo en el Editor de Textos vamos a ver que hay bloques de texto que no estaban en el archivo original (fueron agregados por git):

```
```

En este punto tenemos que decidir cual de los dos cambios dejamos. No hay una regla general sobre como hacerlo y muchas veces probablemente tenga sentido comunicarnos con el usuario que hizo el otro cambio para ponernos de acuerdo en cual es la mejor version. 
Una vez hecho el cambio podemos hacer `git diff` para ver que es lo que vamos a pushear:

```
git diff
```

Y ahora si podemos pushear

```
git add README.md
git commit -m "hice otro cambio en la primer linea"
git push
```

Si ahora volvemos a la carpeta repo1 podemos ver que nuestro archivo quedo viejo respecto del repositorio remoto:

```
cd ..
cd repo2
git diff README.md
```

Para sincronizar repo2 podemos hacer `git pull`:

```
git pull

```

y ahora si ambas carpetas estan sincronizadas con el repositorio remoto.



## links

git (pagina oficial): https://git-scm.com/
github + git: https://docs.github.com/en/get-started/using-git/about-git



