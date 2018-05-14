Table of Contents for skeleton-chef
=================

   * [Table of Contents for skeleton-chef](#table-of-contents-for-skeleton-chef)
      * [Design Highlights of the project:](#design-highlights-of-the-project)
      * [Setup instructions](#setup-instructions)
      * [Outstanding Work](#outstanding-work)


## Design Highlights of the project:
* Design decisions are documented beneath the **./doc/adr** using [Lightweight Architecture Decision Records](https://www.thoughtworks.com/radar/techniques/lightweight-architecture-decision-records), as used by Thoughtworks.
* The virtualbox base boxes beneath the **./base\_boxes** directory must be created BEFORE starting up the chef cluster under the **./applications/chef** directory.  See the [Setup instructions](#setup-instructions) for more information. 
* To understand the infrastructure-as-code start with **./applications/chef/plays/install.yml**, **./applications/chef/plays/configure.yml** and then **./applications/chef/plays/test.yml**. 
* **./applications/chef/apps/first_cookbook** is the chef cookbook shared amongst all the nodes

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
│       │           ├── group_vars
│       │           ├── host_vars
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
│       └── Vagrantfile
├── base_boxes
│   ├── oel6base
│   ├── oel6base.chefclient
│   ├── oel6base.chefdk
│   └── oel6base.chefserver
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
├── doc
│   └── adr
└── README.md
```

## Setup instructions
* Create the vagrant base boxes beneath **./base\_boxes**:
    * Go to **./base\_boxes/oel6base**
    * vagrant up
    * Run the createNewBasebox.sh as soon as the oel6base has completed without error.  This script will create a local vagrant base box called "oel6base" that subsequent "vagrant up" actions will use.  
    * Repeat the above steps for each of the other base boxes:  **./base\_boxes/oel6base-chefclient**, **./base\_boxes/oel6base-chefdk**, and **./base\_boxes/oel6base-chefserver**

* Only after the base_boxes have all been created, then 
    * Go to **./applications/chef**
    * vagrant up --provision

* SSH onto machine6 the ansible controller:
    * vagrant ssh machine6
    * cd /vagrant/plays
    * ansible-playbook --diff -vv install.yml
    * ansible-playbook --diff -vv configure.yml
    * ansible-playbook test.yml

    There should be no error in either install.yml or configure.yml.  The test.yml verifies that all chef nodes are wired successfully together

## Outstanding Work
- [ ] Continue the Learn Chef Series focussing on adding the various chef testing tools and examples
- [ ] Move the ruby PATH configuration into a common place and distinguish between DK and client/server paths
- [ ] Fix the test.yml so that the expected is dynamically generated for the "knife client list"
- [ ] Create an issue list for this repository
- [ ] Create a project board for managing the roadmap
