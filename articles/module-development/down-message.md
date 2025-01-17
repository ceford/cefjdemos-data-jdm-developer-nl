<!-- Filename: J4.x:J4_Module_example_-_Mydownmsg / Display title: Voorbeeld: Site niet beschikbaar bericht -->

## Introductie

Dit artikel presenteert een complete Joomla-site-module voor nieuwe ontwikkelaars om uit elkaar te halen en zo de structuur en werking ervan te begrijpen. Het is een vervanging voor een eerdere module die oudere coderingsconventies gebruikte. Deze is ontworpen voor Joomla 5 en later.

Er zijn inleidende artikelen over *Algemene Concepten* en *Modules* in de Joomla Programmeurs Documentatie en gereproduceerd in Jdocmanual voor uw gemak. Dit artikel is vergelijkbaar met de JPD [Basismodule](jdocmanual?article=docus/modules/basic-module) maar heeft wat extra functionaliteit.

## Doel

De module is ontworpen om een tijdelijke boodschap in een van de verschillende talen weer te geven voor een korte periode voordat de site sluit voor systeemonderhoud. Dat geeft gebruikers de gelegenheid om af te maken waar ze mee bezig zijn voordat de sluiting plaatsvindt.

De module naam is `mod_downmsg` en het bericht dat het in het Engels weergeeft is vergelijkbaar met *Deze site sluit voor een korte periode om 12:00 GMT*. De tijd en het bericht kunnen worden geselecteerd om te passen bij de aanstaande sluiting van de site.

De module kan worden gedownload van GitHub om te installeren of te onderzoeken zoals nodig.

## Structuur van de Broncode

Het volgende is een schematische illustratie van de structuur van de broncode die gebruikt wordt om de extensie te maken:

```
cefjdemos-plg-toc
|-- .vscode (folder for build settings)
|-- mod_downmsg (folder compressed to create an installable zip file)
    |-- help
        |-- en-GB
            |-- downmsg.html (this is Help screen used in the backend module edit form)
    |-- language
        |-- de-DE
            |-- mod_downmsg.ini (only frontend strings are included)
        |-- en-GB (language folder, kept with the extension code)
            |-- mod_downmsg.ini
            |-- mod_downmsg.sys.ini
        |-- fr-FR
            |-- mod_downmsg.ini (only frontend strings are included)
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Dispatcher (folder for extension specific code)
            |-- Dispatcher.php (the extension class code)
    |-- tmpl (folder for layouts)
        |-- default.php (the table of contents layout)
    |-- mod_downmsg.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- changelog.xml (record of changes)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
|-- updates.xml (update server specification)
```

Dit is de structuur zoals te zien in de VSCode of VSCodium IDE:

![Plugin development file structure in vscodium](../../../en/images/modules/downmsg-module-vscodium.png)

## Het Manifestbestand

Het manifestbestand beheert de installatie-, update- en deïnstallatieprocessen van de extensie:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<extension type="module" method="upgrade" client="site">
    <name>mod_downmsg</name>
    <creationDate>December 2024</creationDate>
    <author>Clifford E Ford</author>
    <authorEmail>cliff@xxx.yyy.co.uk</authorEmail>
    <authorUrl>http://jdocmanual.org/</authorUrl>
    <copyright>Copyright (C) 2024 Clifford E Ford, All rights reserved.</copyright>
    <license>GNU/GPLv3 http://www.gnu.org/licenses/gpl-3.0.html</license>
    <!--  The version string is recorded in the components table -->
    <version>0.1.0</version>
    <!-- The description is optional and defaults to the name -->
    <description>MOD_DOWNMSG_XML_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Module\Downmsg</namespace>
    <files>
        <folder>help</folder>
        <folder>language</folder>
        <folder module="mod_downmsg">services</folder>
        <folder>src</folder>
        <folder>tmpl</folder>
    </files>
    <help url="MOD_DOWNMSG_HELP_URL" />
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-mod-downmsg/main/changelog.xml</changelogurl>
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" priority="2" name="Mod Downmsg Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-mod-downmsg/main/updates.xml</server>
    </updateservers>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="msg_id"
                    type="list"
                    label="MOD_DOWNMSG_PARAMS_MSG_ID_LABEL"
                    description="MOD_DOWNMSG_PARAMS_MSG_ID_DESC"
                    default="v1"
                    required="true"
                >
                    <option value="v1">MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V1</option>
                    <option value="v2">MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V2</option>
                </field>
                <field
                    name="hour"
                    type="number"
                    label="MOD_DOWNMSG_PARAMS_HOUR_LABEL"
                    description="MOD_DOWNMSG_PARAMS_HOUR_DESC"
                    default="12"
                    min="0"
                    max="23"
                    required="true"
                />
                <field
                    name="minute"
                    type="number"
                    label="MOD_DOWNMSG_PARAMS_MINUTE_LABEL"
                    description="MOD_DOWNMSG_PARAMS_MINUTE_DESC"
                    default="00"
                    min="00"
                    max="59"
                    required="true"
                />
                <field
                    name="tz"
                    type="number"
                    label="MOD_DOWNMSG_PARAMS_TZ_LABEL"
                    description="MOD_DOWNMSG_PARAMS_TZ_DESC"
                    default="0"
                    min="-11"
                    max="11"
                    required="true"
                />
            </fieldset>
        </fields>
    </config>
</extension>
```

### Elementnotities

- De **extensie** attributen:
    - **type="module"** geeft het type extensie aan.
    - **method="upgrade"** betekent dat deze extensie kan worden geïnstalleerd en geüpgraded als er nieuwere versies beschikbaar komen.
    - **client="site"** geeft Joomla aan dat dit een *Site* module is in plaats van een *Administrator* module.
- De **versie** verklaring wordt gebruikt om upgrades te controleren. Als er een updateserver beschikbaar is, wordt deze vergeleken met de daar geregistreerde versie. Het voorkomt ook dat een oudere versie een nieuwere versie overschrijft.
- Het **namespace** padattribuut vertelt Joomla waar te zoeken naar namespace-klassen.
- De **bestanden** sectie somt mappen en bestanden op voor installatie. Als een item hier niet aanwezig is, zal het niet worden geïnstalleerd, zelfs als het aanwezig is in het gecomprimeerde installatiebestand.
- Het **help** url-attribuut maakt het mogelijk een aangepast helpbestand te gebruiken in het modulebewerkingsformulier. Het pad naar het helpbestand is een waarde voor de gegeven sleutel in het en-GB/mod_downmsg.ini bestand.
- De **config** sectie laat zien welke parameters deze module nodig heeft. Ze moeten allemaal standaardwaarden hebben omdat ze bij installatie worden ingesteld.

## De Taalbestanden

Deze module heeft zeer weinig strings. Degenen in de **sys.ini** bestanden worden gebruikt bij de installatie en om te onder andere modules te vermelden. De **.ini** bestanden worden gebruikt in het beheerdersformulier en in de weergave van de module op de site. Let op de conventies bij het identificeren van strings:

MOD\_\[naam\]\_\[doel\]\_\[gebruik\]

De onderstaande bestanden tonen aan dat de Duitse en Franse vertalingen alleen de tekenreeksen bevatten die zichtbaar zijn voor sitebezoekers, aangezien het Parameters-formulier altijd in het Engels zal worden beheerd. Mocht u het niet weten: en-GB tekenreeksen worden standaard altijd eerst geladen om te voorkomen dat per ongeluk onvertaalde tekenreeksen als tekenreeksleutels verschijnen. De tekenreeksen van de vereiste taal worden vervolgens geladen en overschrijven de equivalente Engelse tekenreeksen.

De Franse en Duitse versies van de zinnen hier zijn verkregen met behulp van Google Translation Tools. Dit kan beter werken voor zinnen dan voor losse woorden!

### en-GB/mod_downmsg.nl.ini

```ini
MOD_DOWNMSG="System Down Message"
MOD_DOWNMSG_XML_DESCRIPTION="Show a message about imminent system down time."

MOD_DOWNMSG_HELP_URL="../modules/mod_downmsg/help/en-GB/downmsg.html"

MOD_DOWNMSG_MSG_V1="This site will close for a short period at %s"
MOD_DOWNMSG_MSG_V2="Please log out. This site will close for one hour or more at %s"

MOD_DOWNMSG_PARAMS_MSG_ID_LABEL="Select Message version"
MOD_DOWNMSG_PARAMS_MSG_ID_DESC="The short downtime or long downtime message vesion."

MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V1="Short < 1 hour"
MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V2="Long > 1 hour"

MOD_DOWNMSG_PARAMS_HOUR_LABEL="Hour"
MOD_DOWNMSG_PARAMS_HOUR_DESC="Set the hour when the site will close"

MOD_DOWNMSG_PARAMS_MINUTE_LABEL="Minute"
MOD_DOWNMSG_PARAMS_MINUTE_DESC="Set the minutes when the site will close"

MOD_DOWNMSG_PARAMS_TZ_LABEL="Time Zone"
MOD_DOWNMSG_PARAMS_TZ_DESC="Set your time zone offset from GMT"
```

### en-GB/mod_downmsg.sys.ini

Dit bestand wordt gebruikt voor systeembeheer.

```ini
MOD_DOWNMSG="System Down Message"
MOD_DOWNMSG_XML_DESCRIPTION="Show a message about imminent system down time."
```

### de-DE/mod_downmsg.ini

```ini
MOD_DOWNMSG_MSG_V1="Diese Seite wird für kurze Zeit um %s geschlossen"
MOD_DOWNMSG_MSG_V2="Bitte melden Sie sich ab. Diese Seite wird für eine Stunde oder länger um %s geschlossen"
MOD_DOWNMSG_HELP_URL="../modules/mod_downmsg/help/en-GB/downmsg.html"
```

### fr-FR/mod_downmsg.ini

```ini
MOD_DOWNMSG_MSG_V1="Ce site sera fermé pour une courte période à %s"
MOD_DOWNMSG_MSG_V2="Veuillez vous déconnecter. Ce site sera fermé pendant une heure ou plus à %s"
MOD_DOWNMSG_HELP_URL="../modules/mod_downmsg/help/en-GB/downmsg.html"
```

## De Modulecode

De modulecode is ongelooflijk eenvoudig. Er zijn drie PHP-bestanden:

- services/provider.php (de code voor dependency injection).
- src/Dispatcher/Dispatcher.php (stelt gegevens samen voor weergave).
- tmpl/default.php (de sjabloon om de gegevens weer te geven, die kan worden overschreven).

### services/provider.php

```php
<?php

/**
 * @package     Cefjdemos.Site
 * @subpackage  mod_downmsg
 *
 * @copyright   (C) 2023 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\Service\Provider\Module;
use Joomla\CMS\Extension\Service\Provider\ModuleDispatcherFactory;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;

/**
 * The module Downmsg service provider.
 *
 * @since  4.4.0
 */
return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.4.0
     */
    public function register(Container $container): void
    {
        $container->registerServiceProvider(new ModuleDispatcherFactory('\\Cefjdemos\\Module\\Downmsg'));
        $container->registerServiceProvider(new Module());
    }
};
```

### src/Dispatcher/Dispatcher.php

```php
<?php

/**
 * @package     Cefjdemos.Site
 * @subpackage  mod_downmsg
 *
 * @copyright   (C) 2023 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace Cefjdemos\Module\Downmsg\Site\Dispatcher;

use Joomla\CMS\Dispatcher\AbstractModuleDispatcher;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

/**
 * Dispatcher class for mod_downmsg
 *
 * @since  4.4.0
 */
class Dispatcher extends AbstractModuleDispatcher
{
    /**
     * Returns the layout data.
     *
     * @return  array
     *
     * @since   4.4.0
     */
    protected function getLayoutData()
    {
        $data = parent::getLayoutData();

        return $data;
    }
}
```

### tmpl/default.php

```php
<?php

/**
 * @package     Cefjdemos.Module
 * @subpackage  mod_downmsg
 *
 * @copyright   Copyright (C) 2019 Clifford E Ford. All rights reserved.
 * @license     GNU/GPLv3 http://www.gnu.org/licenses/gpl-3.0.html
 */

defined('_JEXEC') or die;

use Joomla\CMS\Language\Text;

// the $msg string contains a %s placeholder to be replaced in a sprintf statement
$msg = Text::_('MOD_DOWNMSG_MSG_' . strtoupper($params->get('msg_id')));

$tod = $params->get('hour') . ':' . $params->get('minute');
$tz = $params->get('tz');

if ($tz > 0) {
    $tz = '(+' . $tz . ')';
} elseif ($tz < 0) {
    $tz = '(-' . $tz . ')';
} else {
    $tz = '';
}

$tod .= ' GMT ' . $tz;

?>

<div class="alert alert-warning text-center" role="alert">
    <?php echo sprintf($msg, $tod); ?>
</div>
```

## Installatie

Ga vanuit het Administrator menu naar de pagina Systeem / Installeren / Extensies en gebruik het tabblad Pakketbestand uploaden om het zipbestand te uploaden. Navigeer vervolgens naar Inhoud / Site Modules en bewerk de nieuw geïnstalleerde module.

In het bewerkingsformulier van de module:

1. Voer een titel in. Bedenk dat een module meer dan één instantie kan hebben, zodat ze op verschillende locaties kunnen verschijnen of verschillende parameters kunnen hebben.
2. Stel de tijd in waarop de site offline gaat.
3. Stel de tijdzone in (er is waarschijnlijk een betere manier dan gebruik maken van GMT +- n uren).

In de lijst met gemeenschappelijke parameters aan de rechterkant

1. Stel de **Titel** in op **verbergen**.
2. Selecteer een sjabloon **Positie** - in Cassiopeia plaatst **topbar** het bericht boven de paginainhoud, perfect.
3. Stel de **Status** in op **Gepubliceerd**.
4. Selecteer op het tabblad **Menu-toewijzing** **Op alle pagina's**.
5. **Opslaan** en je bent klaar om de weergave van de site te controleren.

![the module edit form](../../../en/images/modules/downmsg-module-edit-form.png)

## Testen

Dit is hoe het bericht in het Engels verschijnt:

![site down message in english](../../../en/images/modules/downmsg-module-result-en.png)

In het Duits:

![site down message in english](../../../en/images/modules/downmsg-module-result-de.png)

En Frans:

![site down message in english](../../../en/images/modules/downmsg-module-result-fr.png)

## Site bijwerken en wijziglogboek

Deze items zijn opgenomen in de bron maar worden hier niet behandeld.

*Vertaald door openai.com*

