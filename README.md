# Terraform Beginner Bootcamp 2023

## Semantic Versioning :mage:

This project is going to utilize semantic versioning for its tagging.
[semver.org](https://semver.org/)

The general format:

 **MAJOR.MINOR.PATCH**, eg.  `1.0.1`

 - **MAJOR** version when you make incompatible API changes
 - **MINOR** version when you add functionality in a backward compatible manner
 - **PATCH** version when you make backward compatible bug fixes

Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

## Install the Terraform CLI

### Considerations with the Terraform CLI changes

The Terraform CLI installation instructions have changed due to gpg keyring changes. So we needed to refer to the latest install CLI instructions via Terraform Documentation and change the scripting for install.

[Install Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

### Considerations for Linux Distribution

This project build against Ubuntu.
Please consider checking your Linux distribution and change according to your destribution needs.

Example of checking OS version:

```
gitpod /workspace/terraform-beginner-bootcamp-2023 (3-refactor-cli-terraform) $ cat /etc/os-release 
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

### Refactoring into Bash Scripts

While fixing the Terraform CLI gpg deprecation issues, we noticed that bash scripts steps were a considerable amount more code. So we decided to create a Bash script to install the Terraform CLI.


This bash script is located here: [./bin/install_terraform_cli](./bin/install_terraform_cli)

- This will keep the Gitpod file ([.gitpod.yml](.gitpod.yml)) tidy.
- This allows us an easier to debug and execute manually Terraform CLI install.
- This will allow better portability that need to install Terrafrom CLI.

#### Shebang Considerations

A shebang (Sha-bang) tells the bash script what program that will interpret the script. e.g. `#!/bin/bash`

ChatGPT recommands we use this format for bash: `!/usr/bin/env bash`

- for portability for different OS distributions
- will search the user's PATH for the bash executable

#### Execution Considerations

When executing the bash script, we can use the `./` shorthand notation.

e.g. `bin/install_terraform_cli`

if we are using script in `.gitpod.yml` we need to point the script to a program to interpret it.

e.g. `source ./bin/install_terraform_cli`

#### Linux permissions

In order to make our bash scripts executable, we need to change linux permission for the fix to be executable at the user mode.

```
chmod u+x ./bin/install_terraform_cli
```

Alternatively:

```
chmod 744 ./bin/install_terraform_cli
```
### Github Lifecycle (Before, init, command)

We need to be careful when using the Init because it will not rerun if we restart an existing workspace.

