# Event Dispatcher

The EventDispatcherExtensions provides a Bridge to the [Symfony EventDispatcher Component](http://symfony.com/doc/current/components/event\_dispatcher/index.html). The EventDispatcherComponent allows you to add behaviour without changing Payum.

### Enable the EventDispatcherExtension

```php
<?php

use Payum\Core\Bridge\Symfony\Extension\EventDispatcherExtension;

/** @var \Payum\Core\Gateway $gateway */
/** @var \Symfony\Component\EventDispatcher\EventDispatcherInterface $eventDispatcher */

$gateway->addExtension(
    new EventDispatcherExtension($eventDispatcher)
);
```

### Listen to an Event

```php
<?php

use Payum\Core\Bridge\Symfony\Event\ExecuteEvent;
use Payum\Core\Bridge\Symfony\PayumEvents;

/** @var \Symfony\Component\EventDispatcher\EventDispatcherInterface $eventDispatcher */

$eventDispatcher->addListener(
    PayumEvents::GATEWAY_EXECUTE,
    function(ExecuteEvent $event) {
        // do something
    }
);
```

| Name                        |  `PayumEvents` Constant             | Argument passed to the listener |
| --------------------------- | ----------------------------------- | ------------------------------- |
| payum.gateway.pre\_execute  | `PayumEvents::GATEWAY_PRE_EXECUTE`  | `ExecuteEvent`                  |
| payum.gateway.execute       | `PayumEvents::GATEWAY_EXECUTE`      | `ExecuteEvent`                  |
| payum.gateway.post\_execute | `PayumEvents::GATEWAY_POST_EXECUTE` | `ExecuteEvent`                  |

### Benefit with PayumBundle

If you use Symfony Full-Stack Framework and the PayumBundle you can add the EventDispatcherExtension via Configuration:

```yaml
services:
    app.payum.extension.event_dispatcher:
        class: Payum\Core\Bridge\Symfony\Extension\EventDispatcherExtension
        public: true
        arguments: ["@event_dispatcher"]
        tags:
            - { name: payum.extension, all: true, prepend: false }
```

And add the listener:

```yaml
services:
    app.payum.listener.render_template:
        class: AppBundle\EventListener\RenderTemplateListener
        tags:
            - { name: kernel.event_listener, event: payum.gateway.execute }
```

***

### Supporting Payum

Payum is an MIT-licensed open source project with its ongoing development made possible entirely by the support of community and our customers. If you'd like to join them, please consider:

* [Become a sponsor](https://github.com/sponsors/Payum)
