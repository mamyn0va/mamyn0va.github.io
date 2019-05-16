---
title: 🚥 How to Write Clean Code in PHP 🐘
published: true
description: This is the first of a 2-part's series about code quality. This is mainly about PHP, but wait, don't run way! you may still be interested if you code in some « serious language ».
tags: cleancode, tools, codingstyle, PHP
cover_image: https://thepracticaldev.s3.amazonaws.com/i/4wh6jqve5w0nio45ot6v.png
series: How to Ensure You Write Clean Code
---

This is the first of a 2-part's series about code quality.
This is mainly about **PHP**, but wait, don't run way!, you may still be interested if you code in some «_serious language_».

---

![good code vs bad code](https://thepracticaldev.s3.amazonaws.com/i/u8djzx1ow0iuwsq8r3kx.png)

> Why should we care about code quality?

Why not just make our code work for what it is supposed to? In fact, it depends on the purpose of your app and on your target. If you're developing a _throwaway app_, for a demo, or just for you, you won't obviously spend your time on quality. But if it's something more serious, it will become mandatory.

Mandatory for the readibility of your code, mandatory for the maintainability, mandatory for your health and that of your teammates.

For the most impatient of you, I created a [template project](https://gitlab.com/mamyn0va/php-template) on gitlab.com to allow you to quickly bootstrap a PHP project with ready-to-use quality tools.

---

> Oh wait! You're talking about quality? Really? In PHP?

PHP has a bad reputation, probably because it's a very permissive language used by unexperimented «_developers_». Its dynamically type mechanism is often misused and before the version 5 and the introduction of the OOP paradigm, it was very messy. On top of that, most of the applications developed in PHP are just Web sites, blogs, forums or any other CMS like drupal, wordpress, joomla, prestashop...

Today, things are very different in PHP ecosystem. Its dependency manager, [composer](https://getcomposer.org/), has been massively adopted by most of the project maintainers and helped to make PHP a credible choice for serious backend applications.
Moreover, PHP7 has brought a breaking change in PHP's performance by enhancing the opcode cache and the Zend Engine. And now, there is the possibility to [type functions' parameters and return value](http://php.net/manual/en/migration70.new-features.php). Combined with the [strict types mode](http://php.net/manual/en/functions.arguments.php#functions.arguments.type-declaration.strict), a lot of hidden runtime bugs are now detected in your IDE.

---

I won't cover here all the aspects of the code quality (tests, review, TDD, ...) but I'll focus on the first very basics things: coding standards/styles & static analysis.

Let it be clear that, of course, tooling has never made by itself the quality of any software, but they help to. Let's say they put us in the mood to continually seek to improve our code. But that does not prevent us from doing unit tests and code review!



## <i class="fas fa-terminal"></i> Code static analysis tools ✔️

The [PHP-FIG](https://www.php-fig.org)​ consortium publishes a [set of recommendations](https://www.php-fig.org/psr/) that aims to standardize the use of PHP and to facilitate its interoperability. Three of them focus on the coding style and on stuff like the auto-loading, the encoding, and so on:
- PSR1 : [Basic Coding Standard](https://www.php-fig.org/psr/psr-1/)
- PSR2 : [Coding Style Guide](https://www.php-fig.org/psr/psr-2/)
- PSR12 (in review) : [Extended Coding Style Guide](https://github.com/php-fig/fig-standards/blob/master/proposed/extended-coding-style-guide.md)

> The tool listed below are widely adopted by the PHP community and are actively supported/maintained.


### <i class="fas fa-terminal"></i> [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer) (aka phpcs)

Fortunately, we don't need to know all the details of the PSR specifications (even if it would be better), because some tools can do (a part of) the job for us. I mean, pretty-printing the code to make it compliant with the PSRs and detect violations. And as time goes on and with experience, it will become natural for you.

**PHP_CodeSniffer** detects PSR1/PSR2 violations, and a lot more with the Squiz standard that check 130 rules (7 rules for PSR1 and 42 for PSR2) and that also works with CSS and JS. That being so, it is a good compromise to start with the PSR2 standard and then to create your own standard (aka ruleset) from the PSR2, and to enrich it with the Squiz rules that you need.

#### <i class="fas fa-terminal"></i> phpcs installation 💡

This will add phpcs as a project dependency:

```shell
composer require "squizlabs/php_codesniffer"
```

To use it outside of a project, you should install it globally and add `vendor/bin` to your `PATH`:

```shell
composer global require "squizlabs/php_codesniffer"
export PATH=$PATH:~/path/to/composer/vendor/bin
```

#### <i class="fas fa-terminal"></i> phpcs usage 🆘

Now, consider the following PHP example that contains some violations...

```php
<?php
class MaClasse{
    var $monAttribut;

    public function MaClasse ($monAttribut){
        $this->setMonAttribut( $monAttribut );
    }

    function setMonAttribut($monAttribut){
        $this->monAttribut=$monAttribut;
    }
}
```

...and see what happens if we use phpcs with the PSR2 standard:

![phpcs psr2](https://thepracticaldev.s3.amazonaws.com/i/ad5961j64i00uu8n6ooe.png)

12 errors are detected by the tool and 6 of them can be fixed automatically.

Let's see what happen if we re-run the tool with the Squiz standard:

![phpcs squiz](https://thepracticaldev.s3.amazonaws.com/i/ievvzmjqb59bs1jjnz7b.png)

28 errors are detected by the tool and 18 of them can be fixed automatically. My opinion about Squiz standard is that it is a little too verbose. Thus, what I did is that I created my own ruleset with all the PSR2 rules and added some of the Squiz rules that make sense to me.


To go further, you can have a look at the [phpcs_psr2](https://gitlab.com/mamyn0va/php-template/blob/master/phpcs_psr2.xml.dist) and [phpcs_squiz](https://gitlab.com/mamyn0va/php-template/blob/master/phpcs_squiz.xml.dist) rulesets.



### <i class="fas fa-terminal"></i> PHP Code Beautifier and Fixer (aka phpcbf)


Well, it's nice to detect violations, but it's even better if we can correct them automatically, right? Fortunately, **PHP_CodeSniffer** comes with another utility that fixes the main PSR1 and PSR2 violations: **PHP Code Beautifier and Fixer** (aka phpcbf). Just run it on a PHP file or on a code base, and it will scan all the files and try to automatically fix the violations:

![phpcbf psr2](https://thepracticaldev.s3.amazonaws.com/i/5ltf1nn9grveodqbgrk4.png)

When using it on `MaClasse.php` with PSR2 standard, it fixes 6 out of 12 violations:

```php
<?php

class MaClasse
{
    var $monAttribut;

    public function MaClasse($monAttribut)
    {
        $this->setMonAttribut($monAttribut);
    }

    function setMonAttribut($monAttribut)
    {
        $this->monAttribut=$monAttribut;
    }
}
```


Now it's your turn to play around with it and create your own ruleset that fits your needs, your teammates' need and your project needs. Of course, this **MUST** be shared with the other devs to have an homogenous codebase and to be able to work together.


### <i class="fas fa-terminal"></i> [PHP Mess Detector](https://phpmd.org/) (aka phpmd)


In the same way as Java and its [PMD](https://pmd.github.io/), PHP also has its own mess detection tool. Through a rulesets mechanism (like PHP_CodeSniffer), this tool will detect many violations towards the *clean code* philosophy:
- possible bugs
- suboptimal code
- overcomplicated expressions
- unused parameters, methods, properties
- ...

#### <i class="fas fa-terminal"></i> phpmd installation 💡

```shell
composer require phpmd/phpmd
```

Then, you need to provide phpmd with a file or directory, and to specify an output format (text, XML or HTML) and one or more rulesets.

#### <i class="fas fa-terminal"></i> phpmd usage 🆘

Let's take the previously fixed example with phpcbf/PSR2, and add a unused variable in it:

```php
<?php
class MaClasse
{
    var $monAttribut;

    public function MaClasse($monAttribut)
    {
        $this->setMonAttribut($monAttribut);
    }

    function setMonAttribut($monAttribut)
    {
        $test;
        $this->monAttribut=$monAttribut;
    }
}
```

Now, check it with phpmd and all available rulesets:

```shell
phpmd MaClasse.php text cleancode,codesize,controversial,design,naming,unusedcode
```

Output of phpmd:

```
MaClasse.php:6    The method MaClasse is not named in camelCase.
MaClasse.php:6    Classes should not have a constructor method with the same name as the class
MaClasse.php:12    Avoid unused local variables such as '$test'.
```

As you can see, the tool is complementary to PHP_CodeSniffer as it found three new interesting violations that weren't detected previously. Of course, if you run it through a full codebase, it will also raise a lot of more or less useful violations that may be noisy and prevent you to see meaningful information. Thus, as I said before for PHP_CodeSniffer, I suggest you to create your own ruleset based on [existing ones](https://github.com/phpmd/phpmd/tree/2.6.0/src/main/resources/rulesets). You need to create a file named `phpmd.xml.dist` at the root of your project and you'll be able to use it in the terminal, in your editor/IDE and in your pipeline.

Please find [here](https://gitlab.com/mamyn0va/php-template/blob/master/phpmd.xml.dist) the ruleset that uses all the existing rules.

But like for phpcs, I'd advise you to proceed iteratively by starting by the minimal ruleset `unusedcode`, and to gradually include new rules that make sense for you and your project.

Please find below a custom ruleset that you can use to start with phpmd:

```xml
<?xml version="1.0"?>
<ruleset name="Custom rule set used in pre-commit git hook"
         xmlns="http://pmd.sf.net/ruleset/1.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://pmd.sf.net/ruleset/1.0.0 http://pmd.sf.net/ruleset_xml_schema.xsd"
         xsi:noNamespaceSchemaLocation="http://pmd.sf.net/ruleset_xml_schema.xsd">
    <description>
        Custom rule set used in pre-commit git hooks
    </description>

    <!-- Import the entire unused code rule set -->
    <rule ref="rulesets/unusedcode.xml" />
    <!-- Import a part of design code rule set -->
    <rule ref="rulesets/design.xml">
        <exclude name="NumberOfChildren" />
        <exclude name="DepthOfInheritance" />
        <exclude name="CouplingBetweenObjects" />
    </rule>
</ruleset>

```


### <i class="fas fa-terminal"></i> [PHP Coding Standards Fixer](https://github.com/FriendsOfPhp/PHP-CS-Fixer) (aka php-cs-fixer)


> What if we ended with another tool? If you had to choose only one tool, it would be this one.

It does nearly the same things as **phpcbf**, namely fix PSR rules, but it does it better and deeper. Like phpcbf, it also allow you to add some other rules from a list of [172 rules](https://github.com/FriendsOfPhp/PHP-CS-Fixer#usage). The other cool thing about it is the creation of the ruleset file which is a PHP file and not a verbose XML.

#### <i class="fas fa-terminal"></i> php-cs-fixer installation 💡

```shell
composer require friendsofphp/php-cs-fixer
```

#### <i class="fas fa-terminal"></i> php-cs-fixer usage 🆘

```shell
php-cs-fixer --verbose fix MaClasse.php
Loaded config default from "~/.php_cs.dist".
Using cache file ".php_cs.cache".
Paths from configuration file have been overridden by paths provided as command arguments.
F
Legend: ?-unknown, I-invalid file syntax, file ignored, S-Skipped, .-no changes, F-fixed, E-error
   1) MaClasse.php (class_attributes_separation, no_spaces_inside_parenthesis, declare_strict_types, blank_line_after_opening_tag, array_syntax, class_definition, 
function_declaration, visibility_required, declare_equal_normalize, binary_operator_spaces, braces)

Checked all files in 0.008 seconds, 8.000 MB memory used
```

Here is the output on MaClasse.php with a custom ruleset:

```php
declare(strict_types = 1);
class MaClasse
{
    public $monAttribut = [];

    public function __construct($monAttribut)
    {
        $this->setMonAttribut($monAttribut);
    }

    public function setMonAttribut($monAttribut)
    {
        $this->monAttribut = $monAttribut;
        $test = 1;
    }
}
```

And here is my custom ruleset (.php_cs.dist) :

```php
$finder = PhpCsFixer\Finder::create()
    ->exclude('vendor')
    ->exclude('output')
    ->exclude('releases')
    ->in(__DIR__)
;

return PhpCsFixer\Config::create()
    ->setRiskyAllowed(true)
    ->setRules([
        '@PSR2' => true,
        'declare_strict_types' => true,
        'align_multiline_comment' => ['comment_type' => 'phpdocs_only'],
        'binary_operator_spaces' => true,
        'blank_line_after_opening_tag' => true,
        'cast_spaces' => true,
        'class_attributes_separation' => true,
        'compact_nullable_typehint' => true,
        'concat_space' => ['spacing' => 'one'],
        'declare_equal_normalize' => ['space' => 'single'],
        'function_typehint_space' => true,
        'no_blank_lines_after_class_opening' => true,
        'no_blank_lines_after_phpdoc' => true,
        'no_extra_consecutive_blank_lines' => true,
        'no_mixed_echo_print' => true,
        'no_php4_constructor' => true,
        'no_unused_imports' => true,
        'no_useless_else' => true,
        'no_useless_return' => true,
        'no_whitespace_before_comma_in_array' => true,
        'no_whitespace_in_blank_line' => true,
        'object_operator_without_whitespace' => true,
        'ordered_class_elements' => true,
        'ordered_imports' => true,
        'phpdoc_align' => true,
        'phpdoc_indent' => true,
        'yoda_style' => true,
        'protected_to_private' => true,
        'return_type_declaration' => true,
        'self_accessor' => true,
        'single_blank_line_before_namespace' => true,
        'single_quote' => true,
        'ternary_operator_spaces' => true,
        'ternary_to_null_coalescing' => true,
        'whitespace_after_comma_in_array' => true,
        'array_syntax' => ['syntax' => 'short'],
    ])
    ->setFinder($finder)
;
```

Some of these rules are so called _risky_ because they can impact the PHP's runtime, so use it carefully! On my project, I use two of them:
- `declare_strict_types` (type mode is set to `strict` instead of `weak`)
- `no_php4_constructor` (constructors **MUST** be named `__construct`)

You can disable this with the setter `setRiskyAllowed(false)`.

The `declare_strict_types` rule is particularly interesting because it adds in every PHP files' header the following line: `declare(strict_types = 1);`

This PHP7 statement allows strictly typing function parameters and return type for scalar types (int, array, string, bool, ...). It means that if you provide a function with a `string` instead of an expected `int`, it will raise an error instead of casting the `string` to an `int` (e.g. "7 times" => 7).

---

Closing on the tooling, you must consider it as an integral part of your app, that is to say that one hand, they're listed in your project's dependencies, and on the other hand, their configuration files and rulesets are also part of your codebase:

![php_repo](https://thepracticaldev.s3.amazonaws.com/i/kxwp91bkwtg2mz2rxwf2.png)

This is true for PHP, but also for any kind of language. This is the only way to have an *homogeneous codebase* over time within your team.


### <i class="fas fa-terminal"></i> To go further 🛠️

- [phan](https://github.com/phan/phan) : a powerful code static analyzer
- [phpstan](https://github.com/phpstan/phpstan) : PHP Static Analyzis Tool
- [PHP metrics](https://www.phpmetrics.org/) : (yet) another PHP static analyzer that outputs some user-friendly reports
- [PHP copy/paste detector](https://github.com/sebastianbergmann/phpcpd) (aka phpcpd) : a duplicate code detector
- [SonarPHP](https://github.com/SonarSource/sonar-php) : a static PHP code analyzer for SonarQube and SonarLint
- [phploc](https://github.com/sebastianbergmann/phploc) : a tool that gives you statistics on your PHP project

---

🌟 Please feel free to ask me anything on this topic!

⏭️ In the next part, we'll be talking about integrating these tools in your project's lifecycle (git, editor/IDE, CI).

---

Useful links:
- [PHP Standards Recommendations](https://www.php-fig.org/psr)
- [PHP: The Right Way](https://www.phptherightway.com)
