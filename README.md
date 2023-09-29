# Terraform Beginner Bootcamp 2023

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






