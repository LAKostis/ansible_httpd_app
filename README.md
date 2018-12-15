Simple httpd webapp deploy and rolling updates procedure
---------------------------------------------------------

- Requires Ansible 2+
- CentOS 7.x
- httpd
- haproxy
- php

This app allows to install simple webapp to multiple backends (httpd) with load-balancer (haproxy).

### Features

- allows to roll out new version, roll back to older version, re-deploy current version.
- application can be pulled by the server on which the application is to be installed. 
- not pushing from a central location.

### Initial site setup

Example configuration (see Vagrantfile) consists of 3 backends and one load-balancer. But it could be extended to any number of backends and lb accroding customer needs. Sensitive vars are hidden in vault which should be opened by supplied password.

Command to deploy a site:

        ansible-playbook --ask-vault-pass -i hosts site.yml

The deployment can be verified by accessing the IP address of your load balancer host in a web browser: 
http://<ip-of-lb>:8888. Reloading the page should have you hit different webservers.

### Removing and Adding a Node

Removal and addition of nodes to the cluster is as simple as editing the hosts inventory and re-running:

        ansible-playbook --ask-vault-pass -i hosts site.yml

### Rolling Update

Rolling updates are the preferred way to update the web server software or
deployed application, since the load balancer can be dynamically configured
to take the hosts to be updated out of the pool. This will keep the service
running on other servers so that the users are not interrupted.

In this example the hosts are updated in serial fashion, which means that
only one server will be updated at one time. If you have a lot of web server
hosts, this behaviour can be changed by setting the 'serial' keyword in
rolling_update.yml file.

Once the code has been updated in the source repository for your application
which can be defined in the group_vars/all file, execute the following
command:

         ansible-playbook --ask-vault-pass -i hosts rolling_update.yml

You can optionally pass: -e webapp_version=xxx to the rolling_update
playbook to specify a specific version of the example webapp to deploy.

### Thanks
This app heavily based on https://github.com/ansible/ansible-examples/tree/master/lamp_haproxy
