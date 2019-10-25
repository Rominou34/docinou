# Symfony

## Useful commands

* `php bin/console make:entity` allows you to create a table
* `php bin/console make:migration`, once you modified your entity, you create a migration in order to update the database accordingly
* `php bin/console doctrine:migrations:migrate` launches the migration and updates the database

## Packages

* `Symfony\Component\Routing\Annotation\Route` to use Routes un annotations
* `FOS\RestBundle\Controller\Annotations as Rest` to use Routes to build a REST Api, also using annotations
* `Symfony\Component\HttpFoundation\Request`, `Symfony\Component\HttpFoundation\Response`, and `Symfony\Component\HttpFoundation\JsonResponse` to use Request / Response objects for the Api

## Tips

**Circular References**

When serializing objects (to return them as Json for exemple), you can easily get a circular reference. For exemple, let's say you have products and categories, with each product being in one category (ManyToOne). When serializing the product you will get the category and then when getting the category you will get its products -> BOOM ! circular reference.

```php
$data = $serializer->serialize($product, JsonEncoder::FORMAT);
var_dump($data); // Will throw a circular reference exception
```

To counter this, you have to create a custom callable (for example, when called by the Product's serializer, Category's serializer will only return the name of the category instead of serializing the whole entity).

```php
use Symfony\Component\Serializer\Normalizer\AbstractNormalizer;
use Symfony\Component\Serializer\Normalizer\ObjectNormalizer;
use Symfony\Component\Serializer\Serializer;
// ...
$encoder = new JsonEncoder();
$defaultContext = [
    AbstractNormalizer::CIRCULAR_REFERENCE_HANDLER => function ($object, $format, $context) {
        return $object->getId();
    },
];
$normalizer = new ObjectNormalizer(null, null, null, null, null, null, $defaultContext);

$serializer = new Serializer([$normalizer], [$encoder]);
$data = $serializer->serialize($product, JsonEncoder::FORMAT);
var_dump($data); // Will be ok
```
