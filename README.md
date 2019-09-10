# WordPress Substratum

WordPress boilerplate configured and managed with Composer and PHP dotenv. Based on Roots Bedrock.

- [WordPress Substratum](#wordpress-substratum)
	- [Features](#features)
	- [Requirements](#requirements)
	- [Prerequisites](#prerequisites)
	- [Installation](#installation)
	- [Configuration](#configuration)
		- [Themes](#themes)
		- [Plugins](#plugins)
		- [Constants](#constants)
	- [Directory Structure](#directory-structure)
	- [See Also](#see-also)
	- [Credit](#credit)

## Features

- Improved directory structure
- Dependency management with [Composer](https://getcomposer.org)
- Easy WordPress configuration with environment and constants files
- Environment variables with [PHP dotenv](https://github.com/vlucas/phpdotenv)
- Enhanced security (separated web root and secure passwords with [roots/wp-password-bcrypt](https://github.com/roots/wp-password-bcrypt))

## Requirements

- PHP 5.6+
- Composer

## Prerequisites

[Install Composer](https://getcomposer.org/doc/00-intro.md):

```bash
$ curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer
```

## Installation

```bash
$ composer create-project handpressed/substratum {directory}

$ cd {directory}
```

Replace `{directory}` with the name of your new WordPress project, e.g. its domain name.

## Configuration

Open the `.env` file and add your new project's home URL (`WP_HOME`) and database credentials (`DB_NAME`, `DB_USER`, `DB_PASSWORD`). You can also define the database `$table_prefix` (default is `wp_`) if required.

Set your project's vhost document root to `/path/to/{directory}/web`.

### Themes

Add themes in `web/app/themes` as you would for a normal WordPress install.

### Plugins

[WordPress Packagist](https://wpackagist.org) is already registered in the `composer.json` file so any plugins from the [WordPress Plugin Directory](https://wordpress.org/plugins/) can easily be required.

To require a plugin, add it under the `require` directive in `composer.json` or use `composer require <namespace>/<packagename>` from the command-line. If it's from WordPress Packagist then the namespace is always `wpackagist-plugin`, e.g.:

```bash
$ composer require wpackagist-plugin/wp-optimize
```

Whenever you add a new plugin or update WordPress core, run `composer update` to install your new packages.

The `plugins` and `mu-plugins` directories are `.gitignore`d by default since Composer manages them. If you want to add plugins to those directories that aren't managed by Composer, you need to update `.gitignore` to whitelist them:

`!web/app/plugins/plugin-name`

Note: Some plugins may create files or directories outside of their given scope, or even make modifications to `wp-config.php` and other files in the `app` directory. These files should be added to your `.gitignore` file as they are managed by the plugins themselves, which are managed by Composer. Any modifications to `wp-config.php` that are required should be moved to `conf/wp-constants.php`.

### Constants

Put custom core, theme and plugin constants in `conf/wp-constants.php`.

## Directory Structure

    ├── composer.json             → Manage versions of WordPress, plugins and dependencies
	├── .env       	              → WordPress environment variables (WP_HOME, DB_NAME, DB_USER, DB_PASSWORD required)
    ├── conf                      → WordPress configuration files
    │   ├── wp-constants.php      → Custom core, theme and plugin constants
    │   ├── wp-env-config.php     → Primary WordPress config file (wp-config.php equivalent)
    │   └── wp-salts.php          → Authentication unique keys and salts (auto generated)
    ├── vendor                    → Composer packages (never edit)
    └── web                       → Web root (vhost document root)
        ├── app                   → wp-content equivalent
        │   ├── mu-plugins        → Must-use plugins
        │   ├── plugins           → Plugins
        │   ├── themes            → Themes
        │   └── uploads           → Uploads
        ├── index.php             → Loads the WordPress environment and template (never edit)
        ├── wp-config.php         → Required by WordPress - loads conf/wp-env-config.php (never edit)
	    └── wp                    → WordPress core (never edit)

See [Roots Bedrock documentation](https://roots.io/bedrock/docs/folder-structure/) for further details.

## See Also

[WordPress Multitenancy Boilerplate](https://github.com/handpressed/wp-multitenancy-boilerplate)

## Credit

Inspired by [roots/bedrock](https://github.com/roots/bedrock) and [wpscholar/wp-skeleton](https://github.com/wpscholar/wp-skeleton).