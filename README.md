# Terraform Beginner Bootcamp 2023

## Semantic Versioning

This project is going to utilize semantic versioning for its tagging.

[Semantic Versioning](https://semver.org/)

The general format:

**MAJOR.MINOR.PATCH**, eg. `1.0.1`

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes

## Intall the Terraform CLI

### Considerations with the Terraform CLI changes.

The Terraform CLI installation instructions have changed due to gpg keyring changes. So we needed to refer to the latest install CLI 
instructions via Terraform Documentation and change the scripting for install.

This bash script is located here: [./bin/install_terraform_cli](./bin/install_terraform_cli)

### Considerations for Linux distributions

This project is built against Ubuntu, please consider checking your Linux distribution for any documentation changes.

[How to check Linux distriubution](https://www.tecmint.com/check-linux-os-version/#:~:text=The%20best%20way%20to%20determine,on%20almost%20all%20Linux%20systems)

Example of checking OS version.

```
cat /etc/os-release

```
### Refactoring into Bash Scripts

While fixing the Terraform CLI gpg deprecation issues we notice 
that the bash script steps had change. So we decide to create a bash script to install the Terraform CLI.

-  This will keep the Gitpod Task File ([.gitpod.yml](.gitpod.yml))tidy.
-  This allow us an easir time to debug and execute manually the Terraform install.
-  This will allow better portability for other projects that need to install the CLI.

[Intall the Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

### Shebang Considerations

https://en.wikipedia.org/wiki/Shebang_(Unix)

The shebang is the combination of the # (pound key) and ! (exclamation mark). 
This character combination has a special meaning when it is used in the very first line of the script. 
It is used to specify the interpreter with which the given script will be run by default

ChatGPT recommended this format for bash: `#!/usr/bin/env bash`

-  For portability between different Linux distributions
-  Will search the user's PATH for the bash executable

#### Execution Considerations

When executing the bash script we can use the `./` shorthand notation to execute the bash script.

eg. `./bin/install_terraform_cli`

If we are using a script in .gitpod.yml we need to point the script to a program to interpret it.

eg. `source ./bin/install_terraform_cli`

#### Linux Permissions Considerations 

In order to make our bash script executable we need to change the Linux permissions for the file to be executable at the user mode

```sh

cmod u+x ./bin/install_terraform_cli

```

Alternatively:

```sh

cmod 744 ./bin/install_terraform_cli

```

https://en.wikipedia.org/wiki/Chmod

In Unix and Unix-like operating systems, chmod is the command and system call used to change the access permissions 
and the special mode flags (the setuid, setgid, and sticky flags) of file system objects (files and directories). Collectively these were originally called its modes,and the name chmod was chosen as an abbreviation of change mode



### GitHub Lifecycle (Before, Init, Command)

When you restart a workspace, Gitpod already executed the init task either as part of a Prebuild or when you started the workspace for the first time.

We need to be careful when using the Init in the .gitpod.yml file because it will not rerun if we restarted an existing workspace. 
Instead susbtitute for Before

eg. 

```yml
tasks:
  - name: terraform
    before: |
      source .bin/install_terraform_cli
```

https://www.gitpod.io/docs/configure/workspaces/workspace-lifecycle
https://www.gitpod.io/docs/configure/workspaces/tasks

### Working with Env Vars

- We can list out all Environment variables (ENV Vars) using the `env` command.
- We can filter specific env vars using grep eg. `env | grep AWS_`

#### Setting and Unsetting Env Vars

- In the terminal we can set using `export PROJECT_ROOT='./workspace/terraform-beginner-bootcamp-2023'`
- In the terminal we can ubset using `unset PROJECT_ROOT='./workspace/terraform-beginner-bootcamp-2023'`
- We can set an env var temporarely when just running a command

```sh
HELLO='world' ./bin/print_message

```
- Within a bash cript we can set an env var without writng export  eg.

```sh
#!/usr/bin/env bash

HELLO='world'

echo HELLO

```

#### Priting Env Vars

We can print env var using eg. `echo HELLO`

#### Scoping of Env Vars

- When you open a new bash terminal in VSCode it will not be aware of Env Vars that you have set in another terminal window.

- If you want Env Vars to persist across all future bash terminals that are open you need to set Env Vars in your bash profil. eg. `.bash_profile`

#### Persisting Env Vars in Gitpod

We can persist env vars in Gitpod by storing them in Gitpod Secrets Sorage.

```
gp env HELLO='world'

```

- All future workspaces launched will set the env vars for all bash terminals opened in those worskapces.
- You can also set env vars in the `.gtpod.yml` but this can only contain non-sensitive env vars.
