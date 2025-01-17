<!-- Filename: J4.x:J4_Plugin_example_-_Table_of_Contents / Display title: Voorbeeld: Inhoudsopgave -->

## Introductie

Dit artikel biedt een voorbeeld van een inhoudsplug-in om codering toe te lichten met behulp van de nieuwste Joomla-coderingsconventies. De volledige code is beschikbaar op [GitHub](https://github.com/ceford/cefjdemos-plg-content-toc). Je kunt het downloaden en installeren of gewoon uitpakken om de code te inspecteren.

Er is een sectie over [Pluginontwikkeling](https://manual.joomla.org/docs/building-extensions/plugins/) en meer voorbeelden in de Joomla Programmers Documentatie, [gekopieerd naar deze site](jdocmanual?article=docus/plugins/index) voor uw gemak.

Deze plugin genereert een *Inhoudsopgave* van de koppen in elk artikel dat een `{cefjdemostoc}` placeholder bevat. Het resultaat wordt geïllustreerd in de laatste screenshot in dit artikel.

## Broncode Structuur

Het volgende is een schematische illustratie van de structuur van de broncode die wordt gebruikt om de extensie te maken:

```
cefjdemos-plg-toc
|-- .vscode (folder for build settings)
|-- plg_content_cefjdemostoc (folder compressed to create an installable zip file)
    |-- language/en-GB/ (language folder, kept with the extension code)
        |-- plg_content_cefjdemostoc.ini
        |-- plg_content_cefjdemostoc.sys.ini
    |-- media
        |-- css
            |-- cefjdemostoc.css (layout styles)
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Extension (folder for extension specific code)
            |-- Cefjdemostoc.php (the extension class code)
    |-- tmpl (folder for layouts)
        |-- default.php (the table of contents layout)
    |-- cefjdemostoc.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
```

Dit is de structuur zoals te zien in de VSCode of VSCodium IDE:

![Plugin development file structure in vscodium](../../../en/images/plugins/cefjdemostoc-vscodium.png)

## Het Manifestbestand

Elke extensie moet een manifestbestand in xml-indeling hebben. Het wordt door Joomla gebruikt voor installatie-, update- en verwijderdoeleinden. Dit is het manifestbestand voor deze extensie:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<extension type="plugin" group="content" method="upgrade">
    <name>plg_content_cefjdemostoc</name>
    <author>Clifford E Ford</author>
    <creationDate>2024-12-23</creationDate>
    <copyright>(C) 2024 Clifford E Ford</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <authorEmail>cliff@xxxx.yyyy.co.uk</authorEmail>
    <authorUrl>jdocmanual.org</authorUrl>
    <version>0.2.2</version>
    <description>PLG_CONTENT_CEFJDEMOSTOC_XML_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Plugin\Content\Cefjdemostoc</namespace>
    <files>
        <folder plugin="cefjdemostoc">services</folder>
        <folder>src</folder>
        <folder>language</folder>
    </files>
    <media destination="plg_content_cefjdemostoc" folder="media">
        <folder>css</folder>
    </media>
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-plg-content-toc/main/changelog.xml</changelogurl>
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" priority="2" name="Cefjdemostoc Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-plg-content-toc/main/updates.xml</server>
    </updateservers>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="help"
                    type="list"
                    label="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_LABEL"
                    description="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_DESC"
                    default="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_DEFAULT"
                >
                    <option value="">PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_DEFAULT</option>
                </field>

                <field
                    name="toc_class"
                    type="text"
                    label="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_CLASS_LABEL"
                    description="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_CLASS_DESC"
                />

                <field
                    name="toc_head_class"
                    type="list"
                    label="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_HEAD_CLASS_LABEL"
                    description="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_HEAD_CLASS_DESC"
                    default="center"
                >
                    <option value="text-center">text-center</option>
                    <option value="text-start">text-start</option>
                </field>
            </fieldset>
        </fields>
    </config>
</extension>
```

## Het bestand services/provider.php

Het doel van dit bestand is om een serviceprovider te registreren en eventuele benodigde tools te declareren. Deze specifieke plugin heeft niet veel nodig. Als je aangepaste plugin databasequeries moest uitvoeren, zou je het hier declareren. Bekijk een authenticatieplugin. Zij vereisen database- en gebruikershulpmiddelen.

```php
<?php

/**
 * @package     Cefjdemostoc.Plugin
 * @subpackage  Content.cefjdemostoc
 *
 * @copyright   (C) 2024 Clifford E Ford <https://jdocmanual.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\PluginInterface;
use Joomla\CMS\Factory;
use Joomla\CMS\Plugin\PluginHelper;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;
use Joomla\Event\DispatcherInterface;
use Cefjdemos\Plugin\Content\Cefjdemostoc\Extension\Cefjdemostoc;

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
                $plugin     = new Cefjdemostoc(
                    $container->get(DispatcherInterface::class),
                    (array) PluginHelper::getPlugin('content', 'cefjdemostoc')
                );
                $plugin->setApplication(Factory::getApplication());

                return $plugin;
            }
        );
    }
};
```

## De Extensieklasse

Het doel van het src/Extension/Cefjdemostoc.php-bestand is om gegevens voor output voor te bereiden. Het bestand is 91 regels lang en wordt hier daarom niet gereproduceerd. Echter, sommige delen ervan verdienen verdere uitleg.

### Codesniffer Richtlijnen

Regels 16 tot 18 hebben dit:

```php
// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects
```

De becommentarieerde regels voorkomen foutmeldingen van de CodeSniffer. Ze dienen geen ander doel.

### openbare functie onContentPrepare()

Dit is de gebeurtenis waarop de plugin reageert. Er wordt een *context*, de artikelgegevens en de artikelparameters doorgegeven. Een *Inhoudsopgave* moet alleen worden gegenereerd als de gerenderde pagina een enkel artikel is. Anders moet de `{cefjdemostoc}` placeholder worden verwijderd.

Om een inhoudsopgave te genereren, worden enkele artikelparameters ingesteld en wordt elke kop in het artikel gewijzigd om een volgnummer **id** toe te voegen, zodat `<h2>Introduction</h2>` wordt `<h2 id="cefjemostoc0">Introduction</h2>`. Vervolgens wordt een sjabloon geladen om de weergegeven inhoudsopgave toe te voegen.

### Het tmpl/default.php-bestand

Het doel van dit bestand is om de uitvoer weer te geven met de verstrekte gegevens. Het bestand kan worden overschreven en je zult het zien tussen de **plugins/contents** overschrijvingen voor de Cassiopeia-sjabloon.

Het bestand wordt hieronder weergegeven. Merk op dat sommige elementen hardcoded CSS-klassen hebben en sommige verkregen worden uit de artikelparameters. De **fs-** klassen zijn Bootstrap-lettergroottes. De koppen worden opgeslagen in de `$headings2` array.

```php
<?php

/**
 * @package     Cefjdemostoc.Plugin
 * @subpackage  Content.cefjemostoc
 *
 * @copyright   (C) 2024 Clifford E Ford.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

defined('_JEXEC') or die;

use Joomla\CMS\Language\Text;

/**
 * @var Joomla\CMS\WebAsset\WebAssetManager $wa
 * @var \Joomla\Plugin\Content\Cefjdemostoc\Extension\Cefjdemostoc $this
 */
$wa = $this->getApplication()->getDocument()->getWebAssetManager();
$wa->registerAndUseStyle('plg_content_cefjdemostoc', 'plg_content_cefjdemostoc/cefjdemostoc.css');

?>
<div class="card cefjdemostoc <?php echo $toc_class; ?>">
    <div class="card-header">
        <div class="<?php echo $toc_head_class; ?> fs-2">
            <?php echo Text::_('PLG_CONTENT_CEFJDEMOSTOC_CONTENTS'); ?>
        </div>
    </div>
    <div class="card-body">
        <ul class="list-group">
            <?php foreach ($headings2 as $i => $heading) : ?>
                <li class="list-group-item fs-<?php echo $heading[1] + 2; ?> ps-<?php echo $heading[1] + 2; ?>">
                    <a href="#cefjemostoc<?php echo $i; ?>" class="link-underline-light">
                        <?php echo $heading[2]; ?>
                    </a>
                </li>
            <?php endforeach; ?>
        </ul>
    </div>
</div>
```

#### De Webmiddelbeheerder

Zonder extra opmaak zou de inhoudsopgave (ToC) de volledige breedte van het scherm innemen op de locatie van de `{cefjemostoc}` placeholder in de brontekst. Dat wordt soms gezien in technische publicaties, maar het is misschien niet wat je wilt. Op brede schermen is het vaak beter om de ToC te laten zweven en de artikeltekst eromheen te laten vloeien. Dat kan echter onbruikbaar worden op smalle schermen. De beste oplossing is om een stijlblad met aangepaste mediavragen te voorzien. Op brede schermen zweeft de ToC naar rechts, maar op smalle schermen niet.

De `default.php` code hierboven laat zien hoe een aangepaste stijlsheet voor de plugin te includeren. De CSS bevindt zich in het bestand `media/css/cefjdemostoc.css`. De waarden daarin zijn ter illustratie van deze tutorial.

## Resultaat

![The resulting table of contents](../../../en/images/plugins/cefjdemostoc-result.png)

*Vertaald door openai.com*

