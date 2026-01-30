
# Connect to a remote repository:

#### If you already have a remote repository that has content

If the repo already exists remotely, you **do NOT initialize anything locally**.
Just clone it:
`git clone https://github.com/user/repo.git cd repo`

Cloning:
- creates the local repo
- sets the remote automatically
- pulls all history

#### If you have a remote repository that is only a skeleton to store what's in your local then:

-> **Initialize an empty repository:**
- `git init`
->**commit the changes you have:**
- `git add .`
- `git commit -m "Initial commit"`
->**set the remote repository** :
To add a new remote, use the `git remote add` command on the terminal, in the directory your repository is stored at.

The `git remote add` command takes two arguments:

- A remote name, for example, `origin`
- A remote URL, for example, `https://github.com/OWNER/REPOSITORY.git`

For example:

```shell
$ git remote add origin https://github.com/OWNER/REPOSITORY.git
# Set a new remote

$ git remote -v
# Verify new remote
> origin  https://github.com/OWNER/REPOSITORY.git (fetch)
> origin  https://github.com/OWNER/REPOSITORY.git (push)
```
->**push the changes**
- `git push -u origin main` "origin is the alias you gave to the remote repository"
