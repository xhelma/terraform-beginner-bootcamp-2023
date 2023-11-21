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

### Working Env Vars

#### env command

We can list out all environment variables (Env Vars) using the `env` command 

We can filter specific env vars using grep eg. `env | grep AWS_`

#### Setting and unsetting Env Vars

In the terminal we can set using `export HELLO='world'`

In the terminal we can unset using `unset HELLO`

We can set an env var temporarily when just running a command 

```sh
HELLO='world' ./bin/print_message
```

Within a bash script, we can set an env var without writing export eg.

```sh
#!/usr/bin/env bash
HELLO='world'
echo $HELLO
```

#### Printing Vars

We can print an env var using echo eg. `echo $HELLO`

#### Scoping of Env Vars

When you open up new Bash terminals in Vscode, it will not be aware of env vars that you have set in another window.

If you want to Env Vars to persist across all future bash terminals that are open, you need to set Env Vars in your bash profile. eg. `.bash_profile`

#### Persisting Env Vars in Gitpod

We can persist env vars in Gitpod by storing them in Gitpod Secrets Storage.

```
gp env HELLO='world'
```

All future workspaces launched will set the env vars for all bash terminals open in those workspaces.

You can also set env vars in the `.gitpod.yml` but this can only contain non-sensitive env vars.

### AWS CLI Installation
AWS CLI is installed for this project via the bash script [`./bin/install_aws_cli`](./bin/install_aws_cli)

[Getting started Install (AWS CLI)](/bin/install_aws_cli](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
[AWS CLI env vars](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)

We can check if our AWS credentials are configured correctly by running the following AWS CLI command:
```sh
aws sts get-caller-identity
```
If it is successful, you should see a json payload return that looks like this: 
```json
{
    "UserId": "XXXXXXXXXXXXXXXXXXXXX",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/terraform-beginner-bootcamp"
}
```

We'll need to generate AWS CLI credentials from IAM user in order to use AWS CLI.

