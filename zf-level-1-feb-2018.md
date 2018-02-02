# ZEND FRAMEWORK FUNDAMENTALS I -- Course Notes

## DAY ZERO
* Sample out for `vagrant up`
```
$ vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Box 'datashuttle/RWZ-Ubuntu-16.04LTS-DTP' could not be found. Attem                                                                     pting to find and install...
    default: Box Provider: virtualbox
    default: Box Version: 1.0.2
==> default: Loading metadata for box 'datashuttle/RWZ-Ubuntu-16.04LTS-DTP'
    default: URL: https://vagrantcloud.com/datashuttle/RWZ-Ubuntu-16.04LTS-DTP
==> default: Adding box 'datashuttle/RWZ-Ubuntu-16.04LTS-DTP' (v1.0.2) for provi                                                                     der: virtualbox
    default: Downloading: https://vagrantcloud.com/datashuttle/boxes/RWZ-Ubuntu-                                                                     16.04LTS-DTP/versions/1.0.2/providers/virtualbox.box
==> default: Box download is resuming from prior download progress
    default:
==> default: Successfully added box 'datashuttle/RWZ-Ubuntu-16.04LTS-DTP' (v1.0.                                                                     2) for 'virtualbox'!
==> default: Importing base box 'datashuttle/RWZ-Ubuntu-16.04LTS-DTP'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'datashuttle/RWZ-Ubuntu-16.04LTS-DTP' is up to date                                                                     ...
==> default: Setting the name of the VM: ZFF1 - Provisioning
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 80 (guest) => 8084 (host) (adapter 1)
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Running 'pre-boot' VM customizations...
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default:
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default:
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
    default: The guest additions on this VM do not match the installed version o                                                                     f
    default: VirtualBox! In most cases this is fine, but in rare cases it can
    default: prevent things such as shared folders from working properly. If you                                                                      see
    default: shared folder errors, please make sure the guest additions within t                                                                     he
    default: virtual machine match the version of VirtualBox you have installed                                                                      on
    default: your host and reload your VM.
    default:
    default: Guest Additions Version: 5.1.30
    default: VirtualBox Version: 5.2
==> default: Setting hostname...
==> default: Mounting shared folders...
    default: /home/vagrant/Shared => D:/VM/ZF-Level-1
==> default: Running provisioner: shell...
    default: Running: C:/Users/george/AppData/Local/Temp/vagrant-shell20180202-2                                                                     760-1mlennp.sh
    default: Provisioning course projects...
    default: [DONE: Provisioning course project(s)]
==> default: Running provisioner: shell...
    default: Running: C:/Users/george/AppData/Local/Temp/vagrant-shell20180202-2                                                                     760-1vxbs0m.sh
    default: Provisioning virtual hosts for the project...
    default: [DONE: Provisioning virtual hosts]
==> default: Running provisioner: shell...
    default: Running: inline script
==> default: Running provisioner: shell...
    default: Running: C:/Users/george/AppData/Local/Temp/vagrant-shell20180202-2                                                                     760-13c1p1k.sh
    default: Bootstrap the course MySql database...
    default: Creating database for guestbook and populating
    default: Creating database for onlinemarket and populating
    default: [DONE: Provisioning course MySQL DB]
==> default: Running provisioner: shell...
    default: Running: C:/Users/george/AppData/Local/Temp/vagrant-shell20180202-2                                                                     760-s5oyuq.sh
    default: Provisioning environment...
    default: [DONE: Provisioning environment]
==> default: Running provisioner: shell...
    default: Running: C:/Users/george/AppData/Local/Temp/vagrant-shell20180202-2                                                                     760-1tlezmr.sh
    default: Provisioning general cleanup detail...
    default: [DONE: Provisioning cleanup]
```

## VM NOTES

### guestbook project
* Need to update the database structure:
  * From the browser go to `http://localhost/`
  * Select `phpMyAdmin`
  * Select `guestbook`
  * Select `SQL`
  * Paste in the following:
```
CREATE TABLE `entry` (
  `id` int(10) UNSIGNED NOT NULL AUTO_INCREMENT,
  `name` varchar(255) NOT NULL,
  `email` varchar(255) NOT NULL,
  `website` varchar(255) NOT NULL,
  `message` text NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```
* To test:
  * From the browser: `http://guestbook/guestbook`
  * Enter the requested info and post
  * Check to make sure your entry has been posted