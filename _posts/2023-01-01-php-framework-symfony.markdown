---
layout: post
title:  "Symfony framework features"
date:   2023-01-01 20:00:00 +0200
categories: php framework symfony
excerpt: "An overview of Symnfony's most useful built-in features, including documentation, community support and the services setup."
---

An overview of Symnfony's most useful built-in features, including documentation, community support and the services setup.

<h3>Autowiring</h3>

In Symfony, each service lives inside a very special object called the service container. The container allows you to centralize the way objects are constructed. It makes your life easier, promotes a strong architecture and is super fast!

You can automatically use any classes from the `src/` directory as a service, without needing to manually configure it:

{% highlight yaml %}
# config/services.yaml
services:
    # default configuration for services in *this* file
    _defaults:
        autowire: true      # Automatically injects dependencies in your services.
        autoconfigure: true # Automatically registers your services as commands, event subscribers, etc.

    # makes classes in src/ available to be used as services
    # this creates a service per class whose id is the fully-qualified class name
    App\:
        resource: '../src/'
        exclude:
            - '../src/DependencyInjection/'
            - '../src/Entity/'
            - '../src/Kernel.php'

    # order is important in this file because service definitions
    # always *replace* previous ones; add your own service configuration below

    # ...
{% endhighlight %}

More: [here][autowire-link]{:target="_blank" rel="noopener"}

<h3>Tags</h3>

Service tags are a way to tell Symfony or other third-party bundles that your service should be registered in some special way.
Services tagged with the `twig.extension` tag are collected during the initialization of TwigBundle and added to Twig as extensions.

If you enable autoconfigure, then some tags are automatically applied for you. That's true for the twig.extension tag: the container sees that your class extends `AbstractExtension` (or more accurately, that it implements `ExtensionInterface`) and adds the tag for you.

If you want to apply tags automatically for your own services, use the `_instanceof` option:

{% highlight yaml %}
# config/services.yaml
services:
    # this config only applies to the services created by this file
    _instanceof:
        # services whose classes are instances of CustomInterface will be tagged automatically
        App\Security\CustomInterface:
            tags: ['app.custom_tag']
    # ...
{% endhighlight %}

More: [here][tags-link]{:target="_blank" rel="noopener"}

<h3>Twig</h3>

The Twig templating language allows you to write concise, readable templates that are more friendly to web designers and, in several ways, more powerful than PHP templates.

You can't run PHP code inside Twig templates, but Twig provides utilities to run some logic in the templates. For example, `filters` modify content before being rendered, like the upper [filter][filters-link]{:target="_blank" rel="noopener"} to uppercase contents.

Twig comes with a long list of `tags`, `filters` and `functions` that are available by default. In Symfony applications you can also use these Twig filters and functions defined by Symfony and you can create your own Twig filters and functions.

Twig is fast in the prod environment (because templates are compiled into PHP and cached automatically), but convenient to use in the dev environment (because templates are recompiled automatically when you change them).

More: [here][symf-filters-link]{:target="_blank" rel="noopener"}

<h3>Doctrine</h3>

Symfony provides all the tools you need to use databases in your applications thanks to [Doctrine][doctrine-link]{:target="_blank" rel="noopener"}, the best set of PHP libraries to work with databases. These tools support relational databases like `MySQL` and `PostgreSQL` and also NoSQL databases like `MongoDB`.

<h4>Persisting Objects to the Database</h4>

{% highlight php %}
// src/Controller/ProductController.php
namespace App\Controller;

// ...
use App\Entity\Product;
use Doctrine\Persistence\ManagerRegistry;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class ProductController extends AbstractController
{
    #[Route('/product', name: 'create_product')]
    public function createProduct(ManagerRegistry $doctrine): Response
    {
        $entityManager = $doctrine->getManager();

        $product = new Product();
        $product->setName('Keyboard');
        $product->setPrice(1999);
        $product->setDescription('Ergonomic and stylish!');

        // tell Doctrine you want to (eventually) save the Product (no queries yet)
        $entityManager->persist($product);

        // actually executes the queries (i.e. the INSERT query)
        $entityManager->flush();

        return new Response('Saved new product with id '.$product->getId());
    }
}
{% endhighlight %}

<h4>Deleting an Object</h4>

{% highlight php %}
    $entityManager->remove($product);
    $entityManager->flush();
{% endhighlight %}

<h4>Querying with custom SQL</h4>

{% highlight php %}
// src/Repository/ProductRepository.php

// ...
class ProductRepository extends ServiceEntityRepository
{
    public function findAllGreaterThanPrice(int $price): array
    {
        $conn = $this->getEntityManager()->getConnection();

        $sql = '
            SELECT * FROM product p
            WHERE p.price > :price
            ORDER BY p.price ASC
            ';
        $stmt = $conn->prepare($sql);
        $resultSet = $stmt->executeQuery(['price' => $price]);

        // returns an array of arrays (i.e. a raw data set)
        return $resultSet->fetchAllAssociative();
    }
}
{% endhighlight %}

More here: [Symfony 6.1 docs][symf-link]{:target="_blank" rel="noopener"}

[symf-link]: https://symfony.com/doc/6.1/index.html
[autowire-link]: https://symfony.com/doc/6.1/service_container.html#the-autowire-option
[tags-link]: https://symfony.com/doc/6.1/service_container/tags.html
[filters-link]: https://twig.symfony.com/doc/3.x/filters/index.html
[symf-filters-link]: https://symfony.com/doc/6.1/reference/twig_reference.html
[doctrine-link]: https://symfony.com/doc/6.1/doctrine.html#databases-and-the-doctrine-orm
