# Guía de comandos Git: los más útiles y utilizados

Git es un sistema de control de versiones que permite registrar cambios en un proyecto, volver a versiones anteriores y trabajar en equipo sin perder el código.

## 1. Configuración inicial

### `git config`

Permite configurar Git.

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
```

- `user.name`: establece el nombre del autor de los cambios.
- `user.email`: establece el correo asociado a los commits.
- `--global`: aplica la configuración a todos los repositorios del usuario.

Consultar la configuración:

```bash
git config --list
```

## 2. Crear o descargar un repositorio

### `git init`

Inicializa un repositorio Git en la carpeta actual.

```bash
git init
```

Se utiliza cuando empezamos un proyecto desde cero y queremos comenzar a registrar sus cambios.

### `git clone`

Copia un repositorio remoto a nuestra computadora.

```bash
git clone https://github.com/usuario/proyecto.git
```

Se utiliza para descargar proyectos existentes desde plataformas como GitHub o GitLab.

### `git status`

Muestra el estado actual del repositorio.

```bash
git status
```

Permite identificar archivos nuevos, modificados, preparados para un commit o pendientes de guardar en el historial.

Es uno de los comandos más importantes para usar durante el trabajo diario.

## 3. Registrar cambios

### `git add`

Prepara archivos para incluirlos en el próximo commit.

Agregar un archivo específico:

```bash
git add index.js
```

Agregar todos los cambios del directorio actual:

```bash
git add .
```

Los cambios agregados quedan en el área de preparación (*staging area*), pero todavía no forman parte de un commit.

### `git commit`

Guarda los cambios preparados en el historial del repositorio.

```bash
git commit -m "Agrega validaciones al formulario"
```

- `-m`: permite escribir un mensaje que describa los cambios.
- El commit registra una versión del proyecto.

**Buena práctica:** utilizar mensajes claros que expliquen qué se modificó.

Ejemplos:

```bash
git commit -m "Corrige error de autenticación"
git commit -m "Agrega componente de navegación"
git commit -m "Actualiza documentación del proyecto"
```

### `git log`

Muestra el historial de commits.

```bash
git log
```

Para obtener una versión más compacta:

```bash
git log --oneline
```

Para ver los commits y las ramas de forma gráfica:

```bash
git log --oneline --graph --all
```

## 4. Trabajar con repositorios remotos

Un repositorio remoto es una copia del proyecto alojada en un servidor, por ejemplo, GitHub.

### `git remote`

Permite consultar o administrar los repositorios remotos.

```bash
git remote -v
```

Muestra las URL de los repositorios remotos configurados.

Agregar un remoto llamado `origin`:

```bash
git remote add origin https://github.com/usuario/proyecto.git
```

`origin` es el nombre convencional que suele utilizarse para el repositorio remoto principal.

### `git push`

Envía los commits locales a un repositorio remoto.

```bash
git push
```

Para publicar una rama por primera vez:

```bash
git push -u origin main
```

- `origin`: identifica el repositorio remoto.
- `main`: indica la rama que se publicará.
- `-u`: configura la relación de seguimiento para facilitar futuros `push` y `pull`.

### `git pull`

Descarga los cambios del repositorio remoto e intenta integrarlos en la rama actual.

```bash
git pull
```

Se utiliza para actualizar el proyecto local con los cambios que otras personas publicaron.

### `git fetch`

Descarga información y commits nuevos del repositorio remoto, pero no integra automáticamente esos cambios en la rama actual.

```bash
git fetch
```

Es útil para revisar qué cambios existen en el remoto antes de incorporarlos.

### Diferencia entre `pull`, `fetch` y `push`

| Comando | Función |
|---|---|
| `git fetch` | Descarga novedades del remoto sin integrarlas en la rama actual. |
| `git pull` | Descarga e integra cambios remotos. |
| `git push` | Publica commits locales en el remoto. |

## 5. Trabajar con ramas (*branches*)

Las ramas permiten desarrollar funcionalidades o corregir errores sin modificar directamente la rama principal.

### `git branch`

Lista las ramas existentes.

```bash
git branch
```

Crear una rama:

```bash
git branch nueva-funcionalidad
```

Eliminar una rama local ya integrada:

```bash
git branch -d nueva-funcionalidad
```

### `git switch`

Permite cambiar de rama.

```bash
git switch main
```

Crear una rama nueva y cambiarse a ella:

```bash
git switch -c nueva-funcionalidad
```

Es la forma recomendada actualmente para realizar estas operaciones.

### `git merge`

Integra los cambios de una rama en la rama actual.

Por ejemplo, para incorporar `nueva-funcionalidad` en `main`:

```bash
git switch main
git merge nueva-funcionalidad
```

Primero nos posicionamos en `main` y luego integramos los cambios de la otra rama.

### `git diff`

Muestra las diferencias entre versiones de archivos.

Ver cambios que todavía no fueron preparados:

```bash
git diff
```

Ver cambios preparados para el próximo commit:

```bash
git diff --staged
```

Comparar dos ramas:

```bash
git diff main..nueva-funcionalidad
```

Es útil para revisar el código antes de confirmar los cambios.

## 6. Deshacer cambios

### `git restore`

Permite descartar modificaciones locales que todavía no fueron confirmadas en un commit.

Descartar los cambios de un archivo:

```bash
git restore index.js
```

**Atención:** esta operación elimina las modificaciones no confirmadas de ese archivo.

Quitar un archivo del área de preparación sin borrar sus modificaciones:

```bash
git restore --staged index.js
```

### `git reset`

Permite mover el puntero de la rama actual y, según las opciones, modificar el área de preparación o el directorio de trabajo.

Deshacer el último commit conservando los cambios preparados:

```bash
git reset --soft HEAD~1
```

Deshacer el último commit y conservar los cambios en los archivos, pero quitarlos del área de preparación:

```bash
git reset HEAD~1
```

**Atención:** `git reset --hard` puede eliminar cambios locales y sobrescribir archivos. No debe utilizarse sin entender sus consecuencias.

### `git revert`

Crea un nuevo commit que revierte los cambios de un commit anterior.

```bash
git revert HEAD
```

Es especialmente útil para deshacer cambios que ya fueron publicados, porque conserva el historial en lugar de eliminar el commit original.

### Diferencia entre `restore`, `reset` y `revert`

| Comando | Uso principal |
|---|---|
| `restore` | Descartar cambios de archivos o sacarlos del área de preparación. |
| `reset` | Mover la referencia de la rama y modificar el estado del área de preparación o los archivos. |
| `revert` | Crear un nuevo commit que deshace uno anterior. |

## 7. Otros comandos importantes

### `git stash`

Guarda temporalmente cambios locales para poder trabajar en otra cosa sin crear un commit.

Guardar cambios:

```bash
git stash
```

Recuperar los cambios guardados y eliminarlos de la lista de stash:

```bash
git stash pop
```

Ver los cambios guardados:

```bash
git stash list
```

### `git show`

Muestra los detalles de un commit específico.

```bash
git show
```

También se puede consultar un commit mediante su identificador:

```bash
git show a1b2c3d
```

### `git rm`

Elimina un archivo del repositorio y prepara su eliminación para el próximo commit.

```bash
git rm archivo.txt
```

### `git mv`

Cambia el nombre o la ubicación de un archivo.

```bash
git mv archivo-viejo.js archivo-nuevo.js
```

La modificación debe confirmarse posteriormente mediante un commit.

### `git tag`

Permite marcar versiones importantes del proyecto.

```bash
git tag v1.0.0
```

Consultar las etiquetas:

```bash
git tag
```

Las etiquetas se utilizan frecuentemente para identificar versiones publicadas.

## 8. Flujo de trabajo habitual

Este es un flujo sencillo para trabajar en un proyecto con Git:

```bash
# 1. Consultar el estado del proyecto
git status

# 2. Crear una rama para trabajar
git switch -c nueva-funcionalidad

# 3. Modificar los archivos del proyecto

# 4. Revisar los cambios
git diff

# 5. Preparar los archivos
git add .

# 6. Guardar los cambios
git commit -m "Agrega nueva funcionalidad"

# 7. Publicar la rama en GitHub
git push -u origin nueva-funcionalidad
```

Después de publicar la rama, normalmente se abre un *Pull Request* en GitHub para revisar e integrar los cambios en la rama principal.

## 9. Resumen de los comandos más utilizados

| Comando | ¿Para qué sirve? |
|---|---|
| `git init` | Crear un repositorio local. |
| `git clone` | Descargar un repositorio existente. |
| `git status` | Consultar el estado de los archivos. |
| `git add` | Preparar cambios para un commit. |
| `git commit` | Registrar cambios en el historial. |
| `git log` | Consultar el historial. |
| `git diff` | Revisar diferencias entre archivos o versiones. |
| `git branch` | Consultar, crear o eliminar ramas. |
| `git switch` | Cambiar de rama o crear una nueva. |
| `git merge` | Integrar cambios de otra rama. |
| `git pull` | Descargar e integrar cambios remotos. |
| `git fetch` | Descargar novedades sin integrarlas. |
| `git push` | Publicar commits en el remoto. |
| `git restore` | Descartar cambios o quitar archivos del staging. |
| `git revert` | Deshacer un commit mediante otro commit. |
| `git stash` | Guardar cambios temporalmente. |

## 10. Conceptos fundamentales

Para entender Git, es importante distinguir estos cuatro conceptos:

- **Working directory:** carpeta donde se encuentran los archivos del proyecto y donde realizamos modificaciones.
- **Staging area:** área donde seleccionamos los cambios que formarán parte del próximo commit.
- **Repository:** historial local que almacena los commits.
- **Remote:** repositorio alojado en un servidor, como GitHub.

El flujo básico es:

```text
Modificar archivos
       ↓
    git add
       ↓
   Staging area
       ↓
   git commit
       ↓
 Historial local
       ↓
    git push
       ↓
 Repositorio remoto
```

**Regla para recordar:** `add` prepara, `commit` registra y `push` publica.
