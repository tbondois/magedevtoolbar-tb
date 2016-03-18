
# Magento 1.x dev toolbar + FirePHP

## Quick Changes :

* The magento debug toolbar is a fork of [magneto-debug](https://github.com/madalinoprea/magneto-debug) + inclusion of FirePHP for your magento 1.x project.

* no need of installation via modman. Just copy/paste files to your magento root path

* Insert in database table `sheep_debug_report_info` only Sql Profiler is enabled

* no header menu

* Look for `@fixes` to see all my changes


## Quick install guide :


* copy files/folders from your project root

* Add to your .gitignore these lines:

`
/app/code/community/Sheep/Debug/

/app/etc/modules/Sheep_Debug.xml

/app/design/frontend/base/default/layout/sheep_debug.xml

/app/design/frontend/base/default/template/sheep_debug/

/skin/frontend/base/default/sheep_debug/

/app/design/adminhtml/default/default/layout/sheep_debug.xml

/app/design/adminhtml/default/default/template/sheep_debug/

/skin/adminhtml/default/default/sheep_debug/

/vendor/firephp/
`


#### Debug toolbar

With magerun :

* `magerun cache:clean`

* `magerun sys:setup:run`

#### Firephp lib

* Use Firefox and plugins Firebug + FirePHP

* Inclusion of library for DeveloperMode

Add in the end of your `/index.php` :


	if (Mage::getIsDeveloperMode()) {
		include_once __DIR__ .'/vendor/firephp/firephp-core/lib/FirePHPCore/fssb.php';
	} elseif (!function_exists('fb')) {
		function fb() {};
	}


## Quick disable guide

delete / comment content in `/app/etc/modules/Sheep_Debug.xml`


# Original Magnero/debug README :


[![Build Status](https://travis-ci.org/madalinoprea/magneto-debug.svg?branch=master)](https://travis-ci.org/madalinoprea/magneto-debug) [![Coveralls](https://coveralls.io/repos/github/madalinoprea/magneto-debug/badge.svg?branch=master)](https://coveralls.io/github/madalinoprea/magneto-debug)

This repository represents an extension for Magento 1.x that offers a developer debug toolbar. The idea came from robhudson's [django-debug-toolbar](https://github.com/robhudson/django-debug-toolbar). Latest version is based on Symfony's WebProfilerBundle UI.

![Toolbar](docs/images/frontend_toolbar_request.png)


# Features 
- **Request and Controller information**: lists request attributes and controller that handled the request; captures request info for Ajax and POST requests
- **Execution Timeline**: shows execution timeline based on Varien Profiler timers
- **Logs**: shows log lines added to system and exception logs during a request
- **Events**: shows all events dispatched during request and observers that were called
- **Database**: lists all models and collections loaded during a request; when SQL profiler is enabled, lists all SQL queries executed and offers the ability to see their result or describe their execution plan
- **E-mails**: lists sent e-mails with preview
- **Layout**: outputs rendering tree, lists layout handlers loaded during current request and adds ability to see updated added by layout files to specific handle; offers information about instantiated and rendered blocks
- **Configuration**: lists available Magento modules with their status and their version; 
 also offers the ability to enable/disable them
- **Toolbar Tools**: contains quick links to flush cache, enable template hints, enable SQL profiler, enable Varien Profiler, enable Magento Enterprise full page cache debug

Don't forget to check out [screenshots gallery](docs/images.md)


# Installation 

## Using Modman

- Make sure you have [Modman](https://github.com/colinmollenhour/modman) installed
- Allow symlinks for the templates directory (required for installations via Modman)
    - Use n98-magerun like pro: `n98-magerun.phar dev:symlinks`
    - Or just set 'Allow Symlinks' to 'Yes' from System - Configuration / Advanced / Developer / Template Settings

- Install Debug Toolbar module:
```bash
cd [magento root folder]
modman init
modman clone https://github.com/madalinoprea/magneto-debug.git
```
- Flush Magento's cache 

### How to update
I'm pretty lazy and I don't like to create Magento Connect packages. With modman you can effortlessly grab latest changes from github.
```
cd [magento root folder]
modman update magneto-debug
```
- Flush Magento's cache

## Using composer

Lately, I've been more into composer and thanks to this project https://github.com/Cotya/magento-composer-installer you can use it for your Magento 1.x websites.

```
# Add package as requirement to composer.json
composer require madalinoprea/magneto-debug
# Clear cache and voila..
```


# Change Log

All released versions can be found on [releases' page](https://github.com/madalinoprea/magneto-debug/releases). 

- Latest version: [**1.6.2**](https://github.com/madalinoprea/magneto-debug/releases/latest)
   - Fixes e-mail capture for Magento CE 1.7 and Magento EE 1.12 #75


# Issues, Ideas or Feedback

Please don't be afraid to use [issue tracker on GitHub](https://github.com/madalinoprea/magneto-debug/issues) to report issues, ideas or any feedback. Also I encourage you to send pull requests. I'll review them, change them a bit and make sure unit tests are ok (pedantic :older_man:).


# Roadmap

[![Stories in Ready](https://badge.waffle.io/madalinoprea/magneto-debug.png?label=ready&title=Ready)](https://waffle.io/madalinoprea/magneto-debug)

My goal is to have weekly releases with some meaningful features. To stay focused I use a Scrum board that shows backlog, selected work for current interation and progress.

Hot fix versions are released as soon as possible, outside of our weekly release schedule and they are triggered by some :crying_cat_face:-astrophic issue.


# Compatibility

[![Aggregated Build Status](https://travis-ci.org/madalinoprea/magneto-debug.svg)](https://travis-ci.org/madalinoprea/magneto-debug)

Extension is (hopefully) successfully unit tested against PHP 5.4, PHP 5.5 and Magento CE 1.9, Magento CE 1.8, Magento CE 1.7 and 
their related Magento Enterprise versions.

If you would like to support it on another version let us know.


# Common Issues

- 'Mage Registry key already exists' exception is raised after installation
    - `Mage registry key "_singleton/debug/observer" already exists` is reported when cache regeneration was corrupted. 
    Please try to flush Magento cache.

- Doesn'work. I see these logs on `var/log/system.log`: `Not valid template file:adminhtml/default/default/template/sheep_debug/toolbar.phtml`
    - If you installed the module using modman you've missed an important step. Search this page after 'Allow symlinks for the templates directory' and complete that step.  	
  
- I can't see toolbar.
    - Toolbar is displayed in these conditions:
        - module is installed and enabled
        - toolbar is enabled from Admin / System / Configuration / Advanced - Developer Debug Toolbar (by default it's enabled)
        - Magento is running in developer mode (MAGE_IS_DEVELOPER_MODE) Or your ip is listed under under 'Developer Client Restrictions'
    - Check that module name Sheep_Debug is installed and enabled
    - Check that 'Allow Symlinks' configuration is enabled for Modman installation

- I can't see toolbar on specific page
    - Toolbar is added to all pages that have a structural block named `before_body_end`. By default this block is available on all Magento pages.
    Eliminate a possible cache problem by disabling all caches. Try to determine if there are any customizations that have removed `before_body_end`.




# Authors, contributors

- [Mario O](https://twitter.com/madalinoprea)
- [Other contributors](https://github.com/madalinoprea/magneto-debug/graphs/contributors)


# License

[MIT License](LICENSE.txt)

