# ZEND FRAMEWORK FUNDAMENTALS II -- Course Notes

Left off with: http://localhost:8888/#/7

## NOTE To SELF:
* http://localhost:8888/#/4/15: has SharedEventManagerInterface been changed recently?

## Homework
* Tue 20 Feb 2018
  * Lab: Cache
    * Need to assign filesystem rights to "www-data" to the cache directory
```
chown www-data:vagrant /home/vagrant/Zend/workspaces/DefaultWorkspace/onlinemarket.work/data/cache
```
  * Lab: ACL
    * Don't forget to check the time on your VM before setting the ACL time assertion
* Fri 16 Feb 2018
  * Lab: AbstractRestfulController
    * Use the `ListingsTable` and get listings from the Online Market
* Wed 14 Feb 2018
  * Lab: Authentication and Password Hashing
    * Don't forget to install the `zend-authentication` and `zend-crypt` components!
    * If you get this error: `Fatal error: Class 'Login\Model\AbstractModel' not found` you need to add this "use" statement to the `Login\Model\User` class:
```
use Application\Model\AbstractModel;
```
    * If you get this error: `A plugin by the name "flashMessenger" was not found` you need to install the `zend-mvc-plugin-flashmessenger` component
  * Lab: BlockCipher Lab
* Mon 12 Feb 2018
  * Lab: Listener Aggregates
* Fri 9 Feb 2018
  * Lab: Doctrine
  * Lab: Shared Manager
* Wed 7 Feb 2018
  * Lab: Abstract Factories
  * Lab: Delegators

## Q & A
* Q: How can I check if my passwords are good?
* A: See: from Marcin to All Participants: in your free time you can check strenght of your password and time it takes on this site http://password-checker.online-domain-tools.com/


## Errata
* http://localhost:8888/#/3/16: there *is* no Table Module unit!!
* http://localhost:8888/#/3/32: to check your work:
```
http://onlinemarket.work/doctrine/admin
http://onlinemarket.work/doctrine/signup
```
* http://localhost:8888/#/4/4: "return"???
* http://localhost:8888/#/5/9: bindRequiresDn needs an argument in the table
* http://localhost:8888/#/5/40: could make more efficient create Bcrypt() in __construct()
* http://localhost:8888/#/5/42: code block runs off page
```
namespace Login;
// other use statements not shown
use Zend\Db\Adapter\Adapter;
use Zend\Authentication\Adapter\DbTable\CallbackCheckAdapter;
class Module {
    // other methods not shown
    public function getServiceConfig() {
        return [ 'factories' => [
            // other services not shown
            'login-db-adapter' => function ($container) {
                return new Adapter($container->get('local-db-config'));
            },
            'login-auth-adapter' => function ($container) {
                return new CallbackCheckAdapter(
                    $container->get('login-db-adapter'),
                    UsersTable::$tableName,
                    UsersTable::$identityCol,
                    UsersTable::$passwordCol,
                    function ($hash, $password) {
                        if (strlen($hash) == 32) return $hash == md5($password);
                        else return \Login\Security\Password::verify($password, $hash);
                    });
            },
],];}
}
```
* http://localhost:8888/#/5/56: choosig!!!
* http://localhost:8888/#/5/74: missing "Lab:"
* http://localhost:8888/#/6/34: Generate an API which draws from `ListingsTable`
* http://localhost:8888/#/7/5: should also include "if" statement the checks to see if cache is available
* http://localhost:8888/#/7/27: probably could have just injected the cache-adapter: no need for service manager!
* http://localhost:8888/#/7/40: probably should set a specific start/stop time

## Event Manager
* Shared Manager is not automatically associated with a "local" event in ZF 3
  * When creating an event manager instance, use the pre-defined Service Manager service `EventManager`
  * This uses a pre-defined factory which injects the existing Shared Manager into the new instance
* Listener Aggregate Lab
  * Just have the listener log using `error_log()` PHP function
  * Probably stick to the `Events` module; otherwise if you use `MyDoctrine` have the repository trigger
  * `error_log()` by default goes to `/var/log/apache2/error.log`

## Data Modeling
* See: https://www.infoq.com/minibooks/domain-driven-design-quickly

## Service Container
### Abstract Factory
* Example: see in guestbook: AuthOauth\Factory\AdapterAbstractFactory
### Reflect Abstract Factory
* Example: see in guestbook: module.config.php
```
    TableModule\Model\EventTable::class        => ReflectionBasedAbstractFactory::class,
    TableModule\Model\AttendeeTable::class     => ReflectionBasedAbstractFactory::class,
    TableModule\Model\RegistrationTable::class => ReflectionBasedAbstractFactory::class,
```
### Config Abstract Factory
* Example: see guestbook: `/module/Events/config/module.config.php` lines 189 and 202 - 213
### Delegators
* Example: see guestbook: `/module/Events/config/module.config.php` lines 189 and 202 - 213
* Also in guestbook: `\Doctrine\Factory\SignupDelegatorFactory`

## VM UPDATES
* Look at the ACL for Guestbook: logged in admin but can't see Admin Area under Events
* *IMPORTANT* please replace `onlinemarket.work/module/Market/view/partials/item.phtml` with this:
```
<?php
    $locale = \Locale::getDefault();
    switch ($locale) {
        case 'en' :
            $code = 'USD';
            break;
        case 'es' :
        case 'de' :
        case 'fr' :
            $code = 'EUR';
            break;
        default :
            $code = 'GBP';
    }
?>
<div class="span7">
    <style>
    th {
        text-align: right;
    }
    .listingImage {
        float: left;
        width: 40%;
        height: 800px;
    }
    .listingNotes {
        float: left;
        width: 60%;
    }
    .tableSpace {
        width: 100px;
    }
    </style>
    <p>
        <?php if ($this->item) : ?>
        <h3><i><?php echo $this->escapeHtml($this->item->title); ?></i></h3>
        <table width="60%" cellspacing="5px" cellpadding="5px">
            <tr>
                <!-- //*** I18N FORMATTING LAB: display using I18N currency view helper -->
                <td><h4><?php echo number_format($this->item->price, 2); ?></h4></td>
                <td><h4><?php echo $this->escapeHtml($this->item->city); ?></h4></td>
                <td><h4><?php echo $this->escapeHtml($this->item->country); ?></h4></td>
            </tr>
        </table>
        <hr />
        <div class="listingImage">
            <?php $photoFilename = $this->escapeHtml($this->item->photo_filename); ?>
            <?php if (stripos($photoFilename, 'http:') === FALSE) $photoFilename = $this->basePath() . $photoFilename; ?>
            <img src="<?php echo  $photoFilename; ?>" width="200px"/>
            </div>
            <div class="listingNotes">
            <table cellspacing="10px" cellpadding="10px" class="tableClass">
                <!-- //*** TRANSLATION LAB: display using translate view helper -->
                <tr><th>Category</th><td class="tableSpace">&nbsp;</td><td><?php echo $this->escapeHtml($this->item->category); ?></td></tr>
                <tr><th>Posted</th><td class="tableSpace">&nbsp;</td><td><?php echo $this->escapeHtml($this->item->date_created); ?></td></tr>
                <tr><th>Expires</th><td class="tableSpace">&nbsp;</td><td><?php echo $this->escapeHtml($this->item->date_expires); ?></td></tr>
                <tr><th>Name</th><td class="tableSpace">&nbsp;</td><td><?php echo $this->escapeHtml($this->item->contact_name); ?></td></tr>
                <tr><th>Phone</th><td class="tableSpace">&nbsp;</td><td><?php echo $this->escapeHtml($this->item->contact_phone); ?></td></tr>
                <tr><th>Email</th><td class="tableSpace">&nbsp;</td><td><?php echo $this->escapeHtml($this->item->contact_email); ?></td></tr>
            </table>
        </div>
        <hr>
        <p><?php echo $this->escapeHtml($this->item->description); ?></p>
        <hr />
        <?php else : ?>
            Unable to find listing!
        <?php endif; ?>
    </p>
</div>
```

## LAB NOTES

### LAB: LISTENER AGGREGATE
  * When signing up for an event got this error:
```
Call to a member function filter() on null
```
  * Need to inject the events data filter into the SignupController:
    * In `Events\Controller\SignupController` add the following:
```
    protected $filter;
    public function setFilter($filter)
    {
        $this->filter = $filter;
    }
```
    * Better yet: use a trait ... because you'll need to also inject the event manager in the RegistrationTable model class.
    * In `Events\Module` add `setFilter` to the factory which produces the SignupController as follow:
```
    Controller\SignupController::class => function ($container, $requestedName) {
        $controller = new $requestedName();
        $controller->setEventTable($container->get(Model\EventTable::class));
        $controller->setRegTable($container->get(Model\RegistrationTable::class));
        $controller->setAttendeeTable($container->get(Model\AttendeeTable::class));
        //***vvv*** ADD THIS ***vvv*****
        $controller->setFilter($container->get('events-reg-data-filter'));
        //***^^^*** ADD THIS ***^^^*****
        return $controller;
    },
```

### AccessControl Redirect Issue
#### Notes
* Not a fatal problem
* Messes up screen redrawing
* Affects:
  * guestbook
  * onlinemarket.work
  * onlinemarket.complete
#### Solution
* In these 3 files:
  * `guestbook/module/AccessControl/src/Listener/AclListenerAggregate.php`:
  * `onlinemarket.work/module/AccessControl/src/Listener/AclListenerAggregate.php`:
  * `onlinemarket.completemodule/AccessControl/src/Listener/AclListenerAggregate.php`:
* Change this:
```
$match->setParam('controller', self::DEFAULT_CONTROLLER);
$match->setParam('action', self::DEFAULT_ACTION);
```
* To this:
```
// this does the equivalent of a forward:
$response = $e->getResponse();
$response->getHeaders()->addHeaderLine('Location', '/');
$response->setStatusCode(302);
return $response;
```

### TRANSLATION LAB:
* Need to replace the contents of the files shown here:
  * `onlinemarket.work/module/Translation/language/de.php`
```
// not ready yet!
```
  * `onlinemarket.work/module/Translation/language/es.php`
```
// not ready yet!
```

### GUESTBOOK PROJECT UPDATES:
* `AccessControl\Listener\AclListenerAggregate` lines 59 - 60 change to this:
  * From this:
```
$match->setParam('controller', self::DEFAULT_CONTROLLER);
$match->setParam('action', self::DEFAULT_ACTION);
```
  * To this:
```
// this does the equivalent of a forward:
$response = $e->getResponse();
$response->getHeaders()->addHeaderLine('Location', '/');
$response->setStatusCode(302);
return $response;
```

## DAY ZERO
* Sample out for `vagrant up`
```
$ vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'datashuttle/RWZ-Ubuntu-16.04LTS-DTP'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'datashuttle/RWZ-Ubuntu-16.04LTS-DTP' is up to date                                ...
==> default: Setting the name of the VM: ZFF2 - Provisioning
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
    default: The guest additions on this VM do not match the installed version o                                f
    default: VirtualBox! In most cases this is fine, but in rare cases it can
    default: prevent things such as shared folders from working properly. If you                                 see
    default: shared folder errors, please make sure the guest additions within t                                he
    default: virtual machine match the version of VirtualBox you have installed                                 on
    default: your host and reload your VM.
    default:
    default: Guest Additions Version: 5.1.30
    default: VirtualBox Version: 5.2
==> default: Setting hostname...
==> default: Mounting shared folders...
    default: /home/vagrant/Shared => D:/VM/ZF-Level-2
==> default: Running provisioner: shell...
    default: Running: C:/Users/george/AppData/Local/Temp/vagrant-shell20180130-4                                012-h9fjsf.sh
    default: Provisioning course projects...
    default: [DONE: Provisioning course project(s)]
==> default: Running provisioner: shell...
    default: Running: C:/Users/george/AppData/Local/Temp/vagrant-shell20180130-4                                012-qodn2a.sh
    default: Provisioning virtual hosts for the project...
    default: [DONE: Provisioning virtual hosts]
==> default: Running provisioner: shell...
    default: Running: C:/Users/george/AppData/Local/Temp/vagrant-shell20180130-4                                012-ws6ezu.sh
    default: Provisioning course DB as necessary
    default: Bootstrap the course MySql database...
    default: [DONE: Provisioning course MySQL DB]
==> default: Running provisioner: shell...
    default: Running: C:/Users/george/AppData/Local/Temp/vagrant-shell20180130-4                                012-1fh2eak.sh
    default: Provisioning environment...
    default: [DONE: Provisioning environment]
==> default: Running provisioner: shell...
    default: Running: C:/Users/george/AppData/Local/Temp/vagrant-shell20180130-4                                012-1smdm5x.sh
    default: Provisioning general cleanup detail...
    default: [DONE: Provisioning cleanup]

```

