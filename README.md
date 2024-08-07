# Terraform Beginner Bootcamp 2023

## Semantic Versioning :mage:

This project is gonna use semantic versioning for its tagging.
[semver.org](https://semver.org/)

The general format:

**MAJOR.MINOR.PATCH**, eg. `1.0.1`

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes
Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.


## Install the Terraform CLI


### Considerations with the Terraform CLI changes
The Terraform CLI installation instructions due to gpg keyring changes. I needed to refer to the latest CLI instructions via terraform Documentation and change the scripting for install.

[Install Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)


### Considerations for Linux Distributions

This project is built on Ubuntu. Please consider checking your Linux Distribution and change accordingly to your needs.

[How To Check OS Version](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)

Example of checking OS Version
```
$ cat /etc/os-release

lsb_release -a
hostnamectl
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
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.3 LTS
Release:        22.04
Codename:       jammy
lsb_release -a
hostnamectl
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
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.3 LTS
Release:        22.04
Codename:       jammy
```

### Refacoring into Bash Scripts

While fixing the the Terraform CLI gpg depreciation issues I noticed the bash comand steps were a considerable amount more could. So I decided to create a bash script to install the Terraform CLI.

This bash script is located at [./bin/install_terraform_cli](./bin/install_terraform_cli)

- This will keep the Gitpod Task File([.gitpod.yml](.gitpod.yml)) tidy
- This will allow me to more easily debug and execute the Terraform CLI Install
- This will allow better portability for projects that need to install terraform CLI

### Shebang 

A Shebang (Sha-bang) tells the bash script what program will interpret the script. eg. `#!/bin/bash`

CHATGPT recommended this format for bash `#!/usr/bin/env bash`
- Portability for different distributions
- Will seartch user's PATH for bash executable

https://en.wikipedia.org/wiki/Shebang_(Unix)

#### Execution Considerations

When executing bash script we can use the `./` shorthand notation to execute the bash script.

eg. `./bin/install_terraform_cli`


If we are using a script in .gitpod.yml we need to point the script to a program to interptet it.

eg. `source ./bin/install_terraform_cli`

#### Linux Permissions Considerations

In order to make our bash scripts executable we need to change the linux permissions for the file to be executable at the user level:

```sh
chmod u+x ./bin/install_terraform_cli
```

Alternatively:
```sh
chmod 744 ./bin/install_terraform_cli
```
[chmod wiki](https://en.wikipedia.org/wiki/Chmod)

### Github Lifecycle (Before, Init, Command)

We need to becare when using the Init command because it will not rerun if we restart on an existing workspace.

[Gitpod tasks](https://www.gitpod.io/docs/configure/workspaces/tasks)

### Working Env Vars

#### env command
We can list out all Environment Variables(Env Vars) using the `env` command

We can filter specific env vars using grep eg `env | grep AWS_`

#### Setting and Unsetting Env Vars

In the terminal we can set usin `export HELLO='world'`

In the terminal we can unset using `unset HELLO`

We can set env var temporarily when just running a command

```sh
HELLO='world' ./bin/print_message
```

With a bash script we can set env without writing export eg.

```sh
#! /usr/bin/env bash

HELLO=world
```

#### Printing Vars

We can print an env var using echo eg. `echo $HELLO`

#### Scoping of Env Vars

When you open up new bash terminals in VScode it will not be aware of env vars tat you have set in another window.

If you want to Env Vars to persist across all future bash terminals that are open, you need to set env vars in your bash profiles. eg `.bash_profile`

#### Persisting Env Vars in Gitpod

We can persist env vars into git pod by storing them in Gitpod Secrets Storage

```
gp env HELLO='world'
```

All future workspaces launched will set the env vars for all bash terminals opened in those workspaces.

You can also set env vars in the `.gitpod.yml` but this can only contain non-sensitive env vars.