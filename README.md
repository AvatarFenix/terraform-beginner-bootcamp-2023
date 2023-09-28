# Terraform Beginner Bootcamp 2023

## Table of Contents

- [Semantic Versioning](#semantic-versioning)
- [Install the Terraform CLI](#intall-the-terraform-cli)
  - [Considerations with the Terraform CLI changes](#considerations-with-the-terraform-cli-changes)


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


### AWS CLI installation

AWS CLI is installed for the project bvia the bash script [`./bin/install_aws_cli`](./bin/install_aws_cli)


[AWS CLI installation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

We can check if our AWS credential is configured correctcly by running the following AWS CLI command 


```sh
aws sts get-caller-identity

```
If succesfull you should see a json payload return that looks like this:

```json
{
    "UserId": "AKIAIOSFODNN7EXAMPLE",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/TerraformDev"
}

```
[AWS CLI Env Vars](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)

We'll need to generate AWS CLI credentials from IAM User in order to use the AWS CLI.

## Terraforms Basics

### Terraform Registry

Terraform sources thier providers and modules for the Terraform regisdtry which is located at 
[registry.terraform.io](https://registry.terraform.io/)

The Terraform Registry is the main directory of publicly available Terraform providers, and hosts providers for most major infrastructure platforms.

The Terraform Registry includes documentation for a wide range of providers developed by HashiCorp, third-party vendors, and our Terraform community

- **Providers** in Terraform is a plugin that enables interaction with an API. This includes Cloud providers and Software-as-a-service providers. The providers are specified in the Terraform configuration code. They tell Terraform which services it needs to interact with.

Terraform configurations must declare which providers they require so that Terraform can install and use them. Additionally, some providers require configuration (like endpoint URLs or cloud regions) before they can be used.

[Random Terraform Provider](https://registry.terraform.io/providers/hashicorp/random/latest)

- **Modules** are containers for multiple resources that are used together. A module consists of a collection of .tf and/or .tf.json files kept together in a directory.
Modules are the main way to package and reuse resource configurations with Terraform.

Every Terraform configuration has at least one module, known as its root module, which consists of the resources defined in the .tf files in the main working directory.


### Terraform Console

We can see a list of all the Terraform commands by simply typing `terraform` in the terminal console.

#### Terraform Init

At the start of a new Terrafom project we will run `terraform init` to download the binaries fopr the Terrafom providers that we'll use in this project.

#### Terraform Plan

`terraform plan`

This will generate out a changeset about the state of our infrastructure and what will be changed.

We can output this changeset ie. "plan" to be passed to an apply, but often you can just ignore outputting.

#### Terraform Apply

`terraform apply`

This will run a plan and pass the changeset to be executed by Terraform. Apply should prompt us yes or no .
If we want to automatically approve an apply we can provide the auto approve flag eg. `terraform apply --auto-approve`

#### Terraform Destroy

`terraform destroy`

This will destroy resources. 
You can also provide the auto approve flag eg. `terraform destroy --auto-approve`

#### Terraform Lock Files

`.terraform.lock.hcl` contains the locked versioning for the proviers or modules that should be used with this project.

The Terraform Lock File **should be commited** to your Version Control System (VCS) eg. Github

Terraform 0.14 and later utilize a lock file to enable teams to standardize on specific, approved, verified versions of provider plugins. The lock file is essential for Terraform's operation and so will always be generated if one does not exist, even if it is not retained or distributed.

## Terraform State Files 

`.terraform.tfstate` contains information about the current state of your infrastructure.

The Terraform Sate File Lock File **should NOT be commited** to your Version Control System (VCS) eg. Github

This file can contain sensitive information. If you loose this file you lose knowing the state of your infrastructure.

`.terraform.tfstate.backup` is the previous state file state.

Terraform logs information about the resources it has created in a state file. This enables Terraform to know which resources are under its control and when to update and destroy them. The terraform state file, by default, is named terraform.

### Terraform Directory

`.terraform` directory contains binaries of Terraform providers.

#### Migrate state file to cloud

To migrate the state file to Terraform Cloud we need to edit the main.tf file and add the following code

```json
  cloud {
    organization = "ORGANIZATION-NAME"
    workspaces {
      name = "learn-terraform-cloud-migrate"
    }
```

[Cloud Migrate](https://developer.hashicorp.com/terraform/tutorials/cloud/cloud-migrate)

#### Issues with Terraform Cloud Login and Gitpod Workspace

When attempting to run a `terraform login` it will launch bash a wiswig view to generate a token. However it does not work as expected in Gitpod VSCode.

The workaround is manually generate a tokn in Terraform Cloud

[Log in to Terraform Cloud from the CLI](ttps://developer.hashicorp.com/terraform/tutorials/cloud-get-started/cloud-login)

```
https://app.terraform.io/app/settings/tokens?source=terraform-login

```

Then create the file manually here:

```
touch /home/gitpod/.terraform.d/credentials.tfrc.json
open /home/gitpod/.terraform.d/credentials.tfrc.json

```
Provide the following code  (replace token on file)

[File credentials.tfrc.json content ](https://www.reddit.com/r/Terraform/comments/rtl5ey/can_anyone_please_show_me_show_me_how/)

```json
{
  "credentials": {
    "app.terraform.io": {
      "token": "token"
    }
  }
}

```

We have automated this workaround with the following bash script [bin/generate_tfrc_credentials](bin/generate_tfrc_credentials)