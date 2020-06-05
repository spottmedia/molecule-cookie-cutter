# Molecule Cookie Cutter

To get you easily kick-started with some of the most rudimentary infra setups.

Despite the project's name suggestion, we support creating single instance/cluster for each scenario defined at this point. 
Not exactly cookie cutter yet, but hopefully we'll get there at some point. With a pinch of scripting.

Please do use those pre-made containers to your aid, and we'll keep expanding and tweaking as we go and as needs arise.

This repository is also an integration-testing ground for our set of ansible roles:

https://github.com/spottmedia/shareable-ansible-toolkit

which is one of the core dependency for all the scenarios defined here.

__The main goal__ of this project is to accumulate as many useful molecule recipes as possible. 
It's mostly stuff we are using on daily basis across the board so should be easy to use by non-ops team members, 
but complete enough to not be lacking some essential services or tools.

#####  PRs and suggestions highly appreciated ! 
 

### LXD

The recommended lxd version is 4+ (check with `lxd --version`), and for that you might need to install your lxd from snap. ie.

    sudo apt purge lxd* lxc*  <-- this is potentially destructive operation for existing containers! Make sure you know what you are doing
    sudo snap install lxd
    
Ubuntu 20.04+ should have the fresh enough version out of the box

#### LXD INIT

If not LXD init'ed before (and only if you've not ran it before!), aaand assuming you're ok with dir-based storage:

    lxd init --auto --storage-backend=dir
    
if you know what you are doing, for more bespoke initialization just invoke `lxd init` and go through steps yourself, 

ALSO if running lxd on a vanilla system please make sure you have invoked:

    sudo usermod --append --groups lxd $USER

then: logout -> login from the system for the new group to get applied


## Molecule Converge

select your desired scenario and simply use it to create, converge and verify your container(s).

### Provisioning Bootstrap
Remember to bootstrap into virtualenv with libraries in it. All can be solved with:

    cd provisioning
    ./bootstrap_provisioning.sh  # do the bootstrap only ONCE (or when resetting the virtual env for some reason)
    source provisioningenv/bin/activate

### Convergence

to bring up the lxd-lamp scenario:

    SHARED_FOLDER=absolute_path molecule converge -s lxd-lamp

### Integration test
no need to provide env variables for verification as the machine is already provisioned

    molecule verify -s lxd-lamp
    

## Customization

Beyond specific scenarios' instructions feel free to tweak any of the bits defined in scenarios and inventories 
to suit your needs. 

## Working scenarios

### lxd-lamp

* apache  # latest from ppa:ondrej/apache2
* mysql   # root password randomly generated and put into ~/.my.cnf file
* php7.3 + fpm
* ufw with some allow rules for the aforementioned services added (dev or not let's keep it all nice and tight)
 
#### Configurable ENV variables:

    SHARED_FOLDER : (required) an_abolute_path_to_be_shared
    LXD_POOL      : (optional) name_of_lxd_pool_to_user (or leave blank if the storage pool's name is 'default')
    
#### Configurable HOST variables:

    cp inventories_override/toffee-lamp.template.yml inventories_override/toffee-lamp.yml
    nano inventories_override/toffee-lamp.yml

then consult the content of the file and make adjustments