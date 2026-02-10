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
git diff --staged | git diff --cached HEAD    # Diferencia entre el stage y el ultimo commit (en el history)
```

## Volver a la versión de Stage o History

```bash
cat verduras.txt              # ver la lista de las 3 verduras
git checkout -- verduras.txt / git restore verduras.txt  # traemos el arch desde el "stage" al "WD"
cat verduras.txt              # vemos solo dos verduras
git reset -- verduras.txt / git restore --staged verduras.txt     # traemos el arch desde el "History" al "stage" - <mode> default is "mixed"

# Vemos las diferencia como vimos anteriormente
git diff
git diff HEAD
git diff --staged

# Traemos del último commit el arch y lo remplazamos en el "stage" y "wd" en un solo comanmdo
git reset --hard                  # Pasa el commit donde esta el HEAD en el history, al "stage" y "WD"
    $ git checkout HEAD -- verduras.txt / git restore --source=HEAD verduras.txt      # Idem al comando anterior -> "HEAD" referencia a la rama actual
# Agregar una verdura a lista en el "stage" y el otra al "WD"
```

## Creamos más commits agregando las verduras a la lista

```bash
git commit -m "chore:agregamos una segunda verdura a la lista"
git commit -am "chore: agregamos una tercera verdura a la lisdta"    # agregamos la verdura al stage y al history 
git log
git log --oneline --graph --decorate --all        # muestro de manera más resumida y amigable los commits
```

## Pausar cambios con stash (cambio de contexto sin commitear)

# 1) Estamos en la rama actual (por ejemplo main) y modificamos verduras.txt
echo "tomate" >> verduras.txt
git status

# 2) Guardamos el trabajo "a medio hacer" y volvemos a un working directory limpio
# (git stash sin argumentos equivale a git stash push)
git stash push -m "WIP: agregando tomate a verduras (pausa para pasar a frutas)"

# 3) Verificamos que el working directory quedó limpio
git status

# 4) Vemos los stashes guardados
git stash list

# 5) Opcional: inspeccionamos qué guardamos
git stash show stash@{0}


## Creamos un nueva rama para frutas (con branch o checkout -b). Cambiar de rama en rama

Crear una branch tiene el sentido de trabajar de forma segura sin afectar a la rama `Main` y sus nombres deberían indicar cual es el destino de crear esa rama para que los compañeros entienda que estamos haciendo en ella.

Ejemplos de nombres de rama: refactor-login, change-view-side-bar

```bash
git branch frutas             # Crea rama de frutas
    git branch [ --list <pattern> ] # buscar un patron en el nombre de la branch
git checkout frutas / git switch frutas           # Cambiar a la rama frutas
  $ git checkout -b frutas /hit switch -c frutas   # crea la rama frutas y se cambia a esa rama automaticamente (remlaza las dos lineas anteriores)
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


# Volvemos a la rama donde estábamos con verduras
git checkout main

# Recuperamos lo que habíamos pausado
# Opción A: aplicar y mantener el stash (por si querés reusarlo)
git stash apply stash@{0}

# Opción B: aplicar y eliminarlo del stack (lo más común al retomar)
git stash pop
```

### Stash to Branch

```bash
# Si lo que stasheaste era grande, lo convertís en una rama nueva directamente
git stash branch wip-verduras stash@{0}
```


## Modificamos la lista de verduras en la rama "main"

```bash
git checkout main       # cambio a la rama de verduras para hacer un commit nuevo en esa rama y que se muestre mejor en el log
cat verduras.txt       # vemos que es diferente a la lista que dejamos en la rama frutas
echo cebollas >> verduras.txt
git commit -am "chore: agregue una 4° verdura a la lista de verdura en la rama main"
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
    git remote set-url origin  git@github.com:user_name/repository.git    # cambiar remote URLs from https to ssh
git branch -M main            # Cambiamos el nombre de la rama "master" por "main" con todo su historial.
git push -u origin main       # pusheamos la rama 'main' al repositorio y si no existe la crea.
```

# Tags

```bash
git tag <tag_name>              # agrega un "lightweight tag" al commit actual
    git tag <tag_name> <commit>     # agrega un [tag_name] al [commit] indicado
    git tag -a v1.4 -m "mi version v1.4" [id_commit]  # Indicamos un "annotated tag" - El id_commit es opcional al querer etiquetar una commit pasado
git show v1.4                   # muestra detalles del commit con ese tag
git tag                         # Muestra los tag disponibles en el repositorio
git tag -d <tag_name>           # borra el tag del commit actual
git push origin --tags
```

## Clonar un repositorio

```bash
git clone https://github.com/anaquinpm/config.git carpeta-destino
cd carpeta-destino
git log  --oneline --graph --decorate --all
```

# Git Workflows (Flujos de trabajo)

Esta sección complementa el tutorial introduciendo los **workflows de Git** más utilizados en equipos modernos, alineados con prácticas DevOps y CI/CD.

---

## ¿Qué es un workflow de Git?

Un workflow define **cómo un equipo usa Git**:

- Cómo se crean las ramas
- Cómo se integran los cambios
- Cómo y cuándo se libera el código

Elegir el workflow correcto **reduce conflictos**, mejora la colaboración y acelera las entregas.

Los workflows más usados hoy son:

- Git Flow
- GitHub Flow
- Trunk-Based Development

---

## Git Flow

Git Flow es un workflow **estructurado**, pensado para proyectos con **versiones y releases programados**.

### Ramas principales

- **main**  
  Contiene código **listo para producción**

- **develop**  
  Rama de integración donde se unen las features antes del release

### Ramas de soporte

- **feature/***: desarrollo de nuevas funcionalidades
- **release/***: preparación de una versión
- **hotfix/***: arreglos urgentes en producción

### Flujo general

1. `develop` nace desde `main`
2. Cada feature se desarrolla en una rama `feature/*`
3. Las features se mergean a `develop`
4. Se crea una rama `release/*` para estabilizar
5. `release` se mergea a `main` y a `develop`
6. Los hotfix nacen desde `main`

### ¿Cuándo usar Git Flow?

- Proyectos grandes
- Releases versionados (v1.2.0, v2.0.0)
- Equipos numerosos

No es ideal para despliegue continuo.

---

## GitHub Flow

GitHub Flow es un workflow **simple y liviano**, ideal para **CI/CD y despliegue continuo**.

### Ramas

- **main**: siempre desplegable
- **feature branches**: ramas cortas por cambio

### Flujo general

1. Crear una rama desde `main`
2. Hacer commits pequeños y descriptivos
3. Abrir un Pull Request (PR)
4. Ejecutar tests y validaciones (CI)
5. Mergear a `main`
6. Desplegar inmediatamente

No existe rama `develop` ni `release`.

### ¿Cuándo usar GitHub Flow?

- Proyectos web
- Startups
- Equipos chicos o medianos
- Deploy frecuente

---

## Trunk-Based Development

Trunk-Based Development es el workflow **más moderno**, usado por equipos de alto rendimiento.

### Concepto clave

Todos los desarrolladores integran cambios **rápido y seguido** a una única rama principal llamada *trunk* (`main`).

### Características

- Ramas muy cortas (horas o 1–2 días)
- Integraciones frecuentes
- Uso de *feature flags*
- Fuerte automatización (CI, tests)

### Flujo general

1. Crear una rama corta desde `main`
2. Hacer cambios pequeños
3. Mergear rápidamente a `main`
4. Deploy continuo

### ¿Cuándo usar Trunk-Based?

- Equipos maduros
- CI/CD fuerte
- Microservicios
- Alta velocidad de desarrollo

---

## Comparación rápida de workflows

- **Git Flow**: control y estructura
- **GitHub Flow**: simple y eficiente
- **Trunk-Based Development**: velocidad y automatización

No existe un workflow mejor que otro, sino **el más adecuado para tu equipo y producto**.

---

## Parte práctica – Simulando workflows

### Práctica 1: GitHub Flow

```bash
# Creamos un feature branch
git checkout -b feature/agregar-papa

echo "papa" >> verduras.txt
git commit -am "feat: agregamos papa a la lista de verduras"

# Volvemos a main y hacemos merge
git checkout main
git merge feature/agregar-papa
```

---

### Práctica 2: Git Flow simplificado

```bash
# Creamos rama develop
git checkout -b develop

# Creamos una feature
git checkout -b feature/agregar-naranja
echo "naranja" >> frutas.txt
git commit -am "feat: agregamos naranja"

# Merge a develop
git checkout develop
git merge feature/agregar-naranja
```

---

### Práctica 3: Trunk-Based Development

```bash
# Rama corta
git checkout -b quick-fix

echo "lechuga" >> verduras.txt
git commit -am "chore: agregamos lechuga"

# Merge inmediato al trunk
git checkout main
git merge quick-fix
```

---

## Conclusión

- **Git Flow** aporta orden y control
- **GitHub Flow** equilibra simplicidad y velocidad
- **Trunk-Based Development** maximiza la entrega continua

Elegí el workflow según **tu equipo, tu producto y tu nivel de automatización**.
