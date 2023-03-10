# ansible-satellite
Automate Satellite Installation and Configuration using Ansible Roles

# Requirements
## Ansible Roles
- `redhat.satellite` | [Documentation](https://console.redhat.com/ansible/automation-hub/repo/published/redhat/satellite/docs/)
- `redhat.satellite_operations` | [Documentation](https://console.redhat.com/ansible/automation-hub/repo/published/redhat/satellite_operations/docs/)

## Virtual Machine used
```
CPU: 4 cores
Memory: 20G
Disk: 100G
```

I used [Red Hat Enterprise Linux 8.7 KVM Guest Image](https://access.redhat.com/downloads/content/479/ver=/rhel---8/8.7/x86_64/product-software) to install the basic operating system.

# Steps done in the playbook
1. Register the system at Red Hat Customer Portal, using an Activation-Key
2. Configure required repositories
3. Setup and Install Satellite dependencies
4. Run basic installation of Satellite using `satellite-installer`
5. Download and import Subscirption Manifest
6. Configure Lifecycle Environments
7. Configure Red Hat Repositories
8. Synchronize Red Hat Repositories (If tag `sync` is defined)
9. Configure content-views
10. Publish content-views  (If tag `publish` is defined)
11. Housekeeping of content-views (default: Only keep 1 additional version)
12. Setup additional Satellite Settings
13. Configure Activation Keys


# Usage
To execute this playbook, run nfollwing command.
```
$ ansible-playbook satellite-dev.yml -K -b --ask-vault-pass --tags sync,publish 
```
