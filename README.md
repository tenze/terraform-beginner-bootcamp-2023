# Terraform Beginner Bootcamp 2023

## Table of Content

-[Semantic Versioning](#sementic-versioning)
-[Install the Terraform CLI](#aws-cli-installation)
  -[Consideration-with-the-terraform-cli-changes](#considerations-with-the-terraform-cli-changes)

## Sementic Versioning :mage: 

This project is going to unitlize semantic versining for its tagging. 
[semver.org](https://semver.org/)

The general format: 

 **MAJOR.MINOR.PATCH**, eg. `1.0.1`

- **MAJOR**  version when you make incompatible API changes
- **MINOR**  version when you add functionality in a backward compatible manner
- **PATCH**  version when you make backward compatible bug fixes


## Install the Terraform CLI

### Considerations with the Terraform CLI changes. So we needed refer to the altest install CLI instruction
The Terraform CLI installation instructions have changed due to gpg keyring changes. So we needed to refer to the latest install CLI instructions via Terraform Documentation and change the scripting for install. 

[Install Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

Example of checking OS Version: 
```
$cat /etc/os-release
PRETTY_NAME="Ubuntu 22.04.3 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.3 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```

### Consideration for Linux Distribution

This project is built against Ubuntu. Please consider checking your Linux Distribution and change accordingly to the distribution needs

[How to check OS version in Linux](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)


### Refactoring into Bash Scripts 

While fixing the Terraform CLI gpg depreciation issues we notice that the bash scripts steps were a considerable amount more code. So we decided to create a bash script to install the Terraform CLI. 

This bash script is located here: [./bin/install_terraform_cli](./bin/install_terraform_cli)

- This will keep the Gitpod Task File ([.gitpod.yml](.gitpod.yml))tidy.
- This allow us an easier to debog and execute manually Terraform CLI install.
- This will allow better portability for other projects that need to install Terraform CLI.

#### shebang

A shebang (pronounced Sha-bang) tells the bash script what program that will interpet the script. eg. `#!/bin/bash`

ChatGPT recommended this format for bash: `#!/usr/bin/env bash`

-for protability on different OS distribution.
- will search the user's PATH for the bash executable.

(https://en.wikipedia.org/wiki/Shebang_(Unix))

## Execution Considerations
When executing the bash script we can use the `./` shorthand notation to execute the bash script. 
eg. `./bin/install_terraform_cli`
If we are using a script in .gitpod.yml we need to point the script to a program to interpert it.

eg. `source ./bin/install_terraform_cli`


#### Linux Permission Consideration

In order to make out bash scripts executable we need to change the linux permission for the file to be executable at user mode. 
```sh
chmod u+x ./bin/install_terraform_cli
```
alternatively

```sh
    chmod 744 ./bin/install_terraform_cli
```
https://en.wikipedia.org/wiki/Chmod


### Github Lifecycle (Before, Init, Command)
We meed to be careful when using the Init because it iwll not rerun if we restart an existing workspace.

https://www.gitpod.io/docs/configure/workspaces/tasks

### Working Env Vars 

We can list out all Enviroment Variables (Env Vars) using the `env` command.
We can filter specific env vars using grep eg.  `env | grep AWS_`

#### Setting and Unsetting Env Vars

In the terminal we can set using `export HELLO='world`

In the terminal we unset using `unset HELLO`

We can set an env var temporarily when just running a command

```sh
HELLO='world' ./bin/print_message
```
Within a bash script we can set env var without writting export eg. 

```sh
#!/usr/bin/env bash

HELLO='world'
echo $HELLO
```

#### Printing Vars

We can print an Env var using echo `echo $HELLO`

#### Scoping of Env Vars

When you open up new bash terminal in VSCode it will not be aware of env vars that you have set in another window. 

If you want to Env Vars to persist across all future bash terminals that are open you need to set env vars in your bash profile. eg. `.bash_profile`

#### Persisting Env Vars in Gitpod

We can persist env vars into gitpod by storing them in Gitpod Secrets Storage. 

```
gp env HELLO='world'
```

All future workspaces launched will set the env vars for all bash terminals opened in those workspaces.

You can also set env vars in the `.gitpod.yml` but this can only contain non-sensitive env vars.

### AWS CLI Installation

AWS CLI is installed for the project via the bash script [`./bin/install_aws_cli`](./bin/install_aws_cli)

[Getting Started Install (AWS CLI)](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

[AWS CLI Env Vars](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)

We can check if our AWS credentials is configured correctly by running the following AWS CLI command: 

```sh
aws sts get-caller-identity
```
If it is successful you should see a json playload return that looks like this : 

```json
{
"UserID":"ADIRNKNDKLJLWER",
"Account": "123456789012",
"Arn": "arn:aws:iam::123456789012:user/terraform-beginner-bootcamp"
}

```

We'll need to generate AWS CLI credits from IAM User in order to the user AWS CLI.

Steps to generate AWS CLI credits: 

You need aws account if not create an AWS account. 
- Type IAM in the search box.
- Select IAM and create a user with permission to Admin from the group name.
  ( if no group has been created than you need to first create a group with administrative access option ).
- Select User and go to Security Credentials.
- Under Access Keys, Select Create access key and choose the option Command    
  Line Interface (CLI) and select create access key.
- You can either save the UserID and access key or you can download the 
  .csv file on your local machine for future references. 
You can check to see if it run with following command in terminal/aws-cli

```sh
./bin/install_aws_cli
```

## Terraform Basciss

### Terraform Registry

Terraform sources their providers and mudules from the Terraform registry which locate at [registry.terraform.io](https://registry.terraform.io)

- **Providers** is an interface to APIs that will allow to create resources in terraform. 
-**Moduels** are a way to make large amount of terraform code modular, portable and shareable.

[Random Terraform Provider](https://registry.terraform.io/providers/hashicorp/random)
### Terraform Console

We can see a list of all the Terraform commands by simply typing `terraform`

#### Terraform Init

At the start of a new terraform project we will run `terrafom init` to download the binaries for the terraform provides that we'll use in this project.

#### Terraform Plan

`terrafrom plan`

This will generate out a changeset, about the state of our infrastructure and what will be changed. 

We can output this changeset ie. "plan" to be passed to an apply , but often you can just ignore outputting. 

#### Terraform Apply

`terraform apply`

This will run a plan and pass the changeset to be execute by terrafrom. Apply should prompt yes or no. 

If we want to automatically approve an apply we can provide the auto approve flag eg. `terraform apply --auto-approve`

#### Terraform Destroy

`terraform destroy`
This will destroy resources. 

you can also use the auto approve flag to skip the approve the prompt
eg. ` terraform apply --auto-approve`


#### Terraform Lock Files

`.terraform.lock.hcl` contains the loced versioning for the providers or modules that should be used twith this project.

The Terraform Lock File **should be committed** to your VErsion Control System (VSC) eg. Github

#### Terraform State Files

`.terraform.tfstate` contain information about the current state of your infrastructure. 

This file **should not be committed** to your VCS. 

This file can contain sensitive data.

If you loose this file, you loose knowing the state of you infrastructure. 

`.terraform.tfstate.backup` is the previous state file state.

#### Terraform Directory

`.terraform` directory contains binaries of terraform providers.

#### Creating s3 bucket in Terraform

Based on documentation on creating bucket name, we changed the following configuration file :
- Lowercase to true.
- uppercase to false.
- Name length changed to 32.

## Issues with Terraform Cloud and Gitpod Workspace

When attempting to run `terraform login` it will launch bash a wiswig view to generate a token. However it does not work as expected in Gitpod Vscode in the browser. 

The workaround is maually generate a token in Terraform Cloud. 

```
https://app.terraform.io/app/settings/token?source=terraform-login
```

Then create open the file manually here: 

```sh
touch /home/gitpod/.terraform.d/credentials.tfrc.json
open /home/gitpod/.terraform.d/credentials.tfrc.json
```

Provide the following code (replace your token in the file):

Then open the file

```JSON
{
  "credentials":{
    "app.terraform.io":{
      "token": "YOUR-TERRAFORM-CLOUD-TOKEN
    }
  }
}
```

We have automated this workaround with the following bash script [bin/generate_tfrc_credentials](bin/generate_tfrc_credentials)