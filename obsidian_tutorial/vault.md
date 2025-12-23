#Learning_obsidian
## What it is:

It's a directory that contains all your markdown files and assets used in them. So It's just a folder containing markdown files, it isn't a container as the one's you create with Docker. This one don't isolate anything, It's just a folder where all your notes are stored.

## How to create it:

When you open the obsidian app you'll see this window. So you have three options with very self-descriptive meaning.

- **Create new vault** : This will create a local directory under the path you give it.
- **Open folder as vault** : Does just that, opens an existing folder as a vault.
- **Open vault from Obsidian Sync** : Don't know exactly how it works but probably just let you open a vault stored in a server via internet.
![[vault_creation.png]]
When you successfully create a vault notice that obsidian will create a hidden folder with the name *.obsidian*, it works as a local configuration for the given vault. Each vault has its own .obsidian, that's the thing that let you have different configuration, plugins, etc, between vaults. Works like .git or .vscode hidden folders, they are there just to manage local changes.