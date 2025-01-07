OpenShift Bastion Node Installer
=========

Install the dns, loadbalancer, http server and generate the ignition files automatically.

P.S.
For install the whole OpenShift Cluster automatically, you can use the following project
- https://github.com/RedHatOfficial/ocp4-vsphere-upi-automatio

Requirements
------------

Ansible 2.6+
Ansible Navigator

Role Variables
--------------
    # enable or disable
    online: true
    firewalld_disable: true
    selinux_disable: true

    # DNS
    dns_configure: true
    interface: ens33
    dns_upstream: 8.8.8.8

    # DNS check
    dns_check: true
    dns_ip: 172.20.11.50

    # LB
    haproxy_configure: true

    # Registry
    registry_configure: false
    mirrorRegistryDir: /root/install_source/mirror-registry.tar.gz
    quayRoot: /mirror-registry
    quayStorage: /mirror-registry/storage
    registryPassword: P@ssw0rd

    # OCP
    # define the cluster name for cluster
    clusterName: ocp4
    # define the base domain for cluster
    baseDomain: demo.lab
    # define the resource files absolute path
    sshKeyDir: /root/.ssh/id_rsa.pub
    ocpInstallDir: /root/install_source/openshift-install-rhel9-amd64.tar.gz
    ocpClientDir: /root/install_source/openshift-client-linux-amd64-rhel9-4.16.26.tar.gz
    # for online install
    pullSecretDir: /root/install_source/pull-secret.txt

    # Mirroring from disk to mirror
    mirror: false
    ocmirrorSource: /root/install_source/oc-mirror.rhel9.tar.gz
    imageSetFile: /root/install_source
    reponame: ocp416

    # Nodes
    bastion:
      name: bastion
      ip: 172.20.11.50
    bootstrap:
      name: bootstrap
      ip: 172.20.11.60
    master:
    - name: master01
      ip: 172.20.11.51
    - name: master02
      ip: 172.20.11.52
    - name: master03
      ip: 172.20.11.53
    master:
    - name: infra01
      ip: 172.20.11.54
    - name: infra02
      ip: 172.20.11.55
    - name: infra03
      ip: 172.20.11.56
    worker:
    - name: worker01
      ip: 172.20.11.57
    - name: worker02
      ip: 172.20.11.58
    - name: worker03
      ip: 172.20.11.59


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: all
      remote_user: root
      vars_files:
      - env.yml
      roles:
      - ocp_bastion_installer

Example Inventory
----------------

    bastion.ocp.ansible.lab  ansible_host=172.20.11.50

How to use
----------------

Generate an SSH key pair

    ssh-keygen

Copy the key to bastion

    ssh-copy-id root@<bastion>

Run playbook

    ansible-navigator run --eei quay.io/irislee/ocp-day1:v3-39 -i inventory -mstdout install.yml
