--------------------------------------------------------------------
CONFIG
--------------------------------------------------------------------

git config -l lista todas las configuraciones


git config --global user.name ahierro
git config --global user.email
git config --global color.ui true
git config --global difftool.prompt true

superlog:
git config --global alias.superlog "log --graph --abbrev-commit --decorate --date=relative --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all"

git config --global alias.lol "log --graph --oneline --decorate"



Editor de commits:
En Windows
git config --global core.editor "'C:\Users\Dell\AppData\Local\atom\bin\atom.cmd'  --wait"
En Linux
git config --global core.editor"atom --wait"


Editor de difftool/mergetool:

git config --global mergetool.keepBackup false
git config --global diff.tool meld 
git config --global difftool.prompt false
git config --global merge.tool meld  
git config --global mergetool.prompt false
For Windows. Run these commands in Git Bash:
git config --global difftool.meld.path "C:\Program Files (x86)\Meld\Meld.exe" 

git config --global mergetool.meld.path "C:\Program Files (x86)\Meld\Meld.exe" 
(Update the file path for Meld.exe if yours is different.)
For Linux. Run these commands in Git Bash:
git config --global difftool.meld.path "/usr/bin/meld" 
git config --global mergetool.meld.path "/usr/bin/meld" git config --global mergetool.prompt false
You can verify Meld's path using this command:
which meld


--------------------------------------------------------------------
OPERATORIA BÁSICA
--------------------------------------------------------------------



git status: muestra qué archivos están en staging area / working dir
git init: nos crea un repositorio de manera local y lo hará en la carpeta donde estamos posicionados o se le puede pasar [nombre_de_la_carpeta]y creará la carpeta con ese nombre.
git add [archivo]: Nos agrega al archivo al staging area(el limbo)
-A nos agrega todos los archivos
git rm --cached: Saca el archivo del staging
git rm -f [file]: Saca del staging y elimina el archivo por completo.
git commit -m “mensaje”: comitea con un mensaje
–amend: concatena nuevos cambios con cambios previos.


--------------------------------------------------------------------
TAGS
--------------------------------------------------------------------
git tag: nos permite agregar etiquetas a nuestros cambios.
-a para la anotación
-m para el mensaje
-l nos muestra la lista de etiquetas
-f para renombrar
-d para borrar
anotated 
git tag -a 0.5 -m 'version estable'
lightwieght
git tag 0.5 (donde estoy parado)
gir tag 0.5 sha
git tag -d 0.5

To push a single tag:
git push origin <tag_name>
And the following command should push all tags (not recommended):
git push --tags
Delete remote tag:
git push --delete origin tagname


--------------------------------------------------------------------
REBASE
--------------------------------------------------------------------

Para traerme los cambios del branch develop a mi featureBranch (el cual solamente modifico yo) Primero voy a develop y me actualizo con git pull y luego
git rebase --onto nuevaBase viejaBase
git rebase --onto develop $(git merge-base develop featureBranch)
git rebase develop 

SQUASH COMMITS

Squash los últimos 3 commits
git rebase -i HEAD~3

te muestra la lista de commits ordenado desde el mas viejo arriba hasta el mas nuevo abajo, si se pone squash o fixup sobre un commit se une al commit de arriba. Para editar en vim, esc, y para guardae esc :wq, para salir sin guardar :qa! para cancelar git rebase --abort

--------------------------------------------------------------------
LOGS
--------------------------------------------------------------------
git log shows the current HEAD and its ancestry. That is, it prints the commit HEAD points to, then its parent, its parent, and so on. It traverses back through the repo's ancestry, by recursively looking up each commit's parent.
--oneline
–graph
-[n]: Nos permite ver los últimos n commit

git reflog show: The reflog is an ordered list of the commits that HEAD has pointed to: it's undo history for your repo. 
--------------------------------------------------------------------
BLAME
--------------------------------------------------------------------
git blame file
	-L 1,2 (desde la linea 1 hasta la 2)
--------------------------------------------------------------------
HISTORY
--------------------------------------------------------------------
gitk --follow --all -p file

--------------------------------------------------------------------
DIFFS
--------------------------------------------------------------------
git difftool branch1..branch2 Abre con la tool default configurada
git difftool -t kompare branch1..branch2 Opción -t para indicarle otra tool

Quiero ver que cambios hace mi featurebranch desde que se creó el commit
git difftool develop...featurebranch 
git difftool featureBranch $(git merge-base develop featureBranch)

git diff SHA1: Nos muestra las cambios de ese commit.
git diff [XXX-1] [XXX-2]: Muestra las diferencias del commit [XXX-1] contra el commit [XXX-2]. Si [XXX-1] es hijo de [XXX-2] muestra los nuevos cambios a la izquierda del meld, si es al reves, al reves.
git diff [branchA]...[branchB]: Muestra las diferencias que introduce el branchB desde que se bifurcó del branchA.

--------------------------------------------------------------------
RESET
--------------------------------------------------------------------
git reset --soft SHA1: Resetea un commit pero mantiene sus cambios en el staging area
git reset --mixed SHA1: Elimina los cambios pero mantiene sus cambios en el working directory. Esta es la default.
git reset --hard SHA1: Apunta el HEAD al commit SHA1 y se pierden todos los cambios.

git reset --hard HEAD@{1} Mueve el puntero HEAD a donde apuntaba 1 commit antes. Tener en cuenta que rebase mueve automáticamente el HEAD por varios commits, ver git reflog
git reset --hard HEAD^ Mueve el puntero HEAD al commit padre
git reset file Saca el archivo del stage
--------------------------------------------------------------------
REVERT
--------------------------------------------------------------------
git revert sha1 Agrega un commit con los cambios opuestos al commit
	--no-commit

--------------------------------------------------------------------
CHERRY PICK
--------------------------------------------------------------------

git cherry-pick sha1 Copia el commit y lo comitea a continuación
--no-commit

--------------------------------------------------------------------
BRANCHS
--------------------------------------------------------------------

git branch [name-branch] Crea una nueva rama.
git branch -l Lista todas las ramas locales.
git branch -a Lista todas las ramas locales y remotas.
git branch -d [name-branch] Elimina una rama. Con -D se fuerza el borrado.
git branch -m [old-name-branch] [new-name-branch] Permite renombrar una nueva rama.
git checkout -b [name-branch]: Crea la rama y te mueve hacia esa rama
git checkout [name-branch] Te mueve hacia esa rama. 

--------------------------------------------------------------------
SHA1 donde apunta el branch
--------------------------------------------------------------------
cat .git/refs/heads/master
--------------------------------------------------------------------
SHA1 donde apunta el HEAD
--------------------------------------------------------------------
cat .git/HEAD


--------------------------------------------------------------------
DISCARD CHANGES
--------------------------------------------------------------------
git checkout . Deja los archivos nuevos y lo que está en staged, descarta todo el resto
git checkout file Descarta todos los cambios en el archivo
git checkout -p Descartar hunks que te permite seleccionar
--------------------------------------------------------------------
STASH
--------------------------------------------------------------------
git stash: guarda los archivos de working directory y staged
git stash list: nos muestra la lista de stash que tengamos.
git stash drop stash@{numero}: nos permite borrar un stash.
git stash apply: aplica el último stash, no borra el stash
git stash pop: aplica el último stash y lo borra
git stash branch testchanges: si tira conflicto al aplicar el stash, este comando sirve para crear un branch a partir de donde fue creado el stash y aplica el stash sobre ese branch

--------------------------------------------------------------------
CLONE
--------------------------------------------------------------------
git clone http_url: te pide usuario y pass
git clone ssh_url: Tiene que estar configurada la clave ssh

--------------------------------------------------------------------
Crear Llave SSH
--------------------------------------------------------------------
Crear llave
ssh-keygen -t rsa -b 4096 -C "ahierrodev@gmail.com"
copiar llave
cat ~/.ssh/id_rsa.pub
copiar en ssh de github

--------------------------------------------------------------------
REMOTES
--------------------------------------------------------------------
git remote add [origin] [SSH/HTTPS] Conecta un repositorio con nuestro equipo local.
git remote -v Lista las conexiones existentes.
git remote remove [origin] Elimina una conexión con algún repositorio.

--------------------------------------------------------------------
TRAER CAMBIOS
--------------------------------------------------------------------
Opción fetch + merge

git fetch origin
git merge origin/master


Opción pull
git pull origin master
--rebase
--autostash
Before starting rebase, stash local modifications away (see git-stash[1]) if needed, and apply the stash entry when done. --no-autostash is useful to override the rebase.autoStash configuration variable (see git-config[1]).
This option is only valid when "--rebase" is used.


--------------------------------------------------------------------
SUBIR CAMBIOS
--------------------------------------------------------------------
git push origin master
--force: si reescribo la historia
-u: upstream para despues poder usar pull sin argumentos

--------------------------------------------------------------------
MERGE
--------------------------------------------------------------------
git checkout merge
git merge featureBranch --no-ff

git merge --abort
git merge --continue
--allow-unrelated-histories
By default, git merge command refuses to merge histories that do not share a common ancestor. This option can be used to override this safety when merging histories of two projects that started their lives independently. As that is a very rare occasion, no configuration variable to enable this by default exists and will not be added.
--no-ff
Create a merge commit even when the merge resolves as a fast-forward. This is the default behaviour when merging an annotated (and possibly signed) tag.

Si tira conflicto ejecutar el comando
git mergetool
luego de resolver los conflictos si no tenías configurado esto
git config --global mergetool.keepBackup false entonces ejecutar 
git clean -i
git commit



--------------------------------------------------------------------
PATCHES
--------------------------------------------------------------------
Generar el patch
git format-patch -1 sha1

Aplicar el patch
git am [patchfile]

--------------------------------------------------------------------
GITKRAKEN
--------------------------------------------------------------------

Reinstalar GitKraken en debian

sudo dpkg --remove gitkraken

sudo dpkg -i gitkraken-amd64.deb


