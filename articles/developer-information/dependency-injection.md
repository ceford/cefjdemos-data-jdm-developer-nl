<!-- Filename: J4.x:Dependency_Injection_in_Joomla_4 / Display title: Afhankelijkheidsinjectie -->

## Inleiding

Afhankelijkheidsinjectie (DI) is een softwareontwikkelingstechniek die een klasse van zijn afhankelijkheden voorziet van buitenaf, in plaats van deze intern te creëren. Het verbetert de flexibiliteit, onderhoudbaarheid en testbaarheid van de code. Echter, het is moeilijk te begrijpen!

Er is een uitleg van DI en Dependency Injection Containers in de [Joomla Framework DI](https://github.com/joomla-framework/di/blob/4.x-dev/docs/why-dependency-injection.md) documentatie. Er is ook een [Overzicht](https://github.com/joomla-framework/di/blob/4.x-dev/docs/overview.md) van het gebruik ervan.

De uitleg in de [Dependency Injection](jdocmanual?article=docus/dependency-injection/index) sectie van de Joomla! Programmeursdocumentatie is niet zo gemakkelijk te begrijpen!

## Voorbeelden

Als uw doel is om aan de slag te gaan met het bouwen van een extensie, kunnen de volgende voorbeelden uit de Joomla core-extensies helpen.

De code voor de afhankelijkheidsinjectie bevindt zich in services/provider.php, maar de exacte locatie van dat bestand is afhankelijk van het type extensie.

### Voorbeeld van Component

Het volledige pad naar het bestand services/provider.php is administrator/components/com_example/services/provider.php. Dit voorbeeld is com_tags, dat geen gebruik maakt van categorieën. Als uw component categorieën gebruikt, bekijk dan de code van services/provider.php voor com_content. U heeft verschillende categorie-gerelateerde instructies nodig.

```php
<?php

/**
 * @package     Joomla.Administrator
 * @subpackage  com_tags
 *
 * @copyright   (C) 2018 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Component\Router\RouterFactoryInterface;
use Joomla\CMS\Dispatcher\ComponentDispatcherFactoryInterface;
use Joomla\CMS\Extension\ComponentInterface;
use Joomla\CMS\Extension\Service\Provider\ComponentDispatcherFactory;
use Joomla\CMS\Extension\Service\Provider\MVCFactory;
use Joomla\CMS\Extension\Service\Provider\RouterFactory;
use Joomla\CMS\MVC\Factory\MVCFactoryInterface;
use Joomla\Component\Tags\Administrator\Extension\TagsComponent;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;

/**
 * The tags service provider.
 *
 * @since  4.0.0
 */
return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.0.0
     */
    public function register(Container $container)
    {
        $container->registerServiceProvider(new MVCFactory('\\Joomla\\Component\\Tags'));
        $container->registerServiceProvider(new ComponentDispatcherFactory('\\Joomla\\Component\\Tags'));
        $container->registerServiceProvider(new RouterFactory('\\Joomla\\Component\\Tags'));
        $container->set(
            ComponentInterface::class,
            function (Container $container) {
                $component = new TagsComponent($container->get(ComponentDispatcherFactoryInterface::class));
                $component->setMVCFactory($container->get(MVCFactoryInterface::class));
                $component->setRouterFactory($container->get(RouterFactoryInterface::class));

                return $component;
            }
        );
    }
};
```

### Module Voorbeeld

Dit is de Versiemodule die de Joomla-versie toont in de titelbalk van de beheerderspagina's. De code bevindt zich in administrator/modules/mod_version/services/provider.php.

```php
<?php

/**
 * @package     Joomla.Administrator
 * @subpackage  mod_version
 *
 * @copyright   (C) 2024 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\Service\Provider\HelperFactory;
use Joomla\CMS\Extension\Service\Provider\Module;
use Joomla\CMS\Extension\Service\Provider\ModuleDispatcherFactory;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;

return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   5.1.0
     */
    public function register(Container $container)
    {
        $container->registerServiceProvider(new ModuleDispatcherFactory('\\Joomla\\Module\\Version'));
        $container->registerServiceProvider(new HelperFactory('\\Joomla\\Module\\Version\\Administrator\\Helper'));

        $container->registerServiceProvider(new Module());
    }
};
```

### Plug-in Voorbeeld

Dit is de plugin die contactinformatie levert in inhoudspagina's. De code bevindt zich in plugins/content/contact/services/provider.php

```php
<?php

/**
 * @package     Joomla.Plugin
 * @subpackage  Content.contact
 *
 * @copyright   (C) 2022 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\PluginInterface;
use Joomla\CMS\Factory;
use Joomla\CMS\Plugin\PluginHelper;
use Joomla\Database\DatabaseInterface;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;
use Joomla\Event\DispatcherInterface;
use Joomla\Plugin\Content\Contact\Extension\Contact;

return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.3.0
     */
    public function register(Container $container)
    {
        $container->set(
            PluginInterface::class,
            function (Container $container) {
                $plugin     = new Contact(
                    $container->get(DispatcherInterface::class),
                    (array) PluginHelper::getPlugin('content', 'contact')
                );
                $plugin->setApplication(Factory::getApplication());
                $plugin->setDatabase($container->get(DatabaseInterface::class));

                return $plugin;
            }
        );
    }
};
```

*Vertaald door openai.com*

