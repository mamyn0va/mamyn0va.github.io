---
title: 🐘 Unit Tests in PHP
published: true
description: How to structure your code and use PHPUnit to mock your dependencies
tags: php, testing, showdev, quality
cover_image: https://thepracticaldev.s3.amazonaws.com/i/c5rypwl3ykxxzvuvmkld.png
---

Hi folks! I'm going to talk about the way I use to design my unit tests in PHP.

## <i class="fas fa-terminal"></i> Disclaimer

Let's start with a (my?) definition of unit testing:

> The goal of unit tests is to check the correct behavior of each of the public functions, be given a representative set of data.
> The important thing is that external calls inside these functions must be tested in dedicated tests and therefore needs to be mocked.
> The interest of having these tests is to run them each time you modify the codebase to verify that no regression was introduced.

As a pre-requisite, the code needs to be *testing ready* in the sense that we need that to allow mocking of the dependencies. In a given function, a dependency can be internal (e.g. another class of the project), or external (e.g. a call to 3rd-party library).

I won't be talking about the way of making the code testable. I know two techniques to achieve that using the dependency injection principle, but maybe there are other alternatives:
- first, you can modify the interface of your function to pass all its dependencies as parameters (or using the constructor or setters),
- or you can use a **Dependency Injection Container** (aka DIC) which is a common (anti?)pattern in software development (see [PHP's PSR11](https://www.php-fig.org/psr/psr-11/) for further details).

## <i class="fas fa-terminal"></i> Code architecture

When I start a new web project, I usually split my code in several distinct layers:
- a *router* (usually Slim router), responsible for processing HTTP requests,
- a *controller* layer, responsible for data validation and output rendering,
- a *business* layer, responsible for the business logic,
- a *mapper* layer (aka DTO),
- a *model* layer (aka DAO).

Doing so makes the code more testable. 

Some additional stuff may be required, depending on the needs, such as:
- *checkers*, to validate inputs,
- *views*, to render outputs,
- *middlewares*, to add pre- and post-processing of HTTP requests,
- *routes*, to manage the REST API's routes,
- *enablers*, to manage outgoing requests to 3rd-party APIs (SMS, Email,...).

And finally, a bootstrap file to create all the things.

The classical workflow is:
- a *controller* gets called on a given function,
- it validates the input using a dedicated *checker*,
- it calls the *business* layer with verified input,
    - the *business* layer optionally uses a *mapper* to interact with the db
    - it may also interact with a 3rd-party API through an *enabler*
    - it optionally returns *models* to the *controller* layer
- the *controller* renders the *models* returned by the *business* layer using a dedicated *view*.

When I write unit tests for a given layer, I only test the behavior of the layer's functions and I mock the calls to the functions of the sub-layers. For example, when I write a test for a *controller*, I mock the call to the *business* layer, to the *checker* and to the *view*. When I write a test for a *business* object, I mock the call to the *mapper*, to the *enabler* and to other *business* objects.

## <i class="fas fa-terminal"></i> Example

Let's see what it looks like for a *controller* with a bit of code:

```php
class UserController
{
    /** @var UserBusiness */
    public $business;
    /** @var UserChecker */
    public $checker;
    /** @var UserView */
    public $view;

    public function __construct(
        UserBusiness $business, 
        UserChecker $checker, 
        UserView $view
    ) {
        $this->business = $business;
        $this->checker = $checker;
        $this->view = $view;
    }

    public function getById(string $id): string
    {
        $this->checker->checkId($id);
        $user = $this->business->getById($id);

        return $this->view->render($user);
    }
}
```

Here you can see 3 dependencies in the `getById` function. I choose to pass these dependencies through the constructor of my `UserController` class. Alternatively, I could have used a Container passed to the function (or to the constructor), or I could have passed the deps through the function's parameters. The result would have been the same: I have to mock these 3 deps to test the function.

## <i class="fas fa-terminal"></i> Using mocks

Thankfully, [PHPUnit](https://phpunit.de/) comes with a great API to work with mocks (see [Test Doubles](https://phpunit.readthedocs.io/en/7.4/test-doubles.html)). I won't cover all the features here, but the documentation worth a look.

First, I need to mock the `checkId` function of the `UserChecker`. As you may guess, it raises an exception if the `id` is wrongly formatted.
Then, I mock the `getById` function of the `UserBusiness`.
And finally, the `render` function of the `UserView`.

```php

class UserControllerTest extends PHPUnit\Framework\TestCase
{
    public function testGetById_Ok()
    {
        $business = $this->createMock(UserBusiness::class);
        $checker = $this->createMock(UserChecker::class);
        $view = $this->createMock(UserView::class);

        $expectedId = 'id';
        $expectedUser = new UserModel($expectedId, 'john', 'doe');
        $expectedResult = 'result';

        $checker->expects($this->once())
            ->method('checkId')
            ->with($expectedId);

        $business->expects($this->once())
            ->method('getById')
            ->with($expectedId)
            ->willReturn($expectedUser);

        $view->expects($this->once())
            ->method('render')
            ->with($expectedUser)
            ->willReturn($expectedResult);

        $controller = new UserController($business, $checker, $view);
        $actualResult = $controller->getById($expectedId);

        $this->assertEquals($expectedResult, $actualResult);
    }
}
```

The same has to be done for every layers.

## <i class="fas fa-terminal"></i> Conclusion

Writing exhaustive unit tests can be painful as most of the time, you'll spent more time writing the test than writing the "real" code.
But IMO, there is no acceptable trade-off when it comes to testing your app.
I know that other testing techniques exists (like TDT), but I see them as complementary tests as they're closer to integration tests than to unit tests. But maybe I'm wrong?

---

Please feel free to give your opinion!

Thanks for reading!
