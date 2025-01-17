<!-- Filename: J4.x:MVC_Anatomy:_Administrator_Startup_Files / Display title: MVC Anatomie: Administrator opstartbestanden -->

## Overzicht van Administratorbestanden

Er zijn meer bestanden in het beheerdersgedeelte van de component, deels omdat er twee tabellen zijn, elk met een lijst- en bewerkweergave, en deels omdat sommige bestanden aspecten van het componentgedrag in zowel de site- als beheerdersinterfaces beheersen.

De creatie van code voor een lijstweergave is behandeld in het deel van de tutorial dat betrekking heeft op de sitebestanden, dus zal hier niet herhaald worden. Deze sectie behandelt de bestanden die gemeenschappelijk zijn voor de meeste componenten. Een volgende tutorial zal één van de bewerkingsweergaven behandelen die nodig zijn om nieuwe records bij te werken of toe te voegen.

## services/provider.php

Dit bestand wordt gebruikt wanneer de Joomla-toepassing de ComponentHelper aanroept om de component weer te geven. Het is de allereerste componentcode die wordt uitgevoerd voor zowel Site als Beheerder. De rol ervan is om de component te registreren:

```php
    public function register(Container $container)
    {
        $container->registerServiceProvider(new MVCFactory('\\Cefjdemos\\Component\\Countrybase'));
        $container->registerServiceProvider(new ComponentDispatcherFactory('\\Cefjdemos\\Component\\Countrybase'));
        $container->registerServiceProvider(new RouterFactory('\\Cefjdemos\\Component\\Countrybase'));
        $container->set(
            ComponentInterface::class,
            function (Container $container) {
                $component = new CountrybaseComponent($container->get(ComponentDispatcherFactoryInterface::class));
                $component->setMVCFactory($container->get(MVCFactoryInterface::class));
                $component->setRouterFactory($container->get(RouterFactoryInterface::class));

                return $component;
            }
        );
    }
```

## src/Extensie/CountrybaseComponent.php

De registerfunctie van dit bestand wordt aangeroepen vanuit de \$component = new CountrybaseComponent(...); instructie in services/provider.php. Het doel ervan is om de component op te starten in zowel de Site- als de Beheerderinterfaces.

```php
       public function boot(ContainerInterface $container)
        {
        }
```

## access.xml

Dit bestand stelt de toegangscontroles in die in deze component worden gebruikt. De hier vermelde items verschijnen in het tabblad Opties Machtigingen van de component.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<access component="com_countrybase">
    <section name="component">
        <action name="core.admin" title="JACTION_ADMIN" />
        <action name="core.options" title="JACTION_OPTIONS" />
        <action name="core.manage" title="JACTION_MANAGE" />
        <action name="core.create" title="JACTION_CREATE" />
        <action name="core.delete" title="JACTION_DELETE" />
        <action name="core.edit" title="JACTION_EDIT" />
    </section>
</access>
```

## config.xml

Dit bestand wordt gebruikt om te bepalen of er opties verschijnen in het component *Opties*-formulier en onder welk tabblad ze verschijnen. Voor com_countrybase zijn er geen opties behalve de standaard Joomla-machtigingsopties. Het zou mogelijk zijn om hier opties toe te voegen, bijvoorbeeld of een bepaalde kolom in de uitvoerweergave getoond of verborgen moet worden. Dat zou in een apart fieldset genaamd Opties gaan.

```xml
<?xml version="1.0" encoding="utf-8"?>
<config>

    <fieldset
        name="permissions"
        label="JCONFIG_PERMISSIONS_LABEL"
        description="JCONFIG_PERMISSIONS_DESC"
        >

        <field
            name="rules"
            type="rules"
            label="JCONFIG_PERMISSIONS_LABEL"
            filter="rules"
            validate="rules"
            component="com_countrybase"
            section="component"
         />
    </fieldset>
</config>
```

*Vertaald door openai.com*

