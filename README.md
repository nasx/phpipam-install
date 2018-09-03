
# phpIPAM Installer
Written to support RHEL 7 and phpIPAM version 1.4.
## About
This Ansible role is designed to do a reference install on RHEL 7 using the RHEL install instructions [1] detailed on the phpIPAM website. It is assumed that SELinux is ENABLED on the installation system and a custom SELinux policy [2] is created to allow subnet discovery.
## Variables
| Variable Name | Values | Vault | Description |
| :--- | :--- | :--- | :--- |
| mysql_secure_installation | True/False | No | Perform the equivalent of running the mysql_secure_installation command. |
| php_timezone | String | No | See php documentation [3] for a list of valid vaules (default is America/New_York). |
| phpipam_git_url | String | No | Link to the phpIPAM source code (default is https://github.com/phpipam/phpipam.git). |
| phpipam_database_username | String | Yes | The username phpIPAM will use to connect to the MariaDB database (default is phpipam). |
| phpipam_database_password | String | Yes | The password phpIPAM will use to connect to the MariaDB database. |
| phpipam_database_name | String | Yes | The database phpIPAM will use to store application database in the MariaDB database (default is phpipam). |
| mysql_root_password | String | Yes | The password for the MariaDB root user. The root user is used to bootstrap the phpIPAM phpipam_database_username, associated permissions and create the phpipam_database_name database. |
## Usage
### Cloning the Repository
Create a directory and clone the git repository as follows:
```bash
git clone https://github.com/nasx/phpipam-install
```
### Variables
The non-vault variables should be updated in roles/phpIPAM/vars/main.yml to reflect your installation. They can also be overwritten when launching the playbook by assigning them as extra vars (see ansible-playbook man page, specifically --extra-vars or -e).

Vault variables should be added to a vault and passed to the command line as extra vars (see above). To create a vault, use the ansible-vault command:
```bash
ansible-vault create vault
```
### Inventory
The phpIPAM role is expecting a single host in your inventory file. The contents of the inventory file could be as simple as a single line with a host. This host should be in FQDN format. For example:
```
ipam.lab.local
```
### Running the Playbook
To run the playbook we will use the ansible-playbook command to run the install.yml playbook. The install.yml playbook simple runs the phpIPAM role on all hosts marked in the provided inventory file.

The following example will leverage the root user (-u root) to login to the host defined in your inventory file. It will prompt you for the root password (-k) as well as the vault password (--ask-vault-pass) using the vault we created above (-e @vault). The playbook should run on the host(s) defined in the inventory file simply named inventory (-i inventory).
```bash
ansible-playbook -u root -k --ask-vault-pass -e @vault -i inventory install.yml
```
### Next Steps
After successfully running the playbook you can login to phpIPAM using the default credentials: admin/ipamadmin.
## External Links
[1] - https://phpipam.net/news/phpipam-installation-on-centos-7/
[2] - https://phpipam.net/news/selinux-policy-for-icmp-checks/
[3] - http://php.net/manual/en/timezones.php
