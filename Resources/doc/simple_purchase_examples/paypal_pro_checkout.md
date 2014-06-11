# Paypal pro checkout

Steps:

* [Download libraries](#download-libraries)
* [Configure context](#configure-context)
* [Prepare payment](#prepare-payment)

_**Note**: We assume you followed all steps in [get it started](https://github.com/Payum/PayumBundle/blob/master/Resources/doc/get_it_started.md) and your basic configuration same as described there._

## Download libraries

Run the following command:

```bash
$ php composer.phar require "payum/paypal-pro-checkout-nvp:*@stable"
```

## Configure context

```yaml
#app/config/config.yml

payum:
    contexts:
        your_context_here:
            paypal_pro_checkout_nvp:
                username: 'EDIT ME'
                password: 'EDIT ME'
                partner:  'EDIT ME'
                vendor:   'EDIT ME'
                sandbox: true

            storages:
                Acme\PaymentBundle\Entity\PaymentDetails:
                    doctrine:
                        driver: orm
```

_**Attention**: You have to changed `your_payment_name` to something more descriptive and domain related, for example `post_a_job_with_paypal`._

## Prepare payment

Now we are ready to prepare the payment. Here we set price, currency, cart items details and so.
Please note that you have to set details in the payment gateway specific format.

```php
<?php
//src/Acme/PaymentBundle/Controller
namespace AcmeDemoBundle\Controller;

use Symfony\Component\HttpFoundation\Request;

class PaymentController extends Controller
{
    public function preparePaypalProCheckoutPaymentAction(Request $request)
    {
        $paymentName = 'your_payment_name';

        $storage = $this->get('payum')->getStorageForClass(
            'Acme\PaymentBundle\Entity\PaymentDetails',
            $paymentName
        );

        /** @var \Acme\PaymentBundle\Entity\PaymentDetails $paymentDetails */
        $paymentDetails = $storage->createModel();
        $paymentDetails['amt'] = 1;
        $paymentDetails['currency'] = 'USD';
        $storage->updateModel($paymentDetails);

        $captureToken = $this->get('payum.security.token_factory')->createCaptureToken(
            $paymentName,
            $paymentDetails,
            'acme_payment_done' // the route to redirect after capture;
        );

        return $this->redirect($captureToken->getTargetUrl());
    }
}
```

That's it. It will ask user for credit card and convert it to payment specific format. After the payment done you will be redirect to `acme_payment_done` action.
Check [this chapter](https://github.com/Payum/PayumBundle/blob/master/Resources/doc/purchase_done_action.md) to find out how this done action could look like.

If you still able to pass credit card details explicitly:

```php
<?php
use Payum\Core\Security\SensitiveValue;

$paymentDetails['acct'] = new SensitiveValue('5105105105105100');
$paymentDetails['cvv2'] = new SensitiveValue('123');
$paymentDetails['expDate'] = new SensitiveValue('1214');
```

## Next Step

* [Purchase done action](https://github.com/Payum/PayumBundle/blob/master/Resources/doc/purchase_done_action.md).
* [Configuration reference](https://github.com/Payum/PayumBundle/blob/master/Resources/doc/configuration_reference.md).
* [Back to examples list](https://github.com/Payum/PayumBundle/blob/master/Resources/doc/simple_purchase_examples.md).
* [Back to index](https://github.com/Payum/PayumBundle/blob/master/Resources/doc/index.md).