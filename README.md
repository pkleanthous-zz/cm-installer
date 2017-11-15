# cm-installer
Simple bash script that installs Chef Client or Salt Minion on an instance

## How to use it
The script needs 2 environment variables:

| Env var        |        Supported values                | Default |
|----------------|:--------------------------------------:|:-------:|
| CM_PROVISIONER | `saltminion` OR `chefclient`           |   N/A   |
| CM_VERSION     |  Any value as is defiend by the client | latest  |

Set the env. vars and run the script

## Integrating the script with [bento project](https://github.com/chef/bento)

* Checkout bento project
* Copy the `cminstaller.sh` script at `bento/_common/` path
* Open a packer configuration file like `bento/ubuntu/ubuntu-16.04-amd64.json`
  * Locate the `variables` section and add the following configuration
  ```json
  "variables": {
    ...
    "cm_provisioner": "",
    "cm_version": "",
    ...
  }
  ```
  * Locate the `provisioners` section and add the following configuration
    * inside `environment_vars` section
  ```json
  "environment_vars": [
    "CM_PROVISIONER={{user `cm_provisioner`}}",
    "CM_VERSION={{user `cm_version`}}"
  ],
  ```
  * inside `scripts` section
  ```json
  "scripts": [
    "../_common/cminstaller.sh",
  ],
  ```
* Run packer and pass the relevant values
  ```
  $ cd ubuntu
  $ packer build \
    -var 'cm_provisioner=foo' \
    -var 'cm_version=1.2.3' \
    ubuntu-16.04-amd64.json
  ```
