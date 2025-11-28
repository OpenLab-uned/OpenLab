Aprenderemos cómo gestionar código con git. Temario:
- ¿Qué es git?
- Historia de git
- Fundamentos
- Ramas, Merges, Rebase
- Forma de trabajo
-  Tripas de git
Material didáctico: libro de git:  [github.com/progit2/releases/download/progit.pdf](https://github.com/progit/progit2/releases/download/2.1.448/progit.pdf).
### Sobre el control de versiones
Un control de versiones es un sistema que registra los cambios realizados en un archivo o conjunto de archivos a lo largo del tiempo, de modo que puedas recuperar versiones específicas más adelante.
Dicho sistema te permite regresar a versiones anteriores de tus archivos, regresar a una versión anterior del proyecto completo, comparar cambios a lo largo del tiempo, ver quién modificó por última vez algo que pueda estar causando problemas, ver quién introdujo un problema y cuándo, y mucho más. Usar un VCS también significa generalmente que si arruinas o pierdes archivos, será posible recuperarlos fácilmente. Adicionalmente, obtendrás todos estos beneficios a un costo muy bajo.
Sistema de control de versiones centralizado / Sistema de control de versiones distribuido.

### Breve historia de Git
En el 2002, el proyecto del kernel de Linux empezó a usar un DVCS propietario llamado BitKeeper.
En 2005 BitKeeper dejó de ser gratuito, esto impulsó a la comunidad de desarrollo de Linux (y en particular a Linus Torvalds, el creador de Linux) a desarrollar su propia herramienta. Algunos de los objetivos del nuevo sistema fueron los siguientes:
- Velocidad
- Diseño sencillo
- Gran soporte para desarrollo no lineal (miles de ramas paralelas)
- Completamente distribuido
- Capaz de manejar grandes proyectos (como el kernel de Linux) eficientemente (velocidad y tamaño de los datos)

### Fundamentos de Git
Cada vez que confirmas un cambio, o guardas el estado de tu proyecto en Git, él básicamente toma una foto del aspecto de todos tus archivos en ese momento y guarda una referencia a esa copia instantánea. Para ser eficiente, si los archivos no se han modificado Git no almacena el archivo de nuevo, sino un enlace al archivo anterior idéntico que ya tiene almacenado. Git maneja sus datos como una secuencia de copias instantáneas.

![[Pasted image 20251121081309.png]]

**Casi todas las operaciones son locales: velocidad y no necesidad de conexión**
La mayoría de las operaciones en Git sólo necesitan archivos y recursos locales para funcionar.
Por lo general no se necesita información de ningún otro computador de tu red. Debido a que tienes toda la historia del proyecto ahí mismo, en tu disco local, la mayoría de las operaciones parecen prácticamente inmediatas.
Por ejemplo, para navegar por la historia del proyecto, Git no necesita conectarse al servidor para obtener la historia y mostrártela - simplemente la lee directamente de tu base de datos local.

**Tiene integridad**
Todo en Git es verificado mediante una suma de comprobación antes de ser almacenado, y es identificado a partir de ese momento mediante dicha suma.
El mecanismo que usa Git para generar esta suma de comprobación se conoce como hash SHA-1. Se trata de una cadena de 40 caracteres hexadecimales (0-9 y a-f), y se calcula con base en los contenidos del archivo o estructura del directorio en Git.
Git guarda todo no por nombre de archivo, sino por el valor hash de sus contenidos.

**Git generalmente solo añade información**
Cuando realizas acciones en Git, casi todas ellas sólo añaden información a la base de datos de Git. Es muy difícil conseguir que el sistema haga algo que no se pueda enmendar, o que de algún modo borre información.  Como en cualquier VCS, puedes perder o estropear cambios que no has confirmado todavía. Pero después de confirmar una copia instantánea en Git es muy difícil perderla, especialmente si envías tu base de datos a otro repositorio con regularidad.

### Los Tres Estados
Los tres estados de git en los que puedes encontrar tus archivos:
![[Pasted image 20251121083716.png]]
El directorio de Git es donde se almacenan los metadatos y la base de datos de objetos para tu proyecto. Es la parte más importante de Git, y es lo que se copia cuando clonas un repositorio desde otra computadora.

El directorio de trabajo es una copia de una versión del proyecto. Estos archivos se sacan de la base de datos comprimida en el directorio de Git, y se colocan en disco para que los puedas usar o modificar.
El área de preparación es un archivo, generalmente contenido en tu directorio de Git, que almacena información acerca de lo que va a ir en tu próxima confirmación.
El flujo de trabajo básico en Git es algo así:

1. Modificas una serie de archivos en tu directorio de trabajo.
2. Preparas los archivos, añadiéndolos a tu área de preparación.
3. Confirmas los cambios, lo que toma los archivos tal y como están en el área de preparación y almacena esa copia instantánea de manera permanente en tu directorio de Git.
Si una versión concreta de un archivo está en el directorio de Git, se considera confirmada (committed). Si ha sufrido cambios desde que se obtuvo del repositorio, pero ha sido añadida al área de preparación, está preparada (staged). Y si ha sufrido cambios desde que se obtuvo del repositorio, pero no se ha preparado, está modificada (modified).


**La línea de comandos es el único lugar en donde puedes ejecutar todos los comandos de Git - la mayoría de interfaces gráficas de usuario solo implementan una parte de las características de Git por motivos de simplicidad.**


### Instalación de Git
https://git-scm.com/book/es/v2/Inicio---Sobre-el-Control-de-Versiones-Instalaci%C3%B3n-de-Git

### Configuración de Git
Git trae una herramienta llamada `git config`, que te permite obtener y establecer variables de configuración que controlan el aspecto y funcionamiento de Git. Estas variables pueden almacenarse en tres sitios distintos (se organiza en varios archivos: para tu usuario, para los usuarios del sistema, cada repo..): 
`/etc/gitconfig` , `~/.gitconfig`  `~/.config/git/config`
Técnicamente podrías escribir ahí la configuración global que quieres pero lo suyo es hacerlo a través de los comandos: 
```bash
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
git help config
```
 Sólo necesitas hacer esto una vez si especificas la opción `--global`, ya que Git siempre usará esta información para todo lo que hagas en ese sistema. Si quieres sobrescribir esta información con otro nombre o dirección de correo para proyectos específicos, puedes ejecutar el comando sin la opción `--global` cuando estés en ese proyecto.

### Obtener ayuda
```bash
git help <verb>
git <verb> --help
man git-<verb>
```

### Fundamentos de Git
Esta parte cubre todos los comandos básicos que necesitas para hacer la gran mayoría de cosas a las que eventualmente vas a dedicar tu tiempo mientras trabajas con Git.
Al final del capítulo, sabremos: configurar e inicializar un repositorio, comenzar y detener el seguimiento de archivos, y preparar (stage) y confirmar (commit) cambios. También te enseñaremos a configurar Git para que ignore ciertos archivos y patrones, cómo enmendar errores rápida y fácilmente, cómo navegar por la historia de tu proyecto y ver cambios entre confirmaciones, y cómo enviar (push) y recibir (pull) de repositorios remotos.

 **Obteniendo un repositorio de Git**
 Puedes obtener un proyecto Git de dos maneras. La primera es tomar un proyecto o directorio existente e importarlo en Git. La segunda es clonar un repositorio existente en Git desde otro servidor.
 ```bash
 git init 
 git add
 git add LICENSE
 git commit -m "initial project version"
 ```
 El `git init`  crea un subdirectorio nuevo llamado `.git`, el cual contiene todos los archivos necesarios del repositorio – un esqueleto de un repositorio de Git. Todavía no hay nada en tu proyecto que esté bajo seguimiento.
 `git add` para especificar qué archivos quieres controlar, seguidos de un `git commit` para confirmar los cambios.

Clonar un repositorio existente:
Git recibe una copia de casi todos los datos que tiene el servidor. Cada versión de cada archivo de la historia del proyecto es descargada por defecto cuando ejecutas `git clone`. De hecho, si el disco de tu servidor se corrompe, puedes usar cualquiera de los clones en cualquiera de los clientes para devolver el servidor al estado en el que estaba cuando fue clonado
```bash
git clone https://github.com/libgit2/libgit2
```
Esto crea un directorio llamado `libgit2`, inicializa un directorio `.git` en su interior, descarga toda la información de ese repositorio y saca una copia de trabajo de la última versión. Si te metes en el directorio `libgit2`, verás que están los archivos del proyecto listos para ser utilizados. Si quieres clonar el repositorio a un directorio con otro nombre que no sea `libgit2`, puedes especificarlo con la siguiente opción de línea de comandos:
```bash
git clone https://github.com/libgit2/libgit2 mylibgit
```
Git te permite usar distintos protocolos de transferencia. El ejemplo anterior usa el protocolo `https://`, pero también puedes utilizar `git://` o `usuario@servidor:ruta/del/repositorio.git` que utiliza el protocolo de transferencia SSH. En [Configurando Git en un servidor](https://git-scm.com/book/es/v2/ch00/r_git_on_the_server) se explicarán todas las opciones disponibles a la hora de configurar el acceso a tu repositorio de Git, y las ventajas e inconvenientes de cada una.

**Guardando cambios en el repositorio**
Ya tienes un repositorio Git y un _checkout_ o copia de trabajo de los archivos de dicho proyecto. El siguiente paso es realizar algunos cambios y confirmar instantáneas de esos cambios en el repositorio cada vez que el proyecto alcance un estado que quieras conservar.

Ya tienes un repositorio Git y un _checkout_ o copia de trabajo de los archivos de dicho proyecto. El siguiente paso es realizar algunos cambios y confirmar instantáneas de esos cambios en el repositorio cada vez que el proyecto alcance un estado que quieras conservar.
Mientras editas archivos, Git los ve como modificados, pues han sido cambiados desde su último _commit_. Luego preparas estos archivos modificados y finalmente confirmas todos los cambios preparados, y repites el ciclo.
![[Pasted image 20251121093423.png]]
```bash
git status
git status -s (abreviado)
```
Información de en qué rama estas, te aparecerán los archivos sin rastrea. Para rastrear archivos nuevos: `git add`
Si confirmas en este punto, se guardará en el historial la versión del archivo correspondiente al instante en que ejecutaste `git add`. Anteriormente cuando ejecutaste `git init`, ejecutaste luego `git add (files)` - lo cual inició el rastreo de archivos en tu directorio. El comando `git add` puede recibir tanto una ruta de archivo como de un directorio; si es de un directorio, el comando añade recursivamente los archivos que están dentro de él.

Cuando se modifica un archivo que es rastreado, aparecerá un mensaje "Cambios no preparado para confirmar". Para prepararlo ejecutamos el comando `git add`
Es decir, `git add`lo usamos para rastrear archivos nuevos, preparar archivos, y hacer otras cosas como marcar archivos en conflicto por combinación como resueltos. **Es más útil que lo veas como un comando para “añadir este contenido a la próxima confirmación” más que para “añadir este archivo al proyecto”.**

Modo abreviado: Los archivos nuevos que no están rastreados tienen un `??` a su lado, los archivos que están preparados tienen una `A` y los modificados una `M`. El estado aparece en dos columnas - la columna de la izquierda indica el estado preparado y la columna de la derecha indica el estado sin preparar.

(Aqui añadir cambios a preparado y luego hacer más cambios, en `git status`saldrá un mensaje de que hay cambios preparados para confirmar y cambios no preparados sobre el mismo archivo. Si confirmas en este puntos sólo se añadirán los cambios confirmados)

**Ignorar Archivos**
A veces, tendrás algún tipo de archivo que no quieres que Git añada automáticamente o más aun, que ni siquiera quieras que aparezca como no rastreado. Este suele ser el caso de archivos generados automáticamente como trazas o archivos creados por tu sistema de compilación. En estos casos, puedes crear un archivo llamado `.gitignore` que liste patrones a considerar. Este es un ejemplo de un archivo `.gitignore`:
```bash
cat .gitignore
*.[oa]
*~
```

La primera línea le indica a Git que ignore cualquier archivo que termine en “.o” o “.a” - archivos de objeto o librerías que pueden ser producto de compilar tu código. La segunda línea le indica a Git que ignore todos los archivos que terminen con una tilde (`~`), la cual es usada por varios editores de texto como Emacs para marcar archivos temporales. También puedes incluir cosas como trazas, temporales, o pid directamente; documentación generada automáticamente; etc. Crear un archivo `.gitignore` antes de comenzar a trabajar es generalmente una buena idea, pues así evitas confirmar accidentalmente archivos que en realidad no quieres incluir en tu repositorio Git.

Las reglas sobre los patrones que puedes incluir en el archivo `.gitignore` son las siguientes:
- Ignorar las líneas en blanco y aquellas que comiencen con `#`.
- Emplear patrones glob estándar que se aplicarán recursivamente a todo el directorio del repositorio local.
- Los patrones pueden comenzar en barra (`/`) para evitar recursividad.
- Los patrones pueden terminar en barra (`/`) para especificar un directorio.
- Los patrones pueden negarse si se añade al principio el signo de exclamación (`!`).

Los patrones glob son una especie de expresión regular simplificada usada por los terminales. Un asterisco (`*`) corresponde a cero o más caracteres; `[abc]` corresponde a cualquier caracter dentro de los corchetes (en este caso a, b o c); el signo de interrogación (`?`) corresponde a un caracter cualquiera; y los corchetes sobre caracteres separados por un guión (`[0-9]`) corresponde a cualquier caracter entre ellos (en este caso del 0 al 9). También puedes usar dos asteriscos para indicar directorios anidados; `a/**/z` coincide con `a/z`, `a/b/z`, `a/b/c/z`, etc.
```bash
# ignora los archivos terminados en .a
*.a

# pero no lib.a, aun cuando había ignorado los archivos terminados en .a en la línea anterior
!lib.a

# ignora unicamente el archivo TODO de la raiz, no subdir/TODO
/TODO

# ignora todos los archivos del directorio build/
build/

# ignora doc/notes.txt, pero no este: doc/server/arch.txt
doc/*.txt

# ignora todos los archivos .txt del directorio doc/
doc/**/*.txt
```


**Ver los Cambios Preparados y No Preparados**
Si el comando `git status` es muy impreciso para ti - quieres ver exactamente que ha cambiado, no solo cuáles archivos lo han hecho - puedes usar el comando `git diff`,  te muestra las líneas exactas que fueron añadidas y eliminadas, es decir, el parche.
Este comando compara lo que tienes en tu directorio de trabajo con lo que está en el área de preparación. El resultado te indica los cambios que has hecho pero que aun no has preparado.
Si quieres ver lo que has preparado y será incluido en la próxima confirmación, puedes usar `git diff --staged`. Este comando compara tus cambios preparados con la última instantánea confirmada.
Es importante resaltar que al llamar a `git diff` sin parámetros no verás los cambios desde tu última confirmación - solo verás los cambios que aun no están preparados. Esto puede ser confuso porque si preparas todos tus cambios, `git diff` no te devolverá ninguna salida.
```bash
git diff para ver qué está sin preparar
para ver que has preparado hasta ahora (staged y cached son sinónimos)
git diff --staged 
git diff --cached
git difftool (interfaz gráfica)
difftool --tool-help (para ver qué hay en tu sistema)
```

**Confirmar tus cambios**
Para poder confirmar cambios tienen que estar preparados.
```bash
git commit
```

Al hacerlo, arrancará el editor de tu preferencia. (El editor se establece a través de la variable de ambiente `$EDITOR` de tu terminal - usualmente es vim o emacs, aunque puedes configurarlo con el editor que quieras usando el comando `git config --global core.editor`. 
Puedes ver que el mensaje de confirmación por defecto contiene la última salida del comando `git status` comentada y una línea vacía encima de ella. Puedes eliminar estos comentarios y escribir tu mensaje de confirmación, o puedes dejarlos allí para ayudarte a recordar qué estás confirmando. (Para obtener una forma más explícita de recordar qué has modificado, puedes pasar la opción `-v` a `git commit`. Al hacerlo se incluirá en el editor el diff de tus cambios para que veas exactamente qué cambios estás confirmando). Cuando sales del editor, Git crea tu confirmación con tu mensaje (eliminando el texto comentado y el diff).

Otra alternativa es escribir el mensaje de confirmación directamente en el comando `commit` utilizando la opción -m:
```bash
git commit -m "Story 182: Fix benchmarks for speed"
```

Puedes ver que la confirmación te devuelve una salida descriptiva: indica cuál rama has confirmado (`master`), que _checksum_ SHA-1 tiene el _commit_ (`463dc4f`), cuántos archivos han cambiado y estadísticas sobre las líneas añadidas y eliminadas en el _commit_.

**Cada vez que realizas un _commit_, guardas una instantánea de tu proyecto la cual puedes usar para comparar o volver a ella luego.**

**Saltar el área de comprobación**
```bash
git commit -a -m 'added new benchmarks'
```
Si quieres saltarte el área de preparación, Git te ofrece un atajo sencillo. Añadiendo la opción `-a` al comando `git commit` harás que Git prepare automáticamente todos los archivos rastreados antes de confirmarlos, ahorrándote el paso de `git add`.

**Eliminar archivos**
Para eliminar archivos de Git, debes eliminarlos de tus archivos rastreados (o mejor dicho, eliminarlos del área de preparación) y luego confirmar. Para ello existe el comando `git rm`, que además elimina el archivo de tu directorio de trabajo de manera que no aparezca la próxima vez como un archivo no rastreado.
```bash
git rm PROJECTS.md
```
Si modificaste el archivo y ya lo habías añadido al índice, tendrás que forzar su eliminación con la opción `-f`. Esta propiedad existe por seguridad, para prevenir que elimines accidentalmente datos que aun no han sido guardados como una instantánea y que por lo tanto no podrás recuperar luego con Git.
Esto puede ser particularmente útil si olvidaste añadir algo en tu archivo `.gitignore` y lo preparaste accidentalmente, algo como un gran archivo de trazas a un montón de archivos compilados `.a`. Para hacerlo, utiliza la opción `--cached`
```bash
git rm --cached README
```
Al comando `git rm` puedes pasarle archivos, directorios y patrones glob. Lo que significa que puedes hacer cosas como
```bash
git rm log/\*.log
```
La barra invertida (`\`) antes del asterisco `*`. Esto es necesario porque Git hace su propia expansión de nombres de archivo, aparte de la expansión hecha por tu terminal. Este comando elimina todos los archivo que tengan la extensión `.log` dentro del directorio `log/`.

**Cambiar el Nombre de los Archivos**
Al contrario que muchos sistemas VCS, Git no rastrea explícitamente los cambios de nombre en archivos. Si renombras un archivo en Git, no se guardará ningún metadato que indique que renombraste el archivo. Sin embargo, Git es bastante listo como para detectar estos cambios luego que los has hecho - más adelante, veremos cómo se detecta el cambio de nombre.

Por esto, resulta confuso que Git tenga un comando `mv`.
```bash
git mv file_from file_to
Es lo mismo que:
git rm README.md
git add README
```

**Historial de confirmaciones**
Después de haber hecho varias confirmaciones, o si has clonado un repositorio que ya tenía un histórico de confirmaciones, probablemente quieras mirar atrás para ver qué modificaciones se han llevado a cabo. La herramienta más básica y potente para hacer esto es el comando `git log`.
```bash
git clone https://github.com/schacon/simplegit-progit
git log
git log -p -2
git log --stat
```
Lista las confirmaciones hechas sobre ese repositorio en orden cronológico inverso. Es decir, las confirmaciones más recientes se muestran al principio. Como puedes ver, este comando lista cada confirmación con su suma de comprobación SHA-1, el nombre y dirección de correo del autor, la fecha y el mensaje de confirmación.

El comando `git log` proporciona gran cantidad de opciones para mostrarte exactamente lo que buscas. Aquí veremos algunas de las más usadas.
`-p` muestra las diferencias introducidas en cada confirmación.
`-2`  hace que se muestren únicamente las dos últimas entradas del historial.

Esto resulta muy útil para revisiones de código, o para visualizar rápidamente lo que ha pasado en las confirmaciones enviadas por un colaborador.

También puedes usar con `git log` una serie de opciones de resumen. Por ejemplo, si quieres ver algunas estadísticas de cada confirmación, puedes usar la opción `--stat`
Otra opción realmente útil es `--pretty`, que modifica el formato de la salida. Tienes unos cuantos estilos disponibles. La opción `oneline` imprime cada confirmación en una única línea, lo que puede resultar útil si estás analizando gran cantidad de confirmaciones. Otras opciones son `short`, `full` y `fuller`, que muestran la salida en un formato parecido, pero añadiendo menos o más información, respectivamente.
La opción más interesante es `format`, que te permite especificar tu propio formato. Esto resulta especialmente útil si estás generando una salida para que sea analizada por otro programa —como especificas el formato explícitamente, sabes que no cambiará en futuras actualizaciones de Git—
https://git-scm.com/book/es/v2/Fundamentos-de-Git-Ver-el-Historial-de-Confirmaciones#rpretty_format

Las opciones `oneline` y `format` son especialmente útiles combinadas con otra opción llamada `--graph`. Ésta añade un pequeño gráfico ASCII mostrando tu historial de ramificaciones y uniones

**Deshacer cosas**
Sacar del área de preparación: `git reset HEAD CONTRIBUTING.md`
El archivo `CONTRIBUTING.md` esta modificado y, nuevamente, no preparado.

Volver al estado en que estaba la última confirmación: `git checkout -- CONTRIBUTING.md`

Es importante entender que `git checkout -- [archivo]` es un comando peligroso. Cualquier cambio que le hayas hecho a ese archivo desaparecerá - acabas de sobreescribirlo con otro archivo. Nunca utilices este comando a menos que estés absolutamente seguro de que ya no quieres el archivo.

Para mantener los cambios que has hecho y a la vez deshacerte del archivo temporalmente, hablaremos sobre cómo esconder archivos (_stashing_, en inglés) y sobre ramas. 

**Trabajar con remotos**
Para poder colaborar en cualquier proyecto Git, necesitas saber cómo gestionar repositorios remotos. Los repositorios remotos son versiones de tu proyecto que están hospedadas en Internet o en cualquier otra red. Puedes tener varios de ellos, y en cada uno tendrás generalmente permisos de solo lectura o de lectura y escritura. Colaborar con otras personas implica gestionar estos repositorios remotos enviando y trayendo datos de ellos cada vez que necesites compartir tu trabajo. Gestionar repositorios remotos incluye saber cómo añadir un repositorio remoto, eliminar los remotos que ya no son válidos, gestionar varias ramas remotas, definir si deben rastrearse o no y más. En esta sección, trataremos algunas de estas habilidades de gestión de remotos.
```bash
git remote 
git remote -v (para ver las URL)
```

**Añadir repositorios Remotos**
```bash
git remote add 
```

**Traer y combinar remotos**
```bash
git fetch [remote-name]
git pull [remote-name]
```
El comando irá al proyecto remoto y se traerá todos los datos que aun no tienes de dicho remoto. Luego de hacer esto, tendrás referencias a todas las ramas del remoto, las cuales puedes combinar e inspeccionar cuando quieras.
Si clonas un repositorio, el comando de clonar automáticamente añade ese repositorio remoto con el nombre “origin”.
Por lo tanto, `git fetch origin` se trae todo el trabajo nuevo que ha sido enviado a ese servidor desde que lo clonaste (o desde la última vez que trajiste datos). Es importante destacar que el comando `git fetch` solo trae datos a tu repositorio local - ni lo combina automáticamente con tu trabajo ni modifica el trabajo que llevas hecho. La combinación con tu trabajo debes hacerla manualmente cuando estés listo.
Puedes usar el comando `git pull` para traer y combinar automáticamente la rama remota con tu rama actual. Es posible que este sea un flujo de trabajo mucho más cómodo y fácil para ti; y por defecto, el comando `git clone` le indica automáticamente a tu rama maestra local que rastree la rama maestra remota (o como se llame la rama por defecto) del servidor del que has clonado. Generalmente, al ejecutar `git pull` traerás datos del servidor del que clonaste originalmente y se intentará combinar automáticamente la información con el código en el que estás trabajando.

**Enviar a Tus Remotos**
Cuando tienes un proyecto que quieres compartir, debes enviarlo a un servidor. El comando para hacerlo es simple: `git push [nombre-remoto] [nombre-rama]`. Si quieres enviar tu rama `master` a tu servidor `origin` (recuerda, clonar un repositorio establece esos nombres automáticamente), entonces puedes ejecutar el siguiente comando y se enviarán todos los _commits_ que hayas hecho al servidor.

```bash
git push origin master
```
Este comando solo funciona si clonaste de un servidor sobre el que tienes permisos de escritura y si nadie más ha enviado datos por el medio. Si alguien más clona el mismo repositorio que tú y envía información antes que tú, tu envío será rechazado. Tendrás que traerte su trabajo y combinarlo con el tuyo antes de que puedas enviar datos al servidor.

**Inspeccionar un Remoto**
```bash
git remote show origin
```

El comando lista la URL del repositorio remoto y la información del rastreo de ramas. El comando te indica claramente que si estás en la rama maestra y ejecutas el comando `git pull`, automáticamente combinará la rama maestra remota con tu rama local, luego de haber traído toda la información de ella. También lista todas las referencias remotas de las que ha traído datos.

### Ramificaciones en Git
Cualquier sistema de control de versiones moderno tiene algún mecanismo para soportar el uso de ramas. Cuando hablamos de ramificaciones, significa que tú has tomado la rama principal de desarrollo (master) y a partir de ahí has continuado trabajando sin seguir la rama principal de desarrollo. En muchos sistemas de control de versiones este proceso es costoso, pues a menudo requiere crear una nueva copia del código, lo cual puede tomar mucho tiempo cuando se trata de proyectos grandes.

Algunas personas resaltan que uno de los puntos más fuertes de Git es su sistema de ramificaciones y lo cierto es que esto le hace resaltar sobre los otros sistemas de control de versiones. A diferencia de otros sistemas de control de versiones, Git promueve un ciclo de desarrollo donde las ramas se crean y se unen ramas entre sí, incluso varias veces en el mismo día. Entender y manejar esta opción te proporciona una poderosa y exclusiva herramienta que puede, literalmente, cambiar la forma en la que desarrollas.

**¿Qué es una rama?**
Para entender realmente cómo ramifica Git, previamente hemos de examinar la forma en que almacena sus datos.
Git no los almacena de forma incremental (guardando solo diferencias), sino que los almacena como una serie de instantáneas (copias puntuales de los archivos completos, tal y como se encuentran en ese momento).
En cada confirmación de cambios (commit), Git almacena una instantánea de tu trabajo preparado. Dicha instantánea contiene además unos metadatos con el autor y el mensaje explicativo, y uno o varios apuntadores a las confirmaciones (commit) que sean padres directos de esta (un padre en los casos de confirmación normal, y múltiples padres en los casos de estar confirmando una fusión (merge) de dos o más ramas).
La rama “master” en Git, no es una rama especial. Es como cualquier otra rama. La única razón por la cual aparece en casi todos los repositorios es porque es la que crea por defecto el comando `git init` y la gente no se molesta en cambiarle el nombre.

**Crear una Rama Nueva**
```bash
git branch testing
```
Y, ¿cómo sabe Git en qué rama estás en este momento? Pues…​, mediante un apuntador especial denominado HEAD. 
`git branch`solamente crea una nueva rama pero no salta a ella.

**Cambiar de Rama**
Para saltar de una rama a otra, tienes que utilizar el comando `git checkout`. Hagamos una prueba, saltando a la rama `testing` recién creada:
```bash
git checkout testing
git commit -a -m 'made a change'
git checkout master
```
Esto mueve el apuntador HEAD a la rama `testing`.
Este comando realiza dos acciones: Mueve el apuntador HEAD de nuevo a la rama `master`, y revierte los archivos de tu directorio de trabajo; dejándolos tal y como estaban en la última instantánea confirmada en dicha rama `master`. Esto supone que los cambios que hagas desde este momento en adelante, divergirán de la antigua versión del proyecto. Básicamente, lo que se está haciendo es rebobinar el trabajo que habías hecho temporalmente en la rama `testing`; de tal forma que puedas avanzar en otra dirección diferente.
Es importante destacar que cuando saltas a una rama en Git, los archivos de tu directorio de trabajo cambian. Si saltas a una rama antigua, tu directorio de trabajo retrocederá para verse como lo hacía la última vez que confirmaste un cambio en dicha rama. Si Git no puede hacer el cambio limpiamente, no te dejará saltar.
Tres comandos: `git branch`, `git checkout` , `git commit` 
```bash
git log --oneline --decorate --graph --all
```
Te mostrará el historial de tus confirmaciones, indicando dónde están los apuntadores de tus ramas y como ha divergido tu historial.

**Procedimientos Básicos para Ramificar y Fusionar**
Vamos a presentar un ejemplo simple de ramificar y de fusionar, con un flujo de trabajo que se podría presentar en la realidad. Imagina que sigues los siguientes pasos:

1. Trabajas en un sitio web.
2. Creas una rama para un nuevo tema sobre el que quieres trabajar.
3. Realizas algo de trabajo en esa rama.

En este momento, recibes una llamada avisándote de un problema crítico que has de resolver. Y sigues los siguientes pasos:

1. Vuelves a la rama de producción original.
2. Creas una nueva rama para el problema crítico y lo resuelves trabajando en ella.
3. Tras las pertinentes pruebas, fusionas (merge) esa rama y la envías (push) a la rama de producción.
4. Vuelves a la rama del tema en que andabas antes de la llamada y continúas tu trabajo.

**Procedimientos Básicos de Ramificación**

```bash
git checkout -b iss53
Switched to a new branch "iss53"
```
Esto es un atajo para `git branch`y `git checkout`
Trabajas en el sitio web y haces algunas confirmaciones de cambios (commits). Con ello avanzas la rama `iss53`, que es la que tienes activada (checked out) en este momento (es decir, a la que apunta HEAD)

Entonces, recibes una llamada avisándote de otro problema urgente en el sitio web y debes resolverlo inmediatamente. Al usar Git, no necesitas mezclar el nuevo problema con los cambios que ya habías realizado sobre el problema #53; ni tampoco perder tiempo revirtiendo esos cambios para poder trabajar sobre el contenido que está en producción. Basta con saltar de nuevo a la rama `master` y continuar trabajando a partir de allí.

Pero, antes de poder hacer eso, hemos de tomar en cuenta que si tenemos cambios aún no confirmados en el directorio de trabajo o en el área de preparación, Git no nos permitirá saltar a otra rama con la que podríamos tener conflictos. Lo mejor es tener siempre un estado de trabajo limpio y despejado antes de saltar entre ramas.

Borrar una rama:
```bash
git branch -d <nombre rama>
```

**Procedimientos Básicos de Fusión**
```bash
git checkout master
git merge iss53
Merge made by the 'recursive' strategy.
```

En este caso, el registro de desarrollo había divergido en un punto anterior. Debido a que la confirmación en la rama actual no es ancestro directo de la rama que pretendes fusionar, Git tiene cierto trabajo extra que hacer. Git realizará una fusión a tres bandas, utilizando las dos instantáneas apuntadas por el extremo de cada una de las ramas y por el ancestro común a ambas.
![[Pasted image 20251121205647.png]]

En lugar de simplemente avanzar el apuntador de la rama, Git crea una nueva instantánea (snapshot) resultante de la fusión a tres bandas; y crea automáticamente una nueva confirmación de cambios (commit) que apunta a ella. Nos referimos a este proceso como "fusión confirmada" y su particularidad es que tiene más de un padre.
**Vale la pena destacar el hecho de que es el propio Git quien determina automáticamente el mejor ancestro común para realizar la fusión**
Ahora que todo tu trabajo ya está fusionado con la rama principal, no tienes necesidad de la rama `iss53`. Por lo que puedes borrarla y cerrar manualmente el problema en el sistema de seguimiento de problemas de tu empresa.

```bash
git branch -d iss53
```

**Principales Conflictos que Pueden Surgir en las Fusiones**
En algunas ocasiones, los procesos de fusión no suelen ser fluidos. Si hay modificaciones dispares en una misma porción de un mismo archivo en las dos ramas distintas que pretendes fusionar, Git no será capaz de fusionarlas directamente. Por ejemplo, si en tu trabajo del problema #53 has modificado una misma porción que también ha sido modificada en el problema `hotfix`, verás un conflicto.
Git no crea automáticamente una nueva fusión confirmada (merge commit), sino que hace una pausa en el proceso, esperando a que tú resuelvas el conflicto. Para ver qué archivos permanecen sin fusionar en un determinado momento conflictivo de una fusión, puedes usar el comando `git status`
Todo aquello que sea conflictivo y no se haya podido resolver, se marca como "sin fusionar" (unmerged). Git añade a los archivos conflictivos unos marcadores especiales de resolución de conflictos que te guiarán cuando abras manualmente los archivos implicados y los edites para corregirlos. El archivo conflictivo contendrá algo como:
```bash
<<<<<<< HEAD:index.html
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> iss53:index.html
```
Tras resolver todos los bloques conflictivos, has de lanzar comandos `git add` para marcar cada archivo modificado. Marcar archivos como preparados (staged) indica a Git que sus conflictos han sido resueltos.
Si en lugar de resolver directamente prefieres utilizar una herramienta gráfica, puedes usar el comando `git mergetool`, el cual arrancará la correspondiente herramienta de visualización y te permitirá ir resolviendo conflictos con ella.
Tras salir de la herramienta de fusionado, Git preguntará si hemos resuelto todos los conflictos y la fusión ha sido satisfactoria. Si le indicas que así ha sido, Git marca como preparado (staged) el archivo que acabamos de modificar. En cualquier momento, puedes lanzar el comando `git status` para ver si ya has resuelto todos los conflictos.
Problemas mayores: https://git-scm.com/book/es/v2/Herramientas-de-Git-Fusi%c3%b3n-Avanzada#r_advanced_merging

**Gestión de Ramas**
El comando `git branch` tiene más funciones que las de crear y borrar ramas. Si lo lanzas sin parámetros, obtienes una lista de las ramas presentes en tu proyecto.
```bash
git branch (lista de ramas, la que lleve * es la que está activa)
git branch -v (última confirmación en cada rama)
git branch --merged (las ramas q no lleven delante * pueden ser eliminadas)
git branch --no-merged muestra ramas con trabajos sin fusionar
```
Las ramas que no hayan sido fusionadas nos dará problemas si intentamos borrarlas con `-d`, para ello debemos usar `-D`

#### Flujos de trabajo ramificados

**Ramas de Largo Recorrido**
Por la sencillez de la fusión a tres bandas de Git, el fusionar una rama a otra varias veces a lo largo del tiempo es fácil de hacer. Esto te posibilita tener varias ramas siempre abiertas, e irlas usando en diferentes etapas del ciclo de desarrollo; realizando fusiones frecuentes entre ellas.

Muchos desarrolladores que usan Git llevan un flujo de trabajo de esta naturaleza, manteniendo en la rama `master` únicamente el código totalmente estable (el código que ha sido o que va a ser liberado) y teniendo otras ramas paralelas denominadas `desarrollo` o `siguiente`, en las que trabajan y realizan pruebas. Estas ramas paralelas no suelen estar siempre en un estado estable; pero cada vez que sí lo están, pueden ser fusionadas con la rama `master`. También es habitual el incorporarle (pull) ramas puntuales cuando las completamos y estamos seguros de que no van a introducir errores.
![](presentacion/images/ramasLargaDuracion.png)

Este sistema de trabajo se puede ampliar para diversos grados de estabilidad. Algunos proyectos muy grandes suelen tener una rama denominada `propuestas` o `pu` (del inglés “proposed updates”, propuesta de actualización), donde suele estar todo aquello que es integrado desde otras ramas, pero que aún no está listo para ser incorporado a las ramas `siguiente` o `master`. La idea es mantener siempre diversas ramas en diversos grados de estabilidad; pero cuando alguna alcanza un estado más estable, la fusionamos con la rama inmediatamente superior a ella. Aunque no es obligatorio el trabajar con ramas de larga duración, realmente es práctico y útil, sobre todo en proyectos largos o complejos.

**Ramas Puntuales**
Las ramas puntuales, en cambio, son útiles en proyectos de cualquier tamaño. Una rama puntual es aquella rama de corta duración que abres para un tema o para una funcionalidad determinada. Esta técnica te posibilita realizar cambios de contexto rápidos y completos y, debido a que el trabajo está claramente separado en silos, con todos los cambios de cada tema en su propia rama, te será mucho más sencillo revisar el código y seguir su evolución. Puedes mantener los cambios ahí durante minutos, días o meses; y fusionarlos cuando realmente estén listos, sin importar el orden en el que fueron creados o en el que comenzaste a trabajar en ellos.
![](presentacion/images/ramasPuntuales.png)


Es importante recordar que, mientras estás haciendo todo esto, todas las ramas son completamente locales. Cuando ramificas y fusionas, todo se realiza en tu propio repositorio Git. No hay ningún tipo de comunicación con ningún servidor.

#### Ramas Remotas
Las ramas remotas son referencias al estado de las ramas en tus repositorios remotos. Son ramas locales que no puedes mover; se mueven automáticamente cuando estableces comunicaciones en la red. Las ramas remotas funcionan como marcadores, para recordarte en qué estado se encontraban tus repositorios remotos la última vez que conectaste con ellos.

Así como la rama “master” no tiene ningún significado especial en Git, tampoco lo tiene “origin”. “master” es un nombre muy usado solo porque es el nombre por defecto que Git le da a la rama inicial cuando ejecutas `git init`. De la misma manera, “origin” es el nombre por defecto que Git le da a un remoto cuando ejecutas `git clone`. Si en cambio ejecutases `git clone -o booyah`, tendrías una rama `booyah/master` como rama remota por defecto.

Si haces algún trabajo en tu rama `master` local, y al mismo tiempo, alguien más lleva (push) su trabajo al servidor `git.ourcompany.com`, actualizando la rama `master` de allí, te encontrarás con que ambos registros avanzan de forma diferente. Además, mientras no tengas contacto con el servidor, tu apuntador a tu rama `origin/master` no se moverá.
![[Pasted image 20251122051453.png]]
Para sincronizarte, puedes utilizar el comando `git fetch origin`. Este comando localiza en qué servidor está el origen (en este caso `git.ourcompany.com`), recupera cualquier dato presente allí que tú no tengas, y actualiza tu base de datos local, moviendo tu rama `origin/master` para que apunte a la posición más reciente.
![[Pasted image 20251122051637.png]]
Para ilustrar mejor el caso de tener múltiples servidores y cómo van las ramas remotas para esos proyectos remotos, supongamos que tienes otro servidor Git; utilizado por uno de tus equipos sprint, solamente para desarrollo. Este servidor se encuentra en `git.team1.ourcompany.com`. Puedes incluirlo como una nueva referencia remota a tu proyecto actual, mediante el comando `git remote add`
Puedes denominar `teamone` a este remoto al asignarle este nombre a la URL.![[Pasted image 20251122052024.png]]

**Publicar**
Cuando quieres compartir una rama con el resto del mundo, debes llevarla (push) a un remoto donde tengas permisos de escritura. Tus ramas locales no se sincronizan automáticamente con los remotos en los que escribes, sino que tienes que enviar (push) expresamente las ramas que desees compartir. De esta forma, puedes usar ramas privadas para el trabajo que no deseas compartir, llevando a un remoto tan solo aquellas partes que deseas aportar a los demás.

Si tienes una rama llamada main, con la que vas a trabajar en colaboración; puedes llevarla al remoto de la misma forma que llevaste tu primera rama. Con el comando `git push (remoto) (rama)`:
```bash
git push origin main
```

Si utilizas una dirección URL con HTTPS para enviar datos, el servidor Git te preguntará tu usuario y contraseña para autenticarte. Por defecto, te pedirá esta información a través del terminal, para determinar si estás autorizado a enviar datos.

Si no quieres escribir tu contraseña cada vez que haces un envío, puedes establecer un “cache de credenciales”. La manera más sencilla de hacerlo es estableciéndolo en memoria por unos minutos, lo que puedes lograr fácilmente al ejecutar `git config --global credential.helper cache`

Es importante destacar que cuando recuperas (fetch) nuevas ramas remotas, no obtienes automáticamente una copia local editable de las mismas. Para integrar (merge) esto en tu rama de trabajo actual, puedes usar el comando `git merge`.

**Hacer seguimiento a las Ramas**
Al activar (checkout) una rama local a partir de una rama remota, se crea automáticamente lo que podríamos denominar una “rama de seguimiento” (tracking branch). Las ramas de seguimiento son ramas locales que tienen una relación directa con alguna rama remota. Si estás en una rama de seguimiento y tecleas el comando `git pull`, Git sabe de cuál servidor recuperar (fetch) y fusionar (merge) datos.

Cuando clonas un repositorio, este suele crear automáticamente una rama `master` que hace seguimiento de `origin/master`. Sin embargo, puedes preparar otras ramas de seguimiento si deseas tener unas que sigan ramas de otros remotos o no seguir la rama `master`. El ejemplo más simple es el que acabas de ver al lanzar el comando `git checkout -b [rama] [nombreremoto]/[rama]`. Esta operación es tan común que git ofrece el parámetro `--track`:
```bash
git checkout --track origin/serverfix
```
Si quieres ver las ramas de seguimiento que tienes asignadas, puedes usar la opción `-vv` con `git branch`. Esto listará tus ramas locales con más información, incluyendo a qué sigue cada rama y si tu rama local está por delante, por detrás o ambas.
```bash
git branch -vv
  iss53     7e424c3 [origin/iss53: ahead 2] forgot the brackets
  master    1ae2a45 [origin/master] deploying index fix
* serverfix f8674d9 [teamone/server-fix-good: ahead 3, behind 1] this should do it
  testing   5ea463a trying something new
```
Aquí podemos ver que nuestra rama `iss53` sigue `origin/iss53` y está “ahead” (delante) por dos, es decir, que tenemos dos confirmaciones locales que no han sido enviadas al servidor. También podemos ver que nuestra rama `master` sigue a `origin/master` y está actualizada. Luego podemos ver que nuestra rama `serverfix` sigue la rama `server-fix-good` de nuestro servidor `teamone` y que está tres cambios por delante (ahead) y uno por detrás (behind), lo que significa que existe una confirmación en el servidor que no hemos fusionado y que tenemos tres confirmaciones locales que no hemos enviado. Por último, podemos ver que nuestra rama `testing` no sigue a ninguna rama remota.

Si quieres tener los cambios por delante y por detrás actualizados, debes traértelos (fetch) de cada servidor antes de ejecutar el comando. Puedes hacerlo de esta manera: `$ git fetch --all; git branch -vv`

**Traer y fusionar**
A pesar de que el comando `git fetch` trae todos los cambios que no tienes del servidor, este no modifica tu directorio de trabajo. Simplemente obtendrá los datos y dejará que tú mismo los fusiones. Sin embargo, existe un comando llamado `git pull`, el cuál básicamente hace `git fetch` seguido por `git merge` en la mayoría de los casos. Si tienes una rama de seguimiento configurada como vimos en la última sección, bien sea asignándola explícitamente o creándola mediante los comandos `clone` o `checkout`, `git pull` identificará a qué servidor y rama remota sigue tu rama actual, traerá los datos de dicho servidor e intentará fusionar dicha rama remota.

Normalmente es mejor usar los comandos `fetch` y `merge` de manera explícita pues la magia de `git pull` puede resultar confusa.

**Eliminar Ramas Remotas**
Imagina que ya has terminado con una rama remota, es decir, tanto tú como tus colaboradores habéis completado una determinada funcionalidad y la habéis incorporado (merge) a la rama `master` en el remoto (o donde quiera que tengáis la rama de código estable). Puedes borrar la rama remota utilizando la opción `--delete` de `git push`. Por ejemplo, si quieres borrar la rama `serverfix` del servidor, puedes utilizar:
```bash
git push origin --delete serverfix
```

##### Reorganizar el Trabajo Realizado (Rebase)
En Git tenemos dos formas de integrar cambios de una rama en otra: la fusión (merge) y la reorganización (rebase).

**Reorganización básica**

![[Pasted image 20251122055132.png]]
La manera más sencilla de integrar ramas, tal y como hemos visto, es el comando `git merge`. Realiza una fusión a tres bandas entre las dos últimas instantáneas de cada rama (C3 y C4) y el ancestro común a ambas (C2); creando una nueva instantánea (snapshot) y la correspondiente confirmación (commit).
![[Pasted image 20251122055406.png]]
Sin embargo, también hay otra forma de hacerlo: puedes capturar los cambios introducidos en C4 y reaplicarlos encima de C3. Esto es lo que en Git llamamos _reorganizar_ (_rebasing_, en inglés). Con el comando `git rebase`, puedes capturar todos los cambios confirmados en una rama y reaplicarlos sobre otra.
```bash
git checkout experiment
git rebase master
```
Haciendo que Git vaya al ancestro común de ambas ramas (donde estás actualmente y de donde quieres reorganizar), saque las diferencias introducidas por cada confirmación en la rama donde estás, guarde esas diferencias en archivos temporales, reinicie (reset) la rama actual hasta llevarla a la misma confirmación que la rama de donde quieres reorganizar, y finalmente, vuelva a aplicar ordenadamente los cambios.
![[Pasted image 20251122055558.png]]

En este momento, puedes volver a la rama `master` y hacer una fusión con avance rápido (fast-forward merge).
```bash
git checkout master
git merge experiment
```
Así, la instantánea apuntada por `C4'` es exactamente la misma apuntada por `C5` en el ejemplo de la fusión. No hay ninguna diferencia en el resultado final de la integración, pero el haberla hecho reorganizando nos deja un historial más claro. Si examinas el historial de una rama reorganizada, este aparece siempre como un historial lineal: como si todo el trabajo se hubiera realizado en series, aunque realmente se haya hecho en paralelo.

Cabe destacar que, la instantánea (snapshot) apuntada por la confirmación (commit) final, tanto si es producto de una reorganización (rebase) como si lo es de una fusión (merge), es exactamente la misma instantánea; lo único diferente es el historial. La reorganización vuelve a aplicar cambios de una rama de trabajo sobre otra rama, en el mismo orden en que fueron introducidos en la primera, mientras que la fusión combina entre sí los dos puntos finales de ambas ramas.

**Algunas Reorganizaciones Interesantes**
También puedes aplicar una reorganización (rebase) sobre otra cosa además de sobre la rama de reorganización. Has ramificado a una rama puntual (`server`) para añadir algunas funcionalidades al proyecto, y luego has confirmado los cambios. Después, vuelves a la rama original para hacer algunos cambios en la parte cliente (rama `client`), y confirmas también esos cambios. Por último, vuelves sobre la rama `server` y haces algunos cambios más.

**Los peligro de Reorganizar**
**Nunca reorganices confirmaciones de cambio (commits) que hayas enviado (push) a un repositorio público.**
Cuando reorganizas algo, estás abandonando las confirmaciones de cambio ya creadas y estás creando unas nuevas; que son similares, pero diferentes. Si envías (push) confirmaciones (commits) a alguna parte, y otros las recogen (pull) de allí; y después vas tú y las reescribes con `git rebase` y las vuelves a enviar (push); tus colaboradores tendrán que refusionar (re-merge) su trabajo y todo se volverá tremendamente complicado cuando intentes recoger (pull) su trabajo de vuelta sobre el tuyo.

Ahora, sobre si es mejor fusionar o reorganizar: verás que la respuesta no es tan sencilla. Git es una herramienta poderosa que te permite hacer muchas cosas con tu historial, y cada equipo y cada proyecto es diferente. Ahora que conoces cómo trabajan ambas herramientas, será cosa tuya decidir cuál de las dos es mejor para tu situación en particular.

Normalmente, la manera de sacar lo mejor de ambas es reorganizar tu trabajo local, que aún no has compartido, antes de enviarlo a algún lugar; pero nunca reorganizar nada que ya haya sido compartido.

### Git en entornos distribuidos

Git es de naturaleza distribuida. En los sistemas centralizados, cada desarrollador es un nodo de trabajo más o menos en igualdad con un repositorio central. En Git, sin embargo, cada desarrollador es potencialmente un nodo o un repositorio - es decir, cada desarrollador puede contribuir a otros repositorios y mantener un repositorio público en el cual otros pueden basar su trabajo y al cual pueden contribuir.

**Flujos de trabajo centralizado**
Esto significa que si dos desarrolladores clonan desde el punto central, y ambos hacen cambios, solo el primer desarrollador en subir sus cambios lo podrá hacer sin problemas. El segundo desarrollador debe fusionar el trabajo del primero antes de subir sus cambios, para no sobrescribir los cambios del primero.

**Flujo de Trabajo Administrador-Integración**
Debido a que Git permite tener múltiples repositorios remotos, es posible tener un flujo de trabajo donde cada desarrollador tenga acceso de escritura a su propio repositorio público y acceso de lectura a todos los demás. Este escenario a menudo incluye un repositorio canónico que representa el proyecto "oficial". Para contribuir a ese proyecto, creas tu propio clon público del proyecto y haces pull con tus cambios. Luego, puede enviar una solicitud al administrador del proyecto principal para que agregue los cambios. Entonces, el administrador agrega el repositorio como remoto, prueba los cambios localmente, los combina en su rama y los envía al repositorio.
1. El administrador del proyecto hace un push al repositorio público.
2. El contribuidor clona ese repositorio y realiza los cambios.
3. El contribuidor realiza un push con su copia pública del proyecto.
4. El contribuidor envía un correo electrónico al administrador pidiendo que haga pull de los cambios.
5. El administrador agrega el repositorio del contribuidor como remoto y fusiona ambos localmente.
6. El administrador realiza un push con la fusión del código al repositorio principal.

![](presentacion/images/integracion.png)

Este es un flujo de trabajo muy común con herramientas basadas en hubs como GitHub o GitLab, donde es fácil hacer un fork de un proyecto e introducir los cambios en este fork para que todos puedan verlos. Una de las principales ventajas de este enfoque es que el contribuidor puede continuar realizando cambios y el administrador principal del repositorio puede incorporar los cambios en cualquier momento. Los contribuidores no tienen que esperar a que el proyecto incorpore sus cambios; cada parte puede trabajar a su propio ritmo.

**Flujo de Trabajo Dictador-Tenientes**
Esta es una variante de un flujo de trabajo de múltiples repositorios. Generalmente es utilizado por grandes proyectos con cientos de colaboradores; Un ejemplo famoso es el kernel de Linux. Varios administradores de integración están a cargo de ciertas partes del repositorio. Se les llaman “tenientes”. Todos los tenientes tienen un gerente de integración conocido como el “dictador benévolo”. El repositorio del dictador benevolente sirve como el repositorio de referencia del cual todos los colaboradores necesitan realizar pull. El proceso funciona así:

1. Los desarrolladores trabajan en su propia rama especifica y fusionan su código en la rama `master`, la cual, es una copia de la rama del dictador.
2. Los tenientes fusionan el código de las ramas `master` de los desarrolladores en sus ramas `master` de tenientes.
3. El dictador fusiona la rama `master` de los tenientes a su rama `master` de dictador.
4. El dictador hace push del contenido de su rama `master` al repositorio para que otros fusionen los cambios a sus ramas.

![](presentacion/images/tenientes.png)

Este tipo de flujo de trabajo no es común, pero puede ser útil en proyectos muy grandes o en entornos altamente jerárquicos. Permite al líder del proyecto (el dictador) delegar gran parte del trabajo y recopilar grandes subconjuntos de código en múltiples puntos antes de integrarlos.

**Git en entornos distribuidos: contribuyendo a un proyecto**
La principal dificultad con la descripción de cómo contribuir a un proyecto es que hay una gran cantidad de variaciones sobre cómo se hace. Debido a que Git es muy flexible, las personas pueden trabajar juntas de muchas maneras, y es problemático describir cómo deberían contribuir: cada proyecto es un poco diferente. Algunas de las variables involucradas son conteo de contribuyentes activos, flujo de trabajo elegido, acceso de confirmación y posiblemente el método de contribución externa.

El proyecto Git proporciona un documento que presenta una serie de buenos consejos para crear compromisos a partir de los cuales enviar parches: puede leerlos en el código fuente de Git en el archivo `Documentation / SubmittingPatches`.
En primer lugar, no desea enviar ningún error de espacios en blanco. Git proporciona una manera fácil de verificar esto: antes de comprometerse, ejecute `git diff --check`, que identifica posibles errores de espacio en blanco y los enumera.

**Metodología TPP**

![](presentacion/images/TPP.png)

En esta sencilla metodología mantenemos sólo una rama activa, llámese _master_ o [_main_](https://github.com/github/renaming).

- Sacamos una rama para hacer un arreglo (_fix_) o una nueva funcionalidad (_feature_).
- Añadimos cambios (_commits_) a la rama.
- Creamos una petición de mezcla (_pull request_ o _merge request_).
- Se van haciendo revisiones, añadiendo nuevos cambios y pasando los tests.
- Cuando los cambios están revisados y los tests pasan, mezclamos los cambios (hacemos _merge_).
- De nuevo se vuelven a pasar los tests en _master_.
- Una vez que los tests pasan, desplegamos en producción.
- A continuación etiquetamos la versión (_tag_).

**TPP para sistemas online**
Para sistemas online no necesitamos mantener múltiples versiones; basta con mantener una única versión viva que es la que mantenemos en _master_.

Una variante muy relevante en este caso consiste en mantener una rama adicional estable, donde mezclamos sólo el código que pasa las pruebas. Esta rama se puede llamar _stable_, _prod_ o similar. Desplegaremos a producción siempre desde la rama estable.
![](presentacion/images/TPPonline.png)

En este caso las etiquetas o _tags_ son menos importantes e incluso se pueden omitir. Para desplegar simplemente hacemos un `git pull`, y si es necesario lanzamos un build.

**GitFlow**
Esta metodología se liberó en 2010 y tuvo bastante éxito en múltiples ámbitos. Combina

- ramas para cada funcionalidad o _feature braches_,
- ramas para cada versión o _release branches_,
- una rama _master_ para versiones definitivas,
- una rama _develop_ para desarrollo,
- y otra rama _hotfixes_ para arreglos rápidos.

![](presentacion/images/gitflow.png)

El esfuerzo para mantener tantísimas ramas sólo es posible para grandes organizaciones con un montón de recursos.

**GitHub Flow**
En esta metodología [propuesta por GitHub](https://docs.github.com/en/free-pro-team@latest/github/collaborating-with-issues-and-pull-requests/github-flow) se trabaja de forma similar: se crea rama, se añaden cambios, se revisan y se mezclan.

![](presentacion/images/githubflow.png)

Podemos ver en [la guía gráfica](https://docs.github.com/en/free-pro-team@latest/github/collaborating-with-issues-and-pull-requests/github-flow) la diferencia más radical con TPP: en GitHub Flow **se hacen los despliegues desde la rama**, antes de mezclar. Esto tiene el gran problema de que es posible que haya cambios en _main_ que no se van a desplegar, ya que no están en la rama; hay que asegurarse de hacer un `git pull origin main` antes de desplegar y mezclar.


#### Los entresijos internos de Git
En este capítulo trataremos sobre todo con los comandos de fontanería; comandos que te darán acceso a los entresijos internos de Git y que te ayudarán a comprender cómo y por qué hace Git lo que hace como lo hace. Muchos de estos comando no están pensados para ser utilizados manualmente desde la línea de comandos; sino más bien para ser utilizados como bloques de construcción para nuevas herramientas y scripts de usuario personalizados.

Cuando lanzas `git init` sobre una carpeta nueva o sobre una ya existente, Git crea la carpeta auxiliar `.git`; la carpeta donde se ubica prácticamente todo lo almacenado y manipulado por Git. Si deseas hacer una copia de seguridad de tu repositorio, con tan solo copiar esta carpeta a cualquier otro lugar ya tienes tu copia completa. Todo este capítulo se encarga de repasar el contenido en dicha carpeta. Tiene un aspecto como este:

```bash
ls -F1
HEAD
config*
description
hooks/
info/
objects/
refs/
```
Puede que veas algunos otros archivos en tu carpeta `.git`, pero este es el contenido de un repositorio recién creado tras ejecutar `git init`, -es la estructura por defecto. El archivo `description` se utiliza solo en el programa GitWeb; por lo que no necesitas preocuparte por él. El archivo `config` contiene las opciones de configuración específicas de este proyecto concreto, y la carpeta `info` guarda un archivo global de exclusión con los patrones a ignorar además de los presentes en el archivo `.gitignore`. La carpeta `hooks` contiene tus scripts, tanto de la parte cliente como de la parte servidor, tal y como se ha visto a detalle en el [Puntos de enganche en Git](https://git-scm.com/book/es/v2/ch00/r_git_hooks).

Esto nos deja con cuatro entradas importantes: los archivos `HEAD` e `index` (todavía por ser creado), y las carpetas `objects` y `refs`. Estos elementos forman el núcleo de Git. La carpeta `objects` guarda el contenido de tu base de datos, la carpeta `refs` guarda los apuntadores a las confirmaciones de cambios (commits), el archivo `HEAD` apunta a la rama que tengas activa (checked out) en este momento, y el archivo `index` es donde Git almacena la información sobre tu área de preparación (staging área). Vamos a mirar en detalle cada una de esas secciones, para ver cómo trabaja Git.

**Los objetos en Git**
Git es un sistema de archivo orientado a contenidos. Significa que el núcleo Git es un simple almacén de claves y valores. Cuando insertas cualquier tipo de contenido, él te devuelve una clave que puedes utilizar para recuperar de nuevo dicho contenido en cualquier momento. Para verlo en acción, puedes utilizar el comando de fontanería _hash-object_. Este comando coge ciertos datos, los guarda en la carpeta _.git._ y te devuelve la clave bajo la cual se han guardado. Para empezar, inicializa un nuevo repositorio Git y comprueba que la carpeta _objects_ está vacía.

```bash
echo 'test content' | git hash-object -w --stdin
d670460b4b4aece5915caf5c68d12f560a9fe3e4
```
La opción `-w` indica a `hash-object` que ha de guardar el objeto; de otro modo, el comando solo te respondería cual sería su clave. La opción `--stdin` indica al comando de leer desde la entrada estándar stdin; si no lo indicas, `hash-object` espera una ruta de archivo al final. La salida del comando es una suma de comprobación (checksum hash) de 40 caracteres. Este checksum hash SHA-1 es una suma de comprobación del contenido que estás guardando más una cabecera; cabecera sobre la que trataremos en breve. En estos momentos, ya puedes comprobar la forma en que Git ha guardado tus datos:
```bash
find .git/objects -type f
```
En principio, esta es la forma en que guarda Git los contenidos; como un archivo por cada pieza de contenido, nombrado con la suma de comprobación SHA-1 del contenido y su cabecera. La subcarpeta se nombra con los primeros 2 caracteres del SHA-1, y el archivo con los restantes 38 caracteres.

Puedes volver a recuperar los contenidos usando el comando `cat-file`. Este comando es algo así como una "navaja suiza" para inspeccionar objetos Git. Pasándole la opción `-p`, puedes indicar al comando `cat-file` que deduzca el tipo de contenido y te lo muestre adecuadamente:
```bash
git cat-file -p d670460b4b4aece5915caf5c68d12f560a9fe3e4
test content
```
Ahora que sabes cómo añadir contenido a Git y cómo recuperarlo de vuelta. Lo puedes hacer también con el propio contenido de los archivos. Por ejemplo, puedes realizar un control simple de versiones sobre un archivo. Para ello, crea un archivo nuevo y guarda su contenido en tu base de datos:
```bash
git hash-object -w README.md # nos devuelve el hash asociado a esa version 1
# Añadimos algo al README.md
git hash-object -w README.md # nos devuelve el hash asociado a esa versión 2
find .git/objects -type f
git cat-file -p (el primer hash que teníamos)83baae61804e65cc73a7201a7252750c76066a30 > README.md
# volvemos al contenido de la version 1
cat README.md 
```
Pero no es práctico esto de andar recordando la clave SHA-1 para cada versión de tu archivo; es más, realmente no estás guardando el nombre de tu archivo en el sistema, --solo su contenido--. Este tipo de objeto se denomina un blob (binary large object). Con la orden `cat-file -t` puedes comprobar el tipo de cualquier objeto almacenado en Git, sin mas que indicar su clave SHA-1'
```bash
git cat-file -t 1f7a7a472abf3dd9643fd615f6da379c4acb3e3a
```

**Objetos tipo árbol**
El siguiente tipo de objeto a revisar serán los árboles, que se encargan de resolver el problema de guardar un nombre de archivo, a la par que guardamos conjuntamente un grupo de archivos. Git guarda contenido de manera similar a un sistema de archivos UNIX, pero de forma algo más simple.
Todo el contenido se guarda como objetos árbol (tree) u objetos binarios (blob), correspondiendo los árboles a las entradas de carpetas; y correspondiendo los binarios, mas o menos, a los contenidos de los archivos (inodes). Un objeto tipo árbol tiene una o más entradas de tipo árbol, cada una de las cuales consta de un puntero SHA-1 a un objeto binario (blob) o a un subárbol, más sus correspondientes datos de modo, tipo y nombre de archivo. 
Puedes crear fácilmente tu propio árbol. Habitualmente Git suele crear un árbol a partir del estado de tu área de preparación (staging area) o índice, escribiendo un serie de objetos árbol desde él. Por tanto, para crear un objeto árbol, previamente has de crear un índice preparando algunos archivos para ser almacenados. Puedes utilizar el comando de "fontanería" `update-index` para crear un índice con una sola entrada, --la primera versión de tu archivo text.txt--. Este comando se utiliza para añadir artificialmente la versión anterior del archivo test.txt a una nueva área de preparación. Has de utilizar la opción `--add`, porque el archivo no existe aún en tu área de preparación (es más, ni siquiera tienes un área de preparación), y has de utilizar también la opción `--cacheinfo`, porque el archivo que estás añadiendo no está en tu carpeta, sino en tu base de datos. Para terminar, has de indicar el modo, la clave SHA-1 y el nombre de archivo:
```bash
git update-index --add --cacheinfo 100644 \
git write-tree # escribir en el área de presentación un objeto tipo arbol
git cat-file -t d8329fc1cc938780ffdd9f94e0d364e0ea74f579 # comprobar q es un arbol
```
En este caso, indicas un modo `100644`, el modo que denota un archivo normal. Otras opciones son `100755`, para un archivo ejecutable; o `120000`, para un enlace simbólico. Estos modos son como los modos de UNIX, pero mucho menos flexibles — solo estos tres modos son válidos para archivos (blobs) en Git; (aunque también se permiten otros modos para carpetas y submódulos) --.
Tras esto, puedes usar el comando `write-tree` para escribir el área de preparación a un objeto tipo árbol. Sin necesidad de la opción `-w`, solo llamando al comando `write-tree`, y si dicho árbol no existiera ya, se crea automáticamente un objeto tipo árbol a partir del estado del índice.
```bash
echo 'new file' > new.txt
git update-index test.txt
git update-index --add new.txt
```
El área de preparación contendrá ahora la nueva versión de test.txt, así como el nuevo archivo new.txt. 

**Objetos commit**
Un commit apunta a un tree y a sus padres.

```bash
# primer commit
echo "first commit" | git commit-tree d8329fc1cc93...
# fdf4fc3344e67ab068f836878b6c4951e3b15f3d

git cat-file -p fdf4fc33...
# tree d8329fc1cc93...
# author ...
# committer ...
# first commit

echo "second commit" | git commit-tree tree2 -p fdf4fc33...
#  cac0cab538b9...

echo "third commit" | git commit-tree tree3 -p cac0cab...
#  1a410efbd135...

git log --stat 1a410e
```

Cada commit es inmutable: su hash depende del tree, padres, autor, mensaje…Cambia cualquier detalle → cambia el hash → para Git es otro commit. La “historia” es un grafo de commits enlazados por los parent.

**Referencias: ramas, HEAD, remoto**
Una rama es solo un archivo de texto que contiene el SHA de un commit.

```bash
git update-ref refs/heads/main 1a410efbd13591db07496601ebc7a059dd55cfe9
git update-ref refs/heads/test cac0cab...
git log --oneline -graph -all

# remotos: ultima info que tienes de origin/main
cat .git/refs/remotes/origin/main 
```

Una rama no es una copia, ni un directorio. Es un puntero a un commit. Por eso crear ramas y moverlas es baratísimo.

 **HEAD: donde estás**

```bash
cat .git/HEAD # ref: refs/heads/master  git checkout test cat .git/HEAD # ref: refs/heads/test  
# también podemos manipularla 
git symbolic-ref HEAD refs/heads/master git symbolic-ref HEAD 
# refs/heads/master
```
 
Cuando hace `git commit` crea un commit con padre = SHA apuntado por HEAD y luego actualiza la ref a la que apunta HEAD (la rama). HEAD evita muchos sustos con `reset`, `checkout`, etc. HEAD te dice qué se va a mover cuando comites o resetees.

 **Almacenamiento: objetos y packfiles**
Inicialmente todo se guarda como objeto suelto. Pero cuando el repo crece, Git empaqueta objetos similares, guarda deltas entre versiones y agrupa todo en archivos .pack + .idx. Los repos grandes con histoias largas siguen siendo rápidos porque el almacenamiento está comprimido.

`git gc` es el limpiador y compactador interno de Git. Cuando los repos se hacen grandes, los mete en un packfile que contiene duplicados, blobs comprimidos, árboles comprimidos y objetos almacenados como deltas

 **Refspecs** y remotos: cómo decide Git qué traer/enviar
La refspec define cómo mapear refs remotas. En `.git/config`:

```bash
[remote "origin"]
url = https://github.com/usuario/proyecto.git
fetch = +refs/heads/*:refs/remotes/origin/*   
```

Trae todas las ramas del remoto y las guarda localmente en esa ruta.  

 **Variables de entorno**
Algunas variables afectan directamente cómo opera Git:

- `GIT_DIR`
- `GIT_WORK_TREE` ruta del working directory
- `GIT_AUTHOR_NAME, GIT_AUTHOR_EMAIL, GIT_AUTHOR_DATE`
- `GIT_COMMITTER_NAME, GIT_COMMITTER_EMAIL, GIT_COMMITTER_DATE`
- `GIT_TRACE, GIT_TRACE_PERFORMANCE` logs de depuración y rendimiento  

 **Cómo Git opera en esencia**
Todo lo que hace Git se puede reducir a:

1. Crear objetos (blob, tree, commit, tag) inmutables en .git/objects.
2. Mover referencias (refs/heads/**, refs/tags/**, HEAD) para apuntar a esos objetos.
3. Comprimir y optimizar esos objetos (gc, packs).
4. Enviar y recibir objetos + refs con otros repos (fetch, push, refspecs).

**Los comandos de porcelana serán combinaciones de todas estas operaciones básicas.**


