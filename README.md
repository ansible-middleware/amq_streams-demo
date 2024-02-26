# README

TODO

## Prepare the controller

### Install Ansible

The system running Ansible needs to have the appropriate package install. In case of RHEL9 system, you'll need first to enable the appropriate subscription ansible-automation-platform. On x86_64 system, this can be achieved with the following command, run as root:

    # subscription-manager repos --enable=ansible-automation-platform-2.4-for-rhel-9-x86_64-rpms

Then, install Ansible using DNF:

    # dnf install ansible-core

### Configure Ansible to use Ansible Automation Hub

Please refer to the following documentation on [how to configure Ansible to Automation Hub](https://access.redhat.com/documentation/en-us/red_hat_jboss_web_server/6.0/html/installing_jboss_web_server_by_using_the_red_hat_ansible_certified_content_collection/install_collection) and then execute the following command to install the Red Hat provided collection for AMQ Streams:

    $ ansible-galaxy collection install redhat.amq_streams

## Prepare targets

For any systems managed by Ansible, SSHd must be running and a SSH pubkey, associated to root user needs to have been deployed to ensure that Ansible can connect ot it. For RHEL system, the required subscription for AMQ Streams (and RHEL) needs to be enabled.

## Inventory

For Ansible to know which systems to manage an inventory needs to be provided. In the context of this demo, this list must follows a few naming conventions. Host running AMQ Streams Broker should belong to the 'brokers' group while the ones running Zookeepers should belong to 'zookeepers', and the same logic applies for Cruise Control.

    [all]

    [brokers]
    broker.example.com

    [zookeepers]
    zk.example.com

    [cruise_controllers]
    cc.example.com

For testing purpose, you can use localhost, which also remove the need to setup SSHd (as there will be no SSH connection between Ansible and localhost):

    [all]
    localhost ansible_connection=local

    [brokers]
    localhost ansible_connection=local

    [zookeepers]
    localhost ansible_connection=local

## Running Ansible to setup Zookeepers

You can now run the playbooks to deploy Zk on the targets belonging to the 'zookeepers' group:

    $ ansible-playbook -i inventory playbooks/zookeepers -e amq_streams_zookeeper_auth_pass=<password>

## Running Ansible to setup Brokers

You can now run the playbooks to deploy Zk on the targets belonging to the 'zookeepers' group:

    $ ansible-playbook -i inventory playbooks/brokers -e amq_streams_zookeeper_auth_pass=<password>
