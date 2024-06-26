# Paypal via Omnipay

Steps:

* [Download libraries](paypal-via-omnipay.md#download-libraries)
* [Configure gateway](paypal-via-omnipay.md#configure-context)
* [Prepare payment](paypal-via-omnipay.md#prepare-payment)

_**Note**: We assume you followed all steps in_ [_get it started_](../get-it-started.md) _and your basic configuration same as described there._

### Download libraries

Run the following command:

```bash
$ php composer.phar require "payum/omnipay-bridge" "omnipay/paypal:~2.0"
```

### Configure gateway

```yaml
#app/config/config.yml

payum:
    gateways:
        your_gateway_here:
            factory: omnipay
            type: Paypal_Express
            username: 'REPLACE IT',
            password: 'REPLACE IT',
            signature: 'REPLACE IT',
            testMode: true
```

_**Note:** You have to changed `your_gateway_name` to something more descriptive and domain related, for example `post_a_job_with_omnipay`._

_**Note:** If you have to use onsite gateway like paypal express checkout use `omnipay_onsite` factory._

_**Note:** The `type` option can be set directly with a class name to register an unofficial gateway._

### Prepare payment

Now we are ready to prepare the payment. Here we set price, currency, cart items details and so. Please note that you have to set details in the payment gateway specific format.

```php
<?php
//src/Acme/PaymentBundle/Controller
namespace AcmeDemoBundle\Controller;

use Payum\Core\Security\SensitiveValue;
use Symfony\Component\HttpFoundation\Request;

class PaymentController extends Controller
{
    public function preparePaypalPaymentAction(Request $request)
    {
        $gatewayName = 'your_gateway_name';

        $storage = $this->get('payum')->getStorage('Acme\PaymentBundle\Entity\PaymentDetails');

        /** @var \Acme\PaymentBundle\Entity\PaymentDetails */
        $details = $storage->create();
        $details['amount'] = 10;

        $storage->update($details);

        $captureToken = $this->get('payum')->getTokenFactory()->createCaptureToken(
            $gatewayName,
            $details,
            'acme_payment_done' // the route to redirect after capture;
        );

        return $this->redirect($captureToken->getTargetUrl());
    }
}
```

That's it. It will ask user for credit card and convert it to payment specific format. After the payment done you will be redirect to `acme_payment_done` action. Check [this chapter](../purchase-done-action.md) to find out how this done action could look like.

If you still able to pass credit card details explicitly:

```php
<?php
use Payum\Core\Security\SensitiveValue;

$details['card'] = new SensitiveValue(array(
    'number' => '5555556778250000',
    'cvv' => 123,
    'expiryMonth' => 6,
    'expiryYear' => 16,
    'firstName' => 'foo',
    'lastName' => 'bar',
));
```

### Next Step

* [Examples list](../custom-purchase-examples.md).

***

### Supporting Payum

Payum is an MIT-licensed open source project with its ongoing development made possible entirely by the support of community and our customers. If you'd like to join them, please consider:

* [Become a sponsor](https://github.com/sponsors/Payum)
