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