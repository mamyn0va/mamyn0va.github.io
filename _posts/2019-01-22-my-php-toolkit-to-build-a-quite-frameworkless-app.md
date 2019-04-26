---
title: My PHP Toolkit to Build a (quite) Frameworkless App
published: true
description: My PHP Toolkit / On Building a (quite) Frameworkless App with Slim and no ORM
tags: php, tools, githunt, framework
cover_image: https://thepracticaldev.s3.amazonaws.com/i/sp6baiex1tjq736iz7lk.jpg
---

Hey, let me introduce you some of the libraries & tools I've been using in many PHP projects running in production.

I'm used to build my own framework by picking up libs in the below list each time I start a new PHP project. But to be honest, I must admit that I still use a micro-framework for basic HTTP stuff: [Slim](https://www.slimframework.com/).

📝 *I use no ORM and I mainly build backend apps with Web APIs.*

## Libraries

### 1. Slim Framework

![slim](https://thepracticaldev.s3.amazonaws.com/i/qrzabdcle0b75ecjqa1o.png)

🙋 **Purpose:** *Micro-Framework intended to build Web APIs*<br/>
🌠 **GitHub stars:** *9,475<br/>*
🔗 **URL:** *[slimphp/sliim](https://github.com/slimphp/Slim)*<br/>

### 2. Slim Framework CSRF protection middleware

🙋 **Purpose:** *Protect your GUI pages with a CSRF token<br/>*
🌠 **GitHub stars:** *201*<br/>
🔗 **URL:** *[slimphp/csrf](https://github.com/slimphp/Slim-Csrf)*<br/>

### 3. Slim Framework Flash Messages

🙋 **Purpose:** *This enables you to define transient messages that persist only from the current request to the next request*<br />
🌠 **GitHub stars:** *104*<br />
🔗 **URL:** *[slimphp/flash](https://github.com/slimphp/Slim-Flash)*<br />

### 4. Twig

![twig](https://thepracticaldev.s3.amazonaws.com/i/0s27wg4owzcagj2urnpf.png)

🙋 **Purpose:** *A very popular template engine that integrates well with Slim ([slimphp/twig-view](https://github.com/slimphp/Twig-View))*<br />
🌠 **GitHub stars:** *5,705*<br />
🔗 **URL:** *[twigphp/twig](https://github.com/twigphp/Twig)*<br />

### 5. Monolog

🙋 **Purpose:** *Sends your logs to files, sockets, inboxes, databases and various web services*<br />
🌠 **GitHub stars:** *13,388*<br />
🔗 **URL:** *[seldaek/monolog](https://github.com/Seldaek/monolog)*<br />

### 6. Zend ACL permissions

🙋 **Purpose:** *Provides a lightweight and flexible access control list (ACL) implementation for privileges management*<br />
🌠 **GitHub stars:** *55*<br />
🔗 **URL:** *[zendframework/zend-permissions-acl](https://github.com/zendframework/zend-permissions-acl)*<br />

### 7. Guzzle

🙋 **Purpose:** *Guzzle is a PHP HTTP client that makes it easy to send HTTP requests and trivial to integrate with web services*<br />
🌠 **GitHub stars:** *15,355*<br />
🔗 **URL:** *[guzzlehttp/guzzle](https://github.com/guzzle/guzzle)*<br />

### 8. PDO

🙋 **Purpose:** *PHP extension to build and execute secured SQL prepared statements*<br />
🔗 **URL:** *[PDO](http://php.net/manual/en/book.pdo.php)*<br />

### 9. Zend XML-RPC

🙋 **Purpose:** *Provides support for both consuming remote XML-RPC services and building new XML-RPC servers*<br />
🌠 **GitHub stars:** *14*<br />
🔗 **URL:** *[zendframework/zend-xmlrpc](https://github.com/zendframework/zend-xmlrpc)*<br />

### 10. PHPMailer

🙋 **Purpose:** *A full-featured email creation and transfer class for PHP*<br />
🌠 **GitHub stars:** *12,422*<br />
🔗 **URL:** *[phpmailer/phpmailer](https://github.com/PHPMailer/PHPMailer)*<br />

### 11. Firebase / PHP-JWT

🙋 **Purpose:** *A simple library to encode and decode JSON Web Tokens (JWT) in PHP, conforming to RFC 7519*<br />
🌠 **GitHub stars:** *4,574*<br />
🔗 **URL:** *[firebase/php-jwt](https://github.com/firebase/php-jwt)*<br />

### 12. Hassankhan / Config

🙋 **Purpose:** *Config is a lightweight configuration file loader that supports PHP, INI, XML, JSON, and YAML files*<br />
🌠 **GitHub stars:** *749*<br />
🔗 **URL:** *[hassankhan/config](https://github.com/hassankhan/config)*<br />

## Tools

As a PHP craftsman, the tools below are mandatory in my toolkit. Most of them (except *shellcheck*) are installable through *composer*, which allows you to add them as dev dependencies to your project's `composer.json`.

### 1. Composer

![composer](https://thepracticaldev.s3.amazonaws.com/i/vy9xvol1rclwo2a1q27m.png)

🙋 **Purpose:** *Essential PHP dependency manager, and much more*<br />
🌠 **GitHub stars:** *18,049*<br />
🔗 **URL:** *[Composer](https://getcomposer.org/)*<br />

### 2. PHPUnit

![phpunit](https://thepracticaldev.s3.amazonaws.com/i/7k6yp7wenwtj0eawb7ba.png)

🙋 **Purpose:** *Awesome unit tests framework with mocking features*<br />
🌠 **GitHub stars:** *12,785*<br />
🔗 **URL:** *[PHPUnit](https://phpunit.de/)*<br />

### 3. PHP Code Sniffer

🙋 **Purpose:** *Static analysis tool to detect & fix coding standard violations*<br />
🌠 **GitHub stars:** *5,915*<br />
🔗 **URL:** *[squizlabs/php_codesniffer](https://github.com/squizlabs/PHP_CodeSniffer)*<br />

### 4. PHP Mess Detector aka *phpmd*

![phpmd](https://thepracticaldev.s3.amazonaws.com/i/17tn178k0o2ko760xhi5.png)

🙋 **Purpose:** *Static analysis tool to detect code smells, bad design, bugs, unused parameters, etc.*<br />
🌠 **GitHub stars:** *1,315*<br />
🔗 **URL:** *[phpmd/phpmd](https://phpmd.org/)*<br />

### 5. PHP Coding Standard Fixer aka *php-cs-fixer*

![php-cs-fixer](https://thepracticaldev.s3.amazonaws.com/i/d17gmfhu6g20uah8rp6n.png)

🙋 **Purpose:** *Automatically fixes coding standard violations*<br />
🌠 **GitHub stars:** *7,036*<br />
🔗 **URL:** *[friendsofphp/php-cs-fixer](https://github.com/FriendsOfPHP/PHP-CS-Fixer)*<br />

### 6. SensioLabs Security Checker

![security-checker](https://thepracticaldev.s3.amazonaws.com/i/to78ce7ag0pfgzy45tzs.png)

🙋 **Purpose:** *The SensioLabs Security Checker is a command line tool that checks if your application uses dependencies with known security vulnerabilities*<br />
🌠 **GitHub stars:** *1,397*<br />
🔗 **URL:** *[sensiolabs/security-checker](https://github.com/sensiolabs/security-checker)*<br />

### 7. XML Linter

🙋 **Purpose:** *A PHP tool to lint and validate XML files from the command line*<br />
🌠 **GitHub stars:** *6*<br />
🔗 **URL:** *[sclable/xml-lint](https://github.com/sclable/xml-lint)*<br />

### 8. YAML Linter

🙋 **Purpose:** *Compact command line utility for checking YAML file syntax*<br />
🌠 **GitHub stars:** *3*<br />
🔗 **URL:** *[j13k/yaml-lint](https://github.com/j13k/yaml-lint)*<br />

### 9. Dockerfile Linter

🙋 **Purpose:** *Rule based Dockerfile linter*<br />
🌠 **GitHub stars:** *259*<br />
🔗 **URL:** *[projectatomic/dockerfile_lint](https://github.com/projectatomic/dockerfile_lint)*<br />

### 10. Shellcheck

🙋 **Purpose:** *A static analysis tool for shell scripts*<br />
🌠 **GitHub stars:** *13,440*<br />
🔗 **URL:** *[koalaman/shellcheck](https://github.com/koalaman/shellcheck)*<br />

### 11. Swagger CLI

![swagger-cli](https://thepracticaldev.s3.amazonaws.com/i/eidkatpr5zih7e4k9s4c.png)

🙋 **Purpose:** *Validate Swagger/OpenAPI files in JSON or YAML format*<br />
🌠 **GitHub stars:** *125*<br />
🔗 **URL:** *[APIDevTools/swagger-cli](https://github.com/APIDevTools/swagger-cli)*<br />

---

All these tools can be run automatically:
- in your IDE
- in a git hook
- in your CI/CD pipeline

If you want to go further, please have a look at [one of my former articles]({{ site.baseurl }}{% post_url 2018-09-10-how-to-automate-code-quality-checks-in-your-workflow %}).

Thanks for reading.

See ya!
