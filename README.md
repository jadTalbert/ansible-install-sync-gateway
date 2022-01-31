# Sync Gateway Ansible Playbooks

## Contributing

If you're interested in contributing content, please issue a PR(Pull Request) and your changes will be reviewed and considered for merging.

## Explanation

It is assumed that you already have Ansible installed. For more information on Ansible, review their [installation instructions](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).

If you are interested in installing Sync Gateway quickly without any manual processes, then you can use the Ansible playbooks in this repo to help you attain a fast, stable, reliable and consistent Sync Gateway installation. As of this writing, this implementation works on multiple Linux distributions and has been formally tested on RHEL 7.x+ as well as Amazon Linux AMIs.

## Setup
It is recommended that you use a stand-alone host that has Ansible installed on it and use the host to promote changes to your newly provisioned Sync Gateway nodes.

When you are ready to use this set of playbooks, there is an ```init.yml``` file in the ```bin``` directory that will prompt you to configure your Ansible environment prior to installing Sync Gateway.

## Step 1:
Execute ```ansible-playbook bin/init.yml```

## Ansible Prompts:
  Prompt                | Expected Input
  ----------------------------|-----------------------------
  Would you like to begin the initialization process?         | yes / no
  Enter the full path to your ansible.cfg file | /path/where/we/will/create/your/file
  Enter the full path to your inventory file  | /path/where/we/will/create/your/file
  Enter your SSH user name for your Sync Gateway nodes  | your SSH user Name
  Enter the full path and name of your SSH key | /path/to/your/sshkey.pem

After completing the prompts, we will create two files ```ansible.cfg``` and ```inventory``` in the specified directories. If these files already exist in the specified directory, we will create a backup of the original file in the format ```ansible.cfg-<epoch-timestamp>``` etc.



## Step2:
Enter the following commands for each file paths that we created for you from the earlier Prompts to ensure your files have been created and are in the proper directory.

```ls -l /path/where/we/created/your/ansible.cfg```<br>
```ls -l /path/where/we/will/created/your/inventory file ```

Confirm your SSH key exists at the path you entered<br>
```ls -l /path/to/your/sshkey.pem```<br>
* Note: you may have to modify the permissions on your SSH file to connect to your target server(e.g. chmod 400 sshkey.pem)


Open the ```ansible.cfg``` file located in the directory we created for you at the path ```/path/where/we/created/your/ansible.cfg``` and confirm all information is correct. If you need to change any values, you can modify the file directly or re-run the init.yml again to correct any mistakes.

Open the ```inventory``` file located ```/path/where/we/created/your/inventory_file```
by entering the following command:

```vi /path/where/we/created/your/inventory_file``` (note: the file name is ```inventory```)

and ensure you have a default set of hosts created for you. It will look something like this:
```
#this is the default inventory file

#sgw-hosts will be the first node you add to the Sync Gateway cluster.
[sgw-hosts]
host1.acme.com os_version=amzn

```

## Step 3:
Open your inventory file and enter your hostnames/IP Address and their respective Sync Gateway nodes. The inventory file uses YAML as do the playbooks, so you will have to pay attention to your spacing as required by YAML.


## Step 4:
Run the following command:<br>
```vi playbooks/group_vars/all.yml```

The ```all.yml``` file contains all global variables that Ansible will use when running the playbooks. This file allows you to configure your Sync Gateway nodes and many other settings within the product.

## Step 5:
To use the default ```all.yml``` configurations, you may leave the file in tact and execute the playbooks without making any additional changes. If you want/need specific configuration, you may change those at anytime.

To install Sync Gateway to your target nodes, execute the following:<br>
```ansible-playbook _RunAllPlaybooks.yml```

NOTE: This will install Sync Gateway Server on the specified target nodes and it is recommended that you test this a few times in a lower environment to ensure you have your cluster set up as expected.

## Uninstall Sync Gateway
To uninstall Sync Gateway from the specified target nodes, execute the following: ```ansible-playbook playbooks/uninstall/___Uninstall-sync-gateway.yml```

NOTE: This will uninstall Sync Gateway Server from any target node(s) you specify in your ```inventory``` file, so you should pay close attention if you decide to run this playbook! This is handy if you are testing these playbooks and want to modify your configuration without having to uninstall manually.

## Global Variable Definitions:
Host Services                | Description
----------------------------|-----------------------------
operating_system.update_packages        | yum updates
operating_system.ping_nodes | runs ping command
ntp_server.install  | installs NTP Server
ntp_server.time_zone  |  sets timezone locale
ntp_server.ntp_host  |  NTP host
sync_gateway_version.build  | Sync Gateway version
sync_gateway_version.url  |  URL to RPM
sync_gateway_version.dest_dir  | directory to store downloaded RPM file  
sync_gateway_version.rpm_name  | name of the RPM for Sync Gateway
