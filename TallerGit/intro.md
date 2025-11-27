# Git

- Sistema de control de versiones diseñado para ir a toda pastilla.
	- Se queda corto con ficheros grandes y binarios.
- Focalizado en archivos pequeños y en ser rápido.
	- Experiencia de usuario de [[Bitkeeper]].
	- Features de [[Mercurial HG]].
- Cada desarrollador tiene una copia.
- Desarrollado por [[Linus Torvalds]] cuando [[Bitkeeper]] se privatizó.

```
“Only wimps use tape backup. REAL men just upload their important stuff on ftp and let the rest of the world mirror it.”  

― Linus Torvalds
```

**Beneficios/Motivaciones de Git**
**Conceptualización de Git**
**Buenas prácticas**

![[Git.excalidraw]]
## Trabajo por Ramas

Van cambiando del lugar y del caso de uso.
- Todos los repos tienen un repo `main` o `master`.
- Un producto suele tener una rama `stable` o `prod` que es la que se monta y distribuye.
- Puede haber una rama `pre` o `qa` que despliega a un entorno de pruebas.
- En [[SaaS - Software as a Service]] lo suyo es que siempre está desplegada una sola versión del repo, generalmente `main` o `master`.

El trabajo por ramas generalmente se hace bifurcando la rama principal o `master` a una rama `feature A` donde se submitean los cambios hasta que llega la hora de hacer el *merge*.

*¿Como se hace un merge?* Hay varias estratégias
- Hacemos un *squash* de todos los cambios y lo metemos en `master`.
	- Se destruye toda la información e historia de la rama.
- *merge-commit*
	- Se mantiene el historial de la rama.
- *rebases*
	- Se trae los cambios sobre la última versión de una branch concreta.

```bash
git log --graph --oneline --all
```

## Las tripas

Todas estas abstracciones son mentira, en realidad son todo archivos dentro de `.git`.

```
git pm "commit message"
```
- Este `-p` es muy util porque hace un commit interactivo en el que debes de comprobar cada línea.
-
Puedes hacer `git pull` a directorios dentro del sistema, incluso a otros sistemas por medio de SSH.

Al resolver conflictos `HEAD` es siempre el repo local.

`git pull -X` con `-X mine` solo acepta mis cambios y con `-X theirs` solo acepta los del remoto. 

`git reset` por defecto es soft. Pero aunque usemos el `git reset --hard` los cambios siguen en el `.git` y los podemos recuperar con `git cherry-pick` para recuperar cada uno de ellos.

Se puede configurar con `git config --global push.autoSetupRemote true` para que te genere automáticamente las ramas en el remote.

`git fetch` te trae los cambios del repositorio remoto pero no te los aplica, solo actualiza la información en `.git`.

`git checkout -b feature/rama-bonita` te crea otra rama sobre la que estás trabajando.

`git checkout .-` te trae a la última rama.

`git reflog` para poder ver todas las referencias por las que ha pasado el puntero `HEAD`. A los que se puede acceder con `git checkout <commitid>`.
