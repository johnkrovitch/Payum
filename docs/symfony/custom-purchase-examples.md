# Custom purchase examples

### Configure

```php
<?php
namespace Acme\PaymentBundle\Entity;

use Doctrine\ORM\Mapping as ORM;
use Payum\Core\Model\ArrayObject;

/**
 * @ORM\Table
 * @ORM\Entity
 */
class PaymentDetails extends ArrayObject
{
    /**
     * @ORM\Column(name="id", type="integer")
     * @ORM\Id
     * @ORM\GeneratedValue(strategy="IDENTITY")
     *
     * @var integer $id
     */
    protected $id;
}
```

```yml
#app/config/config.yml

payum:
    storages:
        Acme\PaymentBundle\Entity\PaymentDetails: { doctrine: orm }
```

\-**Note**: You should use commented path if you install payum/payum package.-

* [Paypal express checkout](custom-purchase-examples/paypal-express-checkout.md).
* [Paypal pro checkout](custom-purchase-examples/paypal-pro-checkout.md).
* [Payex](custom-purchase-examples/payex.md).
* [Authorize.Net AIM](custom-purchase-examples/authorize-net-aim.md).
* [Be2Bill credit card](custom-purchase-examples/be2bill.md).
* [Be2Bill onsite](custom-purchase-examples/be2bill-onsite.md).
* [Klarna Checkout](custom-purchase-examples/klarna-checkout.md).
* [Klarna Invoice](custom-purchase-examples/klarna-invoice.md).
* [Stripe.Js](custom-purchase-examples/stripe-js.md).
* [Stripe Checkout](custom-purchase-examples/stripe-checkout.md).
* [Stripe Direct (via omnipay)](custom-purchase-examples/stripe-via-omnipay.md).
* [Paypal express checkout (via omnipay)](custom-purchase-examples/paypal-via-omnipay.md).
* [JMS payment plugins](../jms-payment-bridge/get-it-started.md).

***

### Supporting Payum

Payum is an MIT-licensed open source project with its ongoing development made possible entirely by the support of community and our customers. If you'd like to join them, please consider:

* [Become a sponsor](https://github.com/sponsors/Payum)
