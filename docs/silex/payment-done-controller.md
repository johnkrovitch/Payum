# Payment Done Controller

First we have to validate the request. If it is valid the verifier returns a token. We can use it later to get payment status, details and any other information.

```php
<?php

use Payum\Core\Registry\RegistryInterface;
use Payum\Core\Request\GetHumanStatus;
use Payum\Core\Security\HttpRequestVerifierInterface;
use Symfony\Component\HttpFoundation\Request;

class PaymentController extends BaseController
{
    protected $app;

    public function __constructor(Application $app)
    {
        $this->app = $app;
    }

    public function done(Request $request)
    {
        $token = $this->app['payum.security.http_request_verifier']->verify($request);

        $gateway = $this->app['payum']->getGateway($token->getGatewayName());

        $gateway->execute($status = new GetHumanStatus($token));
        
        $payment = $status->getFirstModel();

        return new JsonResponse(array(
            'status' => $status->getValue(),
            'details' => $payment->getDetails(), 
        ));
    }
}
```

***

### Supporting Payum

Payum is an MIT-licensed open source project with its ongoing development made possible entirely by the support of community and our customers. If you'd like to join them, please consider:

* [Become a sponsor](https://github.com/sponsors/Payum)
