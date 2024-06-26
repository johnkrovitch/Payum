# Klarna Invoice

Steps:

* [Download libraries](klarna-invoice.md#download-libraries)
* [Configure gateway](klarna-invoice.md#configure-context)
* [Prepare payment](klarna-invoice.md#prepare-payment)

_**Note**: We assume you followed all steps in_ [_get it started_](../get-it-started.md) _and your basic configuration same as described there._

### Download libraries

Run the following command:

```bash
$ php composer.phar require "payum/klarna-invoice:@stable"
```

### Configure gateway

```yaml
#app/config/config.yml

payum:
    gateways:
        your_gateway_here:
            factory: klarna_invoice
            secret: 'EDIT ME'
            eid: 'EDIT ME'
            sandbox: true
```

_**Attention**: You have to changed `your_gateway_name` to something more descriptive and domain related, for example `post_a_job_with_klarna`._

### Prepare payment

Now we are ready to prepare the payment. Here we set price, currency, cart items details and so on. Please note that you have to set details in the payment gateway specific format.

```php
<?php
//src/Acme/PaymentBundle/Controller
namespace AcmeDemoBundle\Controller;

use Symfony\Component\HttpFoundation\Request;

class PaymentController extends Controller
{
    public function prepareKlarnaInvoiceAction()
    {
        $gatewayName = 'your_gateway_name';

        $storage = $this->get('payum')->getStorage('Acme\PaymentBundle\Entity\PaymentDetails');

        /** @var \Acme\PaymentBundle\Entity\PaymentDetails $details */
        $payment = $payum->getPayment($paymentName);
        $payment->execute($getAddresses = new GetAddresses($pno));

        $details = $storage->create();
        $details = array(
            /** @link http://developers.klarna.com/en/testing/invoice-and-account */
            'pno' => '410321-9202',
            'amount' => -1,
            'gender' => \KlarnaFlags::MALE,
            'articles' => array(
                array(
                    'qty' => 4,
                    'artNo' => 'HANDLING',
                    'title' => 'Handling fee',
                    'price' => '50.99',
                    'vat' => '25',
                    'discount' => '0',
                    'flags' => \KlarnaFlags::INC_VAT | \KlarnaFlags::IS_HANDLING
                ),
            ),
            'billing_address' => $getAddresses->getFirstAddress()->toArray(),
            'shipping_address' => $getAddresses->getFirstAddress()->toArray(),
        );
        $storage->update($details);

        $captureToken = $this->getTokenFactory()->createCaptureToken(
            $gatewayName,
            $details,
            'acme_payment_done'
        );
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
