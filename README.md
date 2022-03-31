![Build Status](https://github.com/volac-genomics/vg-install/workflows/ansible-ci/badge.svg)
# vg-install
This playbook installs the following:
* [Nextflow](https://www.nextflow.io) 21.10.6
* [Docker](https://www.docker.com) 5:20.10.14\~3-0~ubuntu-focal
* [Go](https://go.dev) 1.17.8
* [Singularity](https://sylabs.io/singularity) v3.9.7

and clones the following repositories to the directory `$HOME/genome_tools`:
* https://github.com/volac-genomics/Tool_1
* https://github.com/volac-genomics/Tool_2
* https://github.com/volac-genomics/Tool_3

It is designed to run on a clean Ubuntu 20.04 server (i.e. a server on which nextflow etc. have not yet been installed).

This repo also contains a GitHub Actions workflow which tests the playbook and the Tool_1 workflow.

## Install instructions
Install ansible
```
sudo apt update
sudo apt install ansible
```

Clone the repo
```
https://github.com/volac-genomics/vg-install.git
```
Run the playbook
```
cd vg-install
ansible-playbook playbook.yml
```

## Updating software versions
The versions for the installed software are found in `vg-install/roles/nextflow-container/defaults/main.yml`. 
These can be updated. When updates are made to the playbook, a new GitHub Actions run is triggered.

To check for the latest versions of Nextflow look [here](https://github.com/nextflow-io/nextflow/releases)

Versions of Singularity can be found [here](https://github.com/sylabs/singularity/releases). 
The release notes should suggest which version of Go to use.
Note that Singularity is end of life and will be replaced with [Apptainer](https://github.com/apptainer/apptainer)

A list of the latest versions of Docker can be found [here](https://docs.docker.com/engine/release-notes/). 
To update the version in the ansible you'll have to use the following format `5:VERSION-NO-HERE~3-0~OS-NAME-HERE`


## GitHub Actions Run
Everytime a commit is made to main branch (unless it's just the README being updated), this triggers a run of the GitHub Actions workflow found in 
`vg-install/.github/workflows/main.yml`.
This workflow tests the ansible playbook by running it on a GitHub Actions hosted server.
Once the ansible-playbook has finished running, it then tests the install by running the Tool_1 workflow.
The OS of the server is set here:
https://github.com/volac-genomics/vg-install/blob/76327a0314713ed074f19abc870b0690b590b518/.github/workflows/main.yml#L16
This can be changed to another Ubuntu environment e.g. `ubuntu-18.04` (note that if you do this, you'll have to update the Docker version to match the OS
and that this playbook can only run on Ubuntu servers). 
More information on GitHub environments can be found [here](https://github.com/actions/virtual-environments).

If the GitHub Actions workflow run is successful, you'll see a green tick next to the commit, failed runs show a red cross.
You can also check the badge at the top of the README for the build status.
To investigate a failed run, look under the Actions tab at the top of the page for more information, 
then click on the failed run and then click build.
The workflow can also be manually run through the Actions tab.

