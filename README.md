# Sylius Redirect Plugin

[![Latest Version on Packagist][ico-version]][link-packagist]
[![Software License][ico-license]](LICENSE)
[![Build Status][ico-travis]][link-travis]
[![Quality Score][ico-code-quality]][link-code-quality]

<a href="https://sylius.com/plugins/" target="_blank"><img src="https://sylius.com/assets/badge-approved-by-sylius.png" width="100"></a>

Gives you the ability to manage redirects in your Sylius shop.

## Installation


### Step 1: Download the plugin

Open a command console, enter your project directory and execute the following command to download the latest stable version of this plugin:

```bash
$ composer require setono/sylius-redirect-plugin
```

This command requires you to have Composer installed globally, as explained in the [installation chapter](https://getcomposer.org/doc/00-intro.md) of the Composer documentation.


### Step 2: Enable the plugin

Then, enable the plugin by adding it to the list of registered plugins/bundles
in the `config/bundles.php` file of your project:

```php
<?php

return [
    // ...
    
    // Add before SyliusGridBundle
    Setono\SyliusRedirectPlugin\SetonoSyliusRedirectPlugin::class => ['all' => true],
    Sylius\Bundle\GridBundle\SyliusGridBundle::class => ['all' => true],
    
    // ...
];
```

It is **IMPORTANT** to add the plugin before the grid bundle else you will get a an exception saying `You have requested a non-existent parameter "setono_sylius_redirect.model.redirect.class".`

### Step 3: Add configuration
```yaml
# config/routes/setono_sylius_redirect.yaml

setono_sylius_redirect_admin:
    resource: "@SetonoSyliusRedirectPlugin/Resources/config/admin_routing.yaml"
    prefix: /admin
```

```yaml
# config/packages/_sylius.yaml
imports:
    # ...
    
    - { resource: "@SetonoSyliusRedirectPlugin/Resources/config/config.yaml" }
    
    # ...
```
### Step 4: Update database

Use Dotrine migrations to create a migration file and update the database.

```bash
$ bin/console doctrine:migrations:diff
$ bin/console doctrine:migrations:migrate
```

## What it does

This plugin allows you to create new redirections.

Under the new menu entry `Redirection` in `Configuration`, you can manage redirections.

### Redirection entry

An entry is composed by:
* Source url, relative to your website
* Target URL, can be relative or absolute in case you want to redirect to another website
* Permanent or Temporary (This impact the HTTP response code of the redirection, 301 or 302)
* Enabled
* Redirect only if 404 (to manage potentially dead links)

### Security

There is a built-in security when creating/modifying redirection that prevent creating an infinite loop. This work with infinite recursive checking.

A second security is to prevent same source redirection leading to inconstant redirect.

### Points of improvements 

At the moment, there is no automatic redirection created at any point. Though you can easily do so by creating an event listener where needed. And even create a PR in this repo. 

## Contributors
- [Joachim Løvgaard](https://github.com/loevgaard)
- [Stephane Decock](https://github.com/Roshyo)

[ico-version]: https://img.shields.io/packagist/v/setono/sylius-redirect-plugin.svg?style=flat-square
[ico-license]: https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square
[ico-travis]: https://img.shields.io/travis/Setono/SyliusRedirectPlugin/master.svg?style=flat-square
[ico-code-quality]: https://img.shields.io/scrutinizer/g/Setono/SyliusRedirectPlugin.svg?style=flat-square

[link-packagist]: https://packagist.org/packages/setono/sylius-redirect-plugin
[link-travis]: https://travis-ci.org/Setono/SyliusRedirectPlugin
[link-code-quality]: https://scrutinizer-ci.com/g/Setono/SyliusRedirectPlugin

