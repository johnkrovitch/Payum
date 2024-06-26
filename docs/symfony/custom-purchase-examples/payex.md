# Payex

Steps:

* [Download libraries](payex.md#download-libraries)
* [Configure gateway](payex.md#configure-context)
* [Prepare payment](payex.md#prepare-payment)

_**Note**: We assume you followed all steps in_ [_get it started_](../get-it-started.md) _and your basic configuration same as described there._

### Download libraries

Run the following command:

```bash
$ php composer.phar require "payum/payex"
```

### Configure gateway

```yaml
#app/config/config.yml

payum:
    gateways:
        your_gateway_here:
            factory: payex
            account_number:  'get this from gateway side'
            encryption_key:  'get this from gateway side'
            sandbox: true
```

_**Attention**: You have to changed `your_gateway_name` to something more descriptive and domain related, for example `post_a_job_with_payex`._

### Prepare payment

Now we are ready to prepare the payment. Here we set price, currency, cart items details and so. Please note that you have to set details in the payment gateway specific format.

```php
<?php
//src/Acme/PaymentBundle/Controller
namespace AcmeDemoBundle\Controller;

use Symfony\Component\HttpFoundation\Request;

class PaymentController extends Controller
{
    public function preparePayexPaymentAction()
    {
        $gatewayName = 'your_gateway_name';

        $storage = $this->getPayum()->getStorage('Acme\PaymentBundle\Entity\PaymentDetails');

        /** @var \Acme\PaymentBundle\Entity\PaymentDetails $details */
        $details = $storage->create();
        $details['price'] = $data['amount'] * 100;
        $details['priceArgList'] = '';
        $details['vat'] = 0;
        $details['currency'] = $data['currency'];
        $details['orderId'] = 123;
        $details['productNumber'] = 123;
        $details['purchaseOperation'] = OrderApi::PURCHASEOPERATION_AUTHORIZATION;
        $details['view'] = OrderApi::VIEW_CREDITCARD;
        $details['description'] = 'a desc';
        $details['clientIPAddress'] = $request->getClientIp();
        $details['clientIdentifier'] = '';
        $details['additionalValues'] = '';
        $details['agreementRef'] = '';
        $details['clientLanguage'] = 'en-US';
        $storage->update($details);

        $captureToken = $this->get('payum')->getTokenFactory()->createCaptureToken(
            $gatewayName,
            $details,
            'acme_payment_done' // the route to redirect after capture;
        );

        $details['returnurl'] = $captureToken->getTargetUrl();
        $details['cancelurl'] = $captureToken->getTargetUrl();
        $storage->update($details);

        return $this->redirect($captureToken->getTargetUrl());
    }
}
```

That's it. After the payment done you will be redirect to `acme_payment_done` action. Check [this chapter](../purchase-done-action.md) to find out how this done action could look like.

### Next Step

* [Examples list](../custom-purchase-examples.md).

***

### Supporting Payum

Payum is an MIT-licensed open source project with its ongoing development made possible entirely by the support of community and our customers. If you'd like to join them, please consider:

* [Become a sponsor](https://github.com/sponsors/Payum)
