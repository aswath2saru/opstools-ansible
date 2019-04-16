Overview
--------

The `opstools-ansible <https://github.com/larsks/opstools-ansible/>`__
project is a collection of Ansible roles and playbooks that will
configure an environment that provides centralized logging and analysis,
availability monitoring, and performance monitoring.

Using opstools-ansible
----------------------

Before you begin
++++++++++++++++

Before using the opstools-ansible playbook, you will need to deploy one
or more servers on which you will install the opstools services. These
servers will need to be running either CentOS 7 or RHEL 7 (or a
compatible distribution).

These playbooks will install packages from a number of third-party
repositories. The opstools-ansible developers are unable to address
problems with the third party packaging (other than via working around
problems in our playbooks).

Creating an inventory file
++++++++++++++++++++++++++

Create an Ansible inventory file ``inventory/hosts`` that defines your
hosts and maps them to host groups declared in ``inventory/structure``.
For example:

::

    server1 ansible_host=192.0.2.7 ansible_user=centos ansible_become=true
    server2 ansible_host=192.0.2.15 ansible_user=centos ansible_become=true

    [am_hosts]
    server1

    [pm_hosts]
    server2

    [logging_hosts]
    server2

There are three high-level groups that can be used to control service
placement:

-  ``am_hosts``: The playbooks will install availability monitoring
   software (Sensu, Uchiwa, and support services) on these hosts.

 - ``pm_hosts``: The playbooks will install performance monitoring
   software (Gnocchi or graphite)

-  ``logging_hosts``: The playbooks will install the centralized logging
   stack (Elasticsearch, Kibana, Fluentd) on these hosts.

While there are more groups defined in the ``inventory/structure`` file,
use of those for more granular service placement is neither tested nor
supported at this time.

You can also run post-install playbook after overcloud deployment to
finish server side configuration dependent on the information about the
overcloud. For that you will need to add undercloud host to the
inventory. So for example, after deploying overcloud via
tripleo-quickstart tool, you should add something like following to the
inventory file before running the playbook:

undercloud\_host ansible\_user=stack ansible\_host=undercloud
ansible\_ssh\_extra\_args='-F "/root/.quickstart/ssh.config.ansible"'

