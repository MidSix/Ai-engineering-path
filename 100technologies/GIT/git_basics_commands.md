## GIT - basics commands

### Every day:

| Comando                 | Descripción                                                |
| ----------------------- | ---------------------------------------------------------- |
| git init                | Inicializa un repositorio Git en el directorio actual      |
| git clone <url>         | Clona un repositorio remoto en local                       |
| git status              | Muestra el estado de los archivos (modified, staged, etc.) |
| git add <archivo>       | Añade un archivo al staging area                           |
| git add .               | Añade todos los cambios al staging area                    |
| git commit -m "mensaje" | Crea un commit con mensaje                                 |
| git log                 | Muestra el historial de commits                            |
| git log --oneline       | Historial resumido (una línea por commit)                  |
| git diff                | Muestra diferencias no añadidas al staging                 |
| git diff --staged       | Muestra diferencias ya en staging                          |
### Branches

| Comando                | Descripción                    |     |
| ---------------------- | ------------------------------ | --- |
| git branch             | Lista las ramas locales        |     |
| git branch <rama>      | Crea una nueva rama            |     |
| git checkout <rama>    | Cambia a una rama              |     |
| git checkout -b <rama> | Crea y cambia a una rama       |     |
| git branch -d <rama>   | Borra una rama local           |     |
| git merge <rama>       | Fusiona una rama con la actual |     |
## Remote Repository
| Comando                   | Descripción                                |
| ------------------------- | ------------------------------------------ |
| git remote -v             | Muestra los remotos configurados           |
| git fetch                 | Descarga cambios del remoto (sin fusionar) |
| git pull                  | fetch + merge (actualiza la rama actual)   |
| git push                  | Envía commits al remoto                    |
| git push origin <rama>    | Envía una rama concreta                    |
| git push -u origin <rama> | Envía rama y la deja como upstream         |
## Undo and 

| Comando | Descripción |
|-------|-------------|
| git commit --amend | Modifica el último commit |
| git checkout <hash> | Recupera el estado de un commit |
| git reset --soft HEAD~1 | Deshace commit manteniendo cambios |
| git reset --hard HEAD~1 | Deshace commit y borra cambios |
