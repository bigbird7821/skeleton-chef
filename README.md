Table of Contents for skeleton-chef
=================
Skeleton chef client server on OEL6 with total of 6 servers in the cluster: a chef dk, a chef server, and 4 clients

   * [skeleton-chef](#skeleton-chef)
      * [Highlights beneath the directory structure:](#highlights-beneath-the-directory-structure)
      * [The directory structure](#the-directory-structure)
      * [Setup instructions](#setup-instructions)

## Highlights beneath the directory structure:
* **applications/chef/apps/first_cookbook** is the chef cookbook shared amongst all the nodes
* **applications/chef/environments** are where all the variables for the application setup are extracted
* **applications/chef/{roles,plays}** and **common/roles** contain all the ansible configurations.  To drill from top to bottom, start with **applications/chef/plays/install.yml** and then **configure.yml**.
* Design decisions are documented using the Lightweight Architecture Decision Records, as used by Thoughtworks, beneath the **docs**
* The vagrant base boxes beneath **base\_boxes** must be created BEFORE doing a "vagrant up" beneath the **applications/chef** directory.  Start first with **base\_boxes/oel6base**: (1) Go to **base\_boxes/oel6base** and run "vagrant up"; (2) Run the createNewBasebox.sh as soon as the oel6base has started up.  This script will create a local vagrant base box called "oel6base" that subsequent "vagrant up" actions will use.  Then go on to complete the rest of the base_boxes in the same
* Only after the base_boxes have all been created, then (1) go to **applications/chef** and and run "vagrant up".  This will use all the newly created base boxes and boot up 6 VMs.  machine1 is the chefdk; machine2 is the chef server; the rest are chef clients.  The last server machine6 is the ansible controller
* Log onto the ansible controler machine6 and configure and test the chef multi-node environment:  (1) go to ./applications/chef (2) vagrant up --provision (3) vagrant ssh machine6 (4) cd /vagrant/plays (5) ansible-playbook --diff -vv install.yml; (6) ansible-playbook --diff -vv configure.yml; (7) ansible-playbook test.yml.

The test.yml should return without error proving that the environment has been successfully configured.

## The directory structure
```
├── applications
│   └── chef
│       ├── apps
│       │   └── first_cookbook
│       │       ├── cookbooks
│       │       └── test
│       ├── environments
│       │   └── dev
│       │       ├── ansible.cfg
│       │       └── inventory
│       │           ├── aaa-group-definitions
│       │           ├── aaa-machine-definitions
│       │           ├── group_vars
│       │           ├── host_vars
│       │           └── hosts
│       ├── plays
│       │   ├── configure.yml
│       │   ├── install.yml
│       │   ├── ping.yml
│       │   └── test.yml
│       ├── roles
│       │   ├── chef-client-configurator
│       │   ├── chef-client-installer
│       │   ├── chef-dk-configurator
│       │   ├── chef-dk-installer
│       │   ├── chef-server-configurator
│       │   ├── chef-server-installer
│       │   ├── crontab-utilities-installer
│       │   └── dnsmasq-configurator
│       ├── scripts
│       │   └── doCopyOfSshKeys.sh
│       └── Vagrantfile
├── base_boxes
│   ├── oel6base
│   │   ├── ansible
│   │   ├── createNewBaseBox.sh
│   │   └── Vagrantfile
│   ├── oel6base.chefclient
│   │   ├── ansible
│   │   ├── createNewBaseBox.sh
│   │   └── Vagrantfile
│   ├── oel6base.chefdk
│   │   ├── ansible
│   │   ├── createNewBaseBox.sh
│   │   └── Vagrantfile
│   └── oel6base.chefserver
│       ├── ansible
│       ├── createNewBaseBox.sh
│       └── Vagrantfile
├── common
│   ├── plays
│   └── roles
│       ├── common-utility-installer
│       ├── dnsmasq-installer
│       ├── epel-6.8-repository-installer
│       ├── function
│       │   └── ansible-test-runner
│       ├── proxy-configurator
│       └── vagrant-base-cleaner
├── directories.txt
├── doc
│   └── adr
│       ├── 0001-record-architecture-decisions.md
│       ├── 0002-separate-the-ansible-skeleton-application-from-the-common-ansible-roles-and-plays.md
│       ├── 0003-use-ansible-inventory-directory-with-empty-group-definitions.md
│       └── 0004-pick-up-the-http-proxy-from-the-environment-and-inject-this-via-the-proxy-role.md
├── files.txt
└── README.md
```

## Setup instructions
* Create the vagrant base boxes beneath **base\_boxes**:
    * Go to **base\_boxes/oel6base**
    * vagrant up
    * Run the createNewBasebox.sh as soon as the oel6base has started up.  This script will create a local vagrant base box called "oel6base" that subsequent "vagrant up" actions will use.  
    * Repeat the above steps for each of the other base boxes:  **base\_boxes/oel6base-chefclient**, **base\_boxes/oel6base-chefdk**, and **base\_boxes/oel6base-chefserver**


* Only after the base_boxes have all been created, then 
    * Go to **applications/chef**
    * vagrant up --provision
* SSH onto machine6 the ansible controller:
    * vagrant ssh machine6
    * cd /vagrant/plays
    * ansible-playbook --diff -vv install.yml
    * ansible-playbook --diff -vv configure.yml
    * ansible-playbook test.yml

There should be no error in either install.yml or configure.yml.  The test.yml verifies that all chef nodes are wired successfully together