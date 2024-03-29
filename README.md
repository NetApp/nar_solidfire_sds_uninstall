Role Name
=========
nar_solidfire_sds_uninstall 

Description
-----------
This role stops solidfire service, removes the sds package and configuration directories on hosts.
We recommend this role only be run on hosts not in an active cluster, and reachable via the network.
The same inventory file that was used for install can be used to uninstall the hosts.

WARNING: Uninstalling hosts from an active cluster can lead to data loss. All hosts in the supplied inventory file will be uninstalled.
Role should only be used during testing phase, not after deployment phase, for data safety.

Requirements
------------
* This role requires the target systems use RHEL 7.6 or later.  Additionally, the target system is required to have eSDS installed.

* This role requires Python version 3.6 or later and Ansible version 2.10 or later installed on the controller.

* This role requires root or superuser privilege to run.


Role Variables
--------------


| Variable                          | Required | Default | Description                                 | Comments                   |
|-----------------------------------|----------|---------|---------------------------------------------|----------------------------|
| sf_cluster_admin_passwd           | no       | ""      | The password of the Cluster Administrator   | Needed for nodes in cluster|
| sf_cluster_admin_username         | no       | ""      | The username of the Cluster Administrator   | Needed for nodes in cluster|
| sf_validate_certs                 | no       | False   | Whether to validate SSL/TLS certificates    |                            |
| sf_use_proxy                      | no       | False   | Use proxy config. See note [1] below        |                            |
| sf_api_version                    | no       | 12.2    | The version of the SolidFire eSDS API to    |                            |
|                                   |          |         | use, should not be modified!                |                            |
| sf_allow_uninstall_active_cluster | no       | False   | "True" allows forceful uninstallation even  |                            |
|                                   |          |         | nodes are in active cluster serving data.   |                            |
|                                   |          |         | Nodes should be removed from active cluster |                            |
|                                   |          |         | before running this role for data safety.   |                            |

Notes:

[1] Setting sf_use_proxy might have no effect due to a bug in Ansible version 2.10.x or earlier. Instead, use the environment
    variable "no_proxy" in a playbook as a workaround when needed until the bug is fixed by Ansible. See the example:
```ansible
    environment:
      no_proxy: <target_ip_address>                                                             
```

Example Playbook
----------------

```
  - name: Uninstall SolidFire Enterprise SDS
    remote_user: <root or superuser ID>
    gather_facts: True
    hosts: all
    roles:
      - role: nar_solidfire_sds_uninstall
```

Example Inventory
-----------------
```
all:
  hosts:
    hosts1:
      ansible_host: <host1 IP>
    host2:
      ansible_host: <host2 IP>
  vars:
    ansible_python_interpreter: <full path on target hosts to a python interpreter, such as /usr/libexec/platform-python>
    become: True
    ansible_password: <login user password> # prefer vault key
    ansible_become_pass: <privilege escalation password> # or --ask-become-pass on command line to prompt for input
```

License
-------

GNU v3

Author Information
------------------
NetApp
http://www.netapp.com
