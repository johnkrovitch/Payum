# Capture Payment

```php
<?php

use Payum\Core\Model\Payment;
use Payum\Core\GatewayInterface;
use Payum\Core\Request\Capture;

$model = new Payment();
$model->setCurrencyCode('USD');
$model->setTotalAmount(1);

/** @var GatewayInterface $gateway */
$gateway->execute(new Capture($model));

// or using raw format
 
$model = [
    'amount' => 100,
    'currency' => 'USD',
];

$gateway->execute(new Capture($model);
```

Back to [examples](index.md)

***

### Supporting Payum

Payum is an MIT-licensed open source project with its ongoing development made possible entirely by the support of community and our customers. If you'd like to join them, please consider:

* [Become a sponsor](https://github.com/sponsors/Payum)
