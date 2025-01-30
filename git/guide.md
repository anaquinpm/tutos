# Guía básica de instalación y configuración de Git

- Author: Pablo Martín Anaquín (@anaquinpm)
- Date: 15-03-2021

## Instalamos git en Linux

```bash
# Actualizamos el repositorio e instalamos git
sudo apt update && sudo apt install git -y

# Verificamos la versión que se instalo de git
git --version
```

## Configuramos usr, mail y editores

```bash
#Configuramos nuestro nombre de usuario, e-mail y editor principal.
git config --global -l
git config --global user.name="devops8020"
git config --global user.email="devops@test.com"
git config --global core.editor=vim
git config --global -l
```

Las configuraciones quedan almacenas de nuestor carpeta home del sistema en el archivo `~/.gitconfig`
> cat ~/.gitconfig

## Iniciamos git en el directorio o lo clonamos

```bash
git init
git status
```

## Definimos estadios de los archivos: history, stage, working directory

Tenemos 3 estados de los archivos/carpetas:

- Working directory: es el directorio actual que vemos al trabajar
- Stage (index): es un estadio donde colocamos cambios que ya tenemos probados y queremos seguir avanzando
- History: Es cuando almacenamos los archivos/directorios del "stage" en nuestro repositorio local (en nuestra PC)

## Creamos una lista de "verduras.txt" con diferentes estadios del archivo (status,add,commit)

Cada vez que agregamos archivos al "history" se crean archivos únicos en el directorio ".git" los cuales no debemos alterar/borrar. El indice de nuestro repositorio se genera en funcíon de los HASH generados al relaizar una modificación en nuestro repositorio.

```bash
vim verduras.txt      # Agregamos una verdura
git status
git add verduras.txt  # Agregamos al stage la lista verduras
git status            # cambio el stado del archivo
echo unaVerdura >> verduras.txt
# Los commits van a seguir el progreso (crear, editar o borrar archivos) en nuestro trabajo.
# La descripción del commit es importante para poder volver por si necesitamos corregir cambios.
git commit -m "feat: creacion de una lista de compras para las verduras"
git status
echo otraVerdura >> verduras.txt
git status
git add .             # agrega todos los archivos del WD y elimina los marcados para borrar
git status
cat verduras.txt
echo otraVerdura >> verduras.txt
git status            # En este puntos podes ver los 3 estados de un archivo
```

## Utilizamos "diff" comparando los diferentes estadios del archivo

```bash
git diff              # Diferencia entre el WD y el stage
git diff HEAD         # Diferencia entre el WD y el ultimo commit (en el history)
git diff --staged     # Diferencia entre el stage y el ultimo commit (en el history)
```

## Volver a la versión de Stage o History

```bash
cat verduras.txt              # ver la lista de las 3 verduras
git checkout -- verduras.txt  # traemos el arch desde el "stage" al "WD"
cat verduras.txt              # vemos solo dos verduras
git reset -- verduras.txt     # traemos el arch desde el "History" al "stage" - <mode> default is "mixed"

# Vemos las diferencia como vimos anteriormente
git diff
git diff HEAD
git diff --staged

# Traemos del último commit el arch y lo remplazamos en el "stage" y "wd" en un solo comanmdo
git reset --hard                  # Pasa el commit donde esta el HEAD en el history, al "stage" y "WD"
    $ git checkout HEAD -- verduras.txt      # Idem al comando anterior -> "HEAD" referencia a la rama actual
# Agregar una verdura a lista en el "stage" y el otra al "WD"
```

## Creamos más commits agregando las verduras a la lista

```bash
git commit -m "chore:agregamos una segunda verdura a la lista"
git commit -am "chore: agregamos una tercera verdura a la lisdta"    # agregamos la verdura al stage y al history
git log
git log --oneline --graph --decorate --all        # muestro de manera más resumida y amigable los commits
```

## Creamos un nueva rama para frutas (con branch o checkout -b). Cambiar de rama en rama

Crear una branch tiene el sentido de trabajar de forma segura sin afectar a la rama `Main` y sus nombres deberían indicar cual es el destino de crear esa rama para que los compañeros entienda que estamos haciendo en ella.

Ejemplos de nombres de rama: refactor-login, change-view-side-bar

```bash
git branch frutas             # Crea rama de frutas
git checkout frutas           # Cambiar a la rama frutas
  $ git checkout -b frutas    # crea la rama frutas y se cambia a esa rama automaticamente (remlaza las dos lineas anteriores)
vim frutas.txt
git add frutas.txt
git commit -m "feat: cración de la lista de frutas"       # creamos un nuevo commit en el history de nuestro repositorio.
echo banana >> frutas.txt
git commit -am "chore: agregamos una segunda fruta la lista de frutas"    # podemos usar el -am porque el archivo frutas.txt ya tiene seguimiento
git diff
git diff HEAD
git log --oneline --graph --decorate --all        # muestro de manera más resumida y amigable los commits
echo pimiento >> verduras.txt
git commit -am "chore: agregamos una cuarta verdura a la lista de verduras"
```

## Modificamos la lista de verduras en la rama "master"

```bash
git checkout master       # cambio a la rama de verduras para hacer un commit nuevo en esa rama y que se muestre mejor en el log
cat verduras.txt       # vemos que es diferente a la lista que dejamos en la rama frutas
echo cebollas >> verduras.txt
git commit -am "chore: agregue una 4° verdura a la lista de verdura en la rama master"
git log --oneline --graph --decorate --all        # Vemos graficamente como evoluciona el repositorio en sus diferentes ramas
```

## Realizamos un merge de las dos ramas

```bash
# Para realizar el merge tenemos que estar en la rama a la que vamos a unir las modificaciones que relizamos en otra rama
git merge frutas      # Nos va indicar los archivos con conflictos
vim arch_conflictos   # tenemos que modificarlos y guardarlos para que pueda continuar el merge
git add .             # agregamos los arreglos para el merge
git merge --continue  # continueamos con el merge que tenía conflictos
```

## Crear cuenta en gihub.com y un repositorio donde subir este ejercicio

```bash
git remote -v
git remote add origin git@github.com:anaquinpm/git-full-course-devops8020.git
git branch -M main            # Cambiamos el nombre de la rama "master" por "main" con todo su historial.
git push -u origin main
```

## Clonar un repositorio

```bash
git clone https://github.com/anaquinpm/config.git carpeta-destino
cd carpeta-destino
git log  --oneline --graph --decorate --all
```
