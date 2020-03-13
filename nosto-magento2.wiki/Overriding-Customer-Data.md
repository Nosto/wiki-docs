# Adding custom marketing permission detection
In order to modify marketing permission detection you must create a small Magento 2 module. Start by following the steps described in the wiki page [Extending Nosto's Module](Extending-Nosto's-Module).

## Required files and configurations

##### 1. The events config `app/code/My/Nosto/etc/events.xml`

```xml
<?xml version="1.0"?>
<!--suppress XmlUnboundNsPrefix -->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Event/etc/events.xsd">
    <event name="nosto_person_load_after">
        <observer name="nosto_person_load_after_marketing_permission" instance="My\Nosto\Observer\Person\Load" />
    </event>
</config>
```

##### 2. The observer `app/code/My/Nosto/Observer/Person/Load.php`

```php
<?php
namespace My\Nosto\Observer\Person;

use Magento\Framework\Event\Observer;
use Magento\Framework\Event\ObserverInterface;
use Nosto\Object\AbstractPerson;
use Nosto\Object\ModelFilter;
use Nosto\Tagging\Model\ElementFilter;

class Load implements ObserverInterface
{
    /**
     * NOTE! You must inject the classes / services required to determinate
     * the marketing permission consent
     */
    public function __construct()
    {
    }

    /**
     * Observer for "nosto_person_load_after" event
     *
     * @param Observer $observer
     * @return void
     */
    public function execute(Observer $observer)
    {
        /* @var ModelFilter $modelFilter */
        $modelFilter = $observer->getData('modelFilter');

        // Note that you can disable completely person data processing
        // with the modelFilter
        // $modelFilter->setValid(false);

        /* @var AbstractPerson $person */
        $person = $observer->getData('person');
        if ($modelFilter instanceof ModelFilter
        && $person instanceof AbstractPerson) {
            $person->setMarketingPermission(
                $this->hasMarketingPermissions($person)
            );
        }
    }

    /**
     * Custom marketing permission detection
     *
     * @param AbstractPerson $person
     * @return boolean
     */
    private function hasMarketingPermissions(AbstractPerson $person)
    {
        if ($person->getEmail()) {
            // Do your extra validation here
        }

        return $person->getMarketingPermission();
    }
}
```
