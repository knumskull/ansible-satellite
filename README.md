# ansible-satellite
Automate Satellite Installation and Configuration using Ansible Roles

# Requirements
## Ansible Roles
- `redhat.satellite` | [Documentation](https://console.redhat.com/ansible/automation-hub/repo/published/redhat/satellite/docs/)
- `redhat.satellite_operations` | [Documentation](https://console.redhat.com/ansible/automation-hub/repo/published/redhat/satellite_operations/docs/)

## Problem with roles `content_views` and `content_view_publish` 
Both roles require the variable `satellite_content_views` defined in different ways. To workaround this, the role task for `content_view_publish` can be adjusted. 

```diff
--- .ansible/collections/ansible_collections/redhat/satellite/roles/content_view_publish/tasks/main.yml  2023-03-15 22:34:42.626174482 +0100
+++ main.yml    2023-03-15 22:34:35.401170571 +0100
@@ -5,7 +5,7 @@
     password: "{{ satellite_password | default(omit) }}"
     server_url: "{{ satellite_server_url | default(omit) }}"
     validate_certs: "{{ satellite_validate_certs | default(omit) }}"
-    content_view: "{{ content_view }}"
+    content_view: "{{ content_view.name | default(content_view) }}"
     organization: "{{ satellite_organization }}"
   loop: "{{ satellite_content_views }}"
   loop_control:

```

The fix is addressed upstream in [Pullrequest #1571](https://github.com/theforeman/foreman-ansible-modules/pull/1571).

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
