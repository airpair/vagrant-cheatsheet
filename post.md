
A 3 minutes introduction to setting up your Vagrant Machine. This very short tutorial will work you through how to configure your Vagrantfile for various VM providers in the form of a cheatsheet. 

Uninstalling Vagrant
    - On Windows, uninstall using the add/remove programs section of the control panel.
    - On Mac OS X, remove the /Applications/Vagrant directory and the /usr/bin/vagrant file.
    - On Linux, remove the /opt/vagrant directory and the /usr/bin/vagrant file.
    - On every platform, remove the ~/.vagrant.d directory to delete the user data.
    
Note: the ‘$’ before the commands signifies a set of commands to run on your terminal. Please, do not type ‘$’ as part of the command.

‘#’ represents a comment/explanation of what the command does

Commands:
--------

    $ vagrant box add NAME URL
    $ vagrant box list
    $ vagrant box remove NAME PROVIDER
    $ vagrant box repackage NAME PROVIDER
    $ vagrant box add precise32 http://files.vagrantup.com/precise32.box

 — -
    # vagrant up — starts vagrant environment (also provisions only on the FIRST vagrant up)

    $ vagrant up — provider=virtualbox
    $ vagrant up — provider=vmware_fusion
    $ vagrant up — provider=vmware_workstation
    $ vagrant up — provider=aws
    
 — -

    # vagrant destroy — stops and deletes all traces of the vagrant machine
    
    $ vagrant destoy
    $ vagrant destroy -f
    $ vagrant destrroy — force
    -f or — force is for : Don’t ask for confirmation before destroying.
    
 — -

# Other commands
    $ vagrant init # Initializes the current directory to be a Vagrant environment by creating an initial Vagrantfile if one doesn’t already exist
    
    $ vagrant status — outputs status of the vagrant machine
    
    $ vagrant reload — restarts vagrant machine, loads new Vagrantfile configuration
    
    $ vagrant provision — forces reprovisioning of the vagrant machine

# Tips

    $ vagrant -v — Get the vagrant version
    $ vagrant global-status — outputs status of all vagrant machines
    $ vagrant suspend — Suspends a virtual machine (remembers state)
    $ vagrant resume — Resume a suspended machine (vagrant up works just fine for this as well)
    $ vagrant push — Yes, vagrant can be configured to deploy code!
    $ vagrant up — provision | tee provision.log — Runs vagrant up, forces provisioning and logs all output to a file
    
 — -

    # vagrant halt — stops the vagrant machine
    $ vagrant halt
    $ vagrant halt -f
    $ vagrant halt — force
    -f or — force is for : Don’t attempt to gracefully shut down the machine. Pull out the power cord.
    
 — -

    # If a first argument is given, it will prepopulate the config.vm.box setting in the created Vagrantfile.
    # If a second argument is given, it will prepopulate the config.vm.box_url setting in the created Vagrantfile.
    
    $ vagrant init [box-name] [box-url]
    
 — -

    $ vagrant package
    
    — base NAME — Instead of packaging a VirtualBox machine that Vagrant manages, this will package a VirtualBox machine that VirtualBox manages. NAME should be the name or UUID of the machine from the VirtualBox GUI.
    — output NAME — The resulting package will be saved as NAME. By default, it will be saved as package.box.
    — include x,y,z — Additional files will be packaged with the box. These can be used by a packaged Vagrantfile (documented below) to perform additional tasks.
    — vagrantfile FILE — Packages a Vagrantfile with the box, that is loaded as part of the Vagrantfile load order when the resulting box is used.
    
 — -

    $ vagrant plugin
    $ vagrant plugin install
    $ vagrant plugin license
    $ vagrant plugin list
    $ vagrant plugin uninstall
    
 — -

    # vagrant provision — debug — Use the debug flag to increase the verbosity of the output
    $ vagrant provision — provision-with x,y,z
    This will only run the given provisioners. For example, if you have a :shell and :chef_solo provisioner and run vagrant provision — provision-with shell, only the shell provisioner will be run.
    
 — -

    # vagrant reload — provision — Restart the virtual machine and force provisioning
    $ vagrant reload
    — no-provision — The provisioners will not run.
    — provision-with x,y,z — This will only run the given provisioners. For example, if you have a :shell and :chef_solo provisioner and run vagrant provision — provision-with shell, only the shell provisioner will be run.
    
 — -

    # vagrant ssh — connects to machine via SSH
    $ vagrant ssh
    -c COMMAND or — command COMMAND — This executes a single SSH command, prints out the stdout and stderr, and exits.
    -p or — plain — This does an SSH without authentication, leaving authentication up to the user.
    
 — -

    $ vagrant ssh-config
    This will output valid configuration for an SSH config file to SSH into the running Vagrant machine from ssh directly (instead of using vagrant ssh).
    — host NAME — Name of the host for the outputted configuration.
    
 — -

Path
----

if vagrant is run in `/home/path/to/foo`, it will search the following paths in order for a Vagrantfile, until it finds one:

    /home/path/to/foo/Vagrantfile
    /home/path/to/Vagrantfile
    /home/path/Vagrantfile
    /home/Vagrantfile
    /Vagrantfile
    VAGRANT_CWD # environment variable to set the starting directory where Vagrant looks for a Vagrantfile

Configs
---------

VagrantFile Configs

    Vagrant.configure(“2”) do |config| config.vm.box = “precise32”
    end
# Load order of Vagrantfile (newest overrides the older ones)
    1. Built-in default Vagrantfile that ships with Vagrant. This has default settings
    2. Vagrantfile packaged with the box that is to be used for a given machine.
    3. Vagrantfile in your Vagrant home directory (defaults to ~/.vagrant.d). This lets you specify some defaults for your system user
    4. Vagrantfile from the project directory. This is the Vagrantfile that you’ll be modifying most of the time.
    
# Machine Settings

    - config.vm.box — This configures what box the machine will be brought up against. The value here should match one of the installed boxes on the system.
    - config.vm.box_url — The URL that the configured box can be found at. If the box is not installed on the system, it will be retrieved from this URL when vagrant up is run.
    - config.vm.graceful_halt_retry_count — The number of times to retry gracefully shutting down the system when vagrant halt is called. Defaults to 3.
    - config.vm.graceful_halt_retry_interval — The amount of time in between each retry of attempting to shut down, in seconds. Defaults to 1 second.
    - config.vm.guest — The guest OS that will be running within this machine. This defaults to :linux, and Vagrant will auto-detect the proper distro. Vagrant needs to know this information to perform some guest OS-specific things such as mounting folders and configuring networks.
    - config.vm.hostname — The hostname the machine should have. Defaults to nil. If nil, Vagrant won’t manage the hostname. If set to a string, the hostname will be set on boot.
    - config.vm.network — Configures networks on the machine.
    - config.vm.provider — Configures provider-specific configuration, which is used to modify settings which are specific to a certain provider. If the provider you’re configuring doesn’t exist or is not setup on the system of the person who runs vagrant up, Vagrant will ignore this configuration block. This allows a Vagrantfile that is configured for many providers to be shared among a group of people who may not have all the same providers installed.
    - config.vm.provision — Configures provisioners on the machine, so that software can be automatically installed and configured when the machine is created.
    - config.vm.synced_folder — Configures synced folders on the machine, so that folders on your host machine can be synced to and from the guest machine.
    - config.vm.usable_port_range — A range of ports Vagrant can use for handling port collisions and such. Defaults to 2200..2250.
    SSH Settings
    - config.ssh.default.host — This sets the default host that Vagrant will use for SSH. This is not set by default because the providers usually detect the proper host. Provider-detected hosts will override this. To force a certain host, use config.ssh.host.
    - config.ssh.default.port — This sets the default port that Vagrant will use to connect to the machine via SSH. This is not set by default because the providers usually detect the proper port. Provider-detected ports will override this. To force a certain port, use config.ssh.port.
    - config.ssh.default.private_key_path — This sets the default private key to use for authenticating with SSH. By default this is set to the insecure private key that ships with Vagrant, since that is what most public boxes use. A provider-detected private key will override this setting. To force a certain private key, use config.ssh.private_key_path. The private key format should be an OpenSSH private key format (as opposed to PuTTY keys and such).
    - config.ssh.default.username — This sets the default username that Vagrant will SSH as, if no other username can be detected. This is set to “vagrant” by default, since that is what most public boxes use. Alternate providers may detect alternate usernames though, in which case this will be overriden. To force a certain username, use config.ssh.username.
    - config.ssh.host — The same as config.ssh.default.host, except this will override any detected host and will always be used.
    - config.ssh.port — The same as config.ssh.default.port, except this will override any detected port and will always be used.
    - config.ssh.private_key_path — The same as config.ssh.default.private_key_path, except this will override any detected private key and will always be used.
    - config.ssh.username — The same as config.ssh.default.username, except this will override any detected username and will always be used.
    - config.ssh.guest_port — The port on the guest that SSH is running on. This is used by some providers to detect forwarded ports for SSH. For example, if this is set to 22 (the default), and Vagrant detects a forwarded port to port 22 on the guest from port 4567 on the host, Vagrant will attempt to use port 4567 to talk to the guest if there is no other option.
    - config.ssh.keep_alive — If true, a “keep-alive” packet will be sent every 5 seconds on the SSH connection. This avoids the connection closing on long-running tasks. This defaults to true.
    - config.ssh.max_tries — Maximum attempts to SSH while waiting for the machine to boot. Default is 100.
    - config.ssh.timeout — Maximum time to wait while attempting to make a single connection via SSH before timing out. Default is 30 seconds.
    - config.ssh.forward_agent — If true, agent forwarding over SSH connections is enabled. Defaults to false.
    - config.ssh.forward_x11 — If true, X11 forwarding over SSH connections is enabled. Defaults to false.
    - config.ssh.shell — The shell to use when executing SSH commands from Vagrant. By default this is bash -l. Note that this has no effect on the shell you get when you run vagrant ssh. This configuration option only affects the shell to use when executing commands internally in Vagrant.

    
    
# Vagrant Settings
    config.vagrant.host — This sets the type of host machine that is running Vagrant. By default this is :detect, which causes Vagrant to auto-detect the host. Vagrant needs to know this information in order to perform some host-specific things, such as preparing NFS folders if they’re enabled. You should only manually set this if auto-detection fails.
    
    
    
# Provider Settings
    - Multiple config.vm.provider blocks can exist to configure multiple providers.

    Vagrant.configure(“2”) do |config| # … (other config)
    config.vm.provider :virtualbox do |vb| vb.customize [“modifyvm”, :id, “ — cpuexecutioncap”, “50”] end
    end
    


VirtualBox configuration
------------------------

# Enable GUI



    config.vm.provider “virtualbox” do |v| v.gui = true
    end
    


# Virtual Machine Name



    config.vm.provider “virtualbox” do |v| v.name = “my_vm”
    end 



# Customization

The following settings are in compliance with VBOXMANAGE (http://www.virtualbox.org/manual/ch08.html)



    config.vm.provider “virtualbox” do |v| v.customize [“modifyvm”, :id, “ — cpuexecutioncap”, “50”]
    end
      


VMWare Fusion/workstation configuration
---------------------------------------

# Installation


    $ vagrant plugin install vagrant-vmware-fusion
    …
    $ vagrant plugin license vagrant-vmware-fusion license.lic
    …
    


Custom VMware install location (VAGRANT_VMWARE_FUSION_APP environment variable)
-------------------------------------------------------------------------------


    $ export VAGRANT_VMWARE_FUSION_APP=”/Apps/VMware Fusion.app”
    $ vagrant up — provider=vmware_fusion


# Enable GUI


    config.vm.provider “vmware_fusion” do |v| v.gui = true
    end



VMX customization
-----------------

    config.vm.provider “vmware_fusion” do |v| v.vmx[“custom-key”] = “value” v.vmx[“another-key”] = nil
    end
    

Synced Folders
--------------

    - By default Vagrant shares the project directory (the one with VagrantFile) to the /vagrant directory on guest machine.
    - Any file created in /vagrant on the guest machine is automatically copied to the project directory
    
Configuring
-----------


    Vagrant.configure(“2”) do |config| # other config here
    config.vm.synced_folder “src/”, “/srv/website”
    end

# Disabling synced folders

    Vagrant.configure(“2”) do |config| config.vm.synced_folder “src/”, “/srv/website”, disabled: true
    end

# NFS Sharing

    - must have nfsd installed on the host machine
    - during vagrant up sequence user may be prompted for administrative privileges to host machine (via sudo program)

    Vagrant.configure(“2”) do |config| # … config.vm.synced_folder “.”, “/vagrant”, :nfs => true
    end

# Automated Provisioning

    - run a by default shell script bootstrap.sh
    External Scripts

    Vagrant.configure(“2”) do |config| config.vm.box = “precise32” config.vm.provision :shell, :path => “bootstrap.sh”
    end

Hope this helps. Checkout some of my Opensource projects on [GITHUB](http://github.com/ojeng) and [my blog](https://medium.com/@ojengwa).