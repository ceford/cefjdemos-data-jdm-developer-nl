<!-- Filename: J4.x:Creating_a_Simple_Module / Display title: Een eenvoudige module maken -->

Joomla!  4.x \>Handleiding Een eenvoudige module maken voor Joomla 4.x

Dit is een handleiding voor het maken van een eenvoudige module voor
Joomla versie 4.x

## Inleiding

Joomla! 4 biedt vijf type extensies:

- **[Componenten](https://docs.joomla.org/Component "Special:MyLanguage/Component")**
  Een component is een van de hoofdgedeeltes van de site. Een component
  verwerkt zowel gegevensmanipulatie als invoer en opslag in de
  database. Op de meeste sites is een component het belangrijkste
  onderdeel van een pagina. Daar ligt de focus op.
- **[Modules](https://docs.joomla.org/Module "Special:MyLanguage/Module")**
  Modules zijn lichtgewicht en flexibele extensies. Ze worden gebruikt
  voor kleine stukjes van de pagina en zijn over het algemeen niet heel
  complex en zichtbaar voor verschillende componenten. Een module is een
  add-on die de functionaliteit uitbreid. Een module bevindt zich over
  het algemeen op een minder belangrijk gedeelte van de pagina en wordt
  niet gezien als de primaire focus op die pagina. Het kan op
  verschillende posities getoond worden en u kunt zelf kiezen op welk
  actieve menu-items de module wordt weergegeven.
- **[Plug-ins](https://docs.joomla.org/Plugin "Special:MyLanguage/Plugin")**
  Een plug-in manipuleert output die al gegenereerd is door het systeem.
  Over het algemeen werkt het niet als apart onderdeel van de website.
  Het gebruikt data vanuit een andere bron (bijvoorbeeld de content) en
  manipuleert deze data voordat het wordt getoond. Een plug-in werkt
  normaal gesproken achter de schermen.
- **[Talen](https://docs.joomla.org/Languages "Special:MyLanguage/Languages")**
  Waarschijnlijk zijn talen de meest standaard uitbreidingen. In wezen
  bestaan de taalpakket bestanden uit sleutel/waarde paren, die de
  vertaling van statische teststrings verzorgen, toegewezen binnen de
  Joomla! broncode.
- **[Templates](https://docs.joomla.org/Templates "Special:MyLanguage/Templates")**
  Een template is het ontwerp van de Joomla! website.

Joomla! 4 is opgebouwd uit vijf verschillende toepassingen:

- Installatie (gebruikt voor het installeren van Joomla en moet na
  installatie verwijderd worden);
- Administrator (beheergedeelte - gebruikt voor het beheren van de
  inhoud);
- Public of site (voorkant - gebruikt voor het tonen van de inhoud);
- CLI (gebruikt voor toegang tot Joomla via de command line of voor cron
  jobs);
- API (web services - gebruikt voor het maken van API's voor machinaal
  toegankelijke content);

De installatie toepassing wordt één keer gebruikt. Het beheergedeelte en
de voorkant van de site worden gebruikt door het concept van
*componenten* met *modules*. Elke module heeft een enkel toegangspunt in
de modules en bijgevolg de map *administrator/modules*. Dit beginpunt
wordt `mod_modulename / mod_modulename.php` genoemd (het
mod\_-voorvoegsel is een historisch spoor). Het toegangspunt voor de
inlogmodule is bijvoorbeeld */mod_login/mod_login.php*.

### Eisen

U hebt Joomla! 4.x nodig voor deze handleiding (op moment van schrijven
Joomla! 4.0.0-alpha6-dev)

U kunt Joomla! 4 downloaden via
<a href="https://github.com/joomla/joomla-cms/tree/4.0-dev"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">GitHub</a>, op de
<a href="https://developer.joomla.org/nightly-builds.html"
class="external text" target="_blank"
rel="noreferrer noopener">Developer website</a> of u kunt een gratis
website maken op
<a href="https://launch.joomla.org" class="external free"
target="_blank" rel="noreferrer noopener">https://launch.joomla.org</a>.

## Een eenvoudige module maken/Een basis module ontwikkelen - Deel 1

U kunt veel voorbeelden van modules bekijken in de standaard Joomla!
installatie. Bijvoorbeeld:

- Menus
- Laatste nieuws
- Login formulier
- en veel meer.

Deze handleiding legt uit hoe een eenvoudige module gemaakt kan worden.
Door middel van deze handleiding leert u de basis bestandsstructuur van
een Joomla! 4 module. Deze basisstructuur kan dan uitgebreid worden om
uitgebreidere modules te maken.

### Bestandsstructuur

Er zijn een aantal basis-bestanden die worden gebruikt in het standaard
patroon van module ontwikkeling:

- `mod_foo.php` - Dit bestand is de hoofdingang van de module. Het voert
  alle nodige initialisatie routines uit, roept hulp-routines aan om
  noodzakelijke gegevens te verzamelen, en neemt het template op dat de
  module-uitvoer toont.
- `mod_foo.xml` - Dit bestand bevat informatie over de module. Het
  definieert de bestanden die geïnstalleerd moeten worden door de
  Joomla! installer en bepaalt instellingen-parameters van de module.
- `tmpl/default.php` - Dit is de module template. Dit bestand pakt de
  gegevens op die mod_foo.php verzamelt en genereert de HTML die op de
  pagina vertoond wordt.

\* `language/en-GB/en-GB.mod_foo.ini` en
`language/en-GB/en-GB.mod_foo.sys.ini` - Dit zijn de bestanden die de
tekst verzorgen in het United Kingdom Engels.

### mod_foo.php maken

Het `mod_foo.php` bestand zal de volgende taken uitvoeren:

- Importeer de class `ModuleHelper` in het huidige scope (reikwijdte).
  Deze is later nodig voor het weergeven van de output.
- Opnemen van de template om de output weer te geven.

De helper class is geïmporteerd in het huidige bereik aan het begin van
het bestand.

```php
    use Joomla\CMS\Helper\ModuleHelper;
```

Tot slot nemen we de template op om de output weer te geven via

```php
    require ModuleHelper::getLayoutPath('mod_foo', $params->get('layout', 'default'));
```

#### Het volledige mod_foo.php file

Het volledige `mod_foo.php` bestand ziet er als volgt uit:

```php
<?php
/**
 * @package    [PACKAGE_NAME]
 *
 * @author     [AUTHOR] <[AUTHOR_EMAIL]>
 * @copyright  [COPYRIGHT]
 * @license    GNU General Public License version 2 or later; see LICENSE.txt
 * @link       [AUTHOR_URL]
 */

// No direct access to this file
defined('_JEXEC') or die;

use Joomla\CMS\Helper\ModuleHelper;

require ModuleHelper::getLayoutPath('mod_foo', $params->get('layout', 'default'));
```

*Kanttekening*: In Joomla 3.x gebruikte u vaak een regel als
`$moduleclass_sfx = htmlspecialchars($params->get('moduleclass_sfx'));`.
Dit is niet meer nodig. Zie deze PR:
<a href="https://github.com/joomla/joomla-cms/pull/17447"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla/joomla-cms/pull/17447</a>.

De enige regel die we nog niet toegelicht hebben is de eerste regel
`defined('_JEXEC') or die;`. Deze regel zorgt ervoor dat dit bestand
wordt opgenomen vanuit de Joomla applicatie. Dit is nodig om variable
injections en andere potentiële veiligheidsproblemen te voorkomen.

### Het maken van tmpl/default.php

Het `default.php` bestand is de template die de output van de module
weergeeft.

#### Het volledige tmpl/default.php bestand

De code voor het tmpl/default.php bestand is als volgt:

```php
<?php
/**
 * @package    [PACKAGE_NAME]
 *
 * @author     [AUTHOR] <[AUTHOR_EMAIL]>
 * @copyright  [COPYRIGHT]
 * @license    GNU General Public License version 2 or later; see LICENSE.txt
 * @link       [AUTHOR_URL]
 */

// No direct access to this file
defined('_JEXEC') or die;

echo '[PROJECT_NAME]';
```

Een belangrijk punt om op te letten is dat het template bestand dezelfde
scope (reikwijdte) heeft als het `mod_foo.php bestand`. Dat betekend dat
een variabele gedefinieerd in het `mod_foo.php` bestand vervolgens in
het template bestand gebruikt kan worden zonder extra declaraties of
functie-aanroepen.

### mod_foo.php maken

Het `mod_foo.xml` bestand is het installatie bestand. De meeste entries
spreken voor zichzelf.

#### Het volledige mod_foo.xml bestand

De code voor het `mod_foo.xml` bestand ziet er als volgt uit:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="module" version="4.0" client="site" method="upgrade">
    <name>MOD_FOO</name>
    <creationDate>[DATE]</creationDate>
    <author>[AUTHOR]</author>
    <authorEmail>[AUTHOR_EMAIL]</authorEmail>
    <authorUrl>[AUTHOR_URL]</authorUrl>
    <copyright>[COPYRIGHT]</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <version>1.0</version>
    <description>MOD_FOO_XML_DESCRIPTION</description>
    <files>
        <filename module="mod_foo">mod_foo.php</filename>
        <folder>tmpl</folder>
        <folder>language</folder>
        <filename>mod_foo.xml</filename>
    </files>
</extension>
```

### De taalbestanden maken

De bestanden `language/en-GB/en-GB.mod_foo.ini` en
`language/en-GB/en-GB.mod_foo.sys.ini` worden gebruikt om de tekst te
vertalen op de voorkant van de site en in het beheergedeelte.

De code voor `language/en-GB/en-GB.mod_foo.sys.ini` ziet er als volgt
uit:

```ini
    MOD_FOO="[PROJECT_NAME]"
    MOD_FOO_XML_DESCRIPTION="Foo Module"
```

De code voor `language/en-GB/en-GB.mod_foo.ini` ziet er als volgt uit:

```ini
    MOD_FOO="[PROJECT_NAME]"
    MOD_FOO_XML_DESCRIPTION="Foo Module"
```

Het `.sys.ini` bestand wordt gebruikt om de omschrijving van de extensie
bij installatie te vertalen. Het `.ini` bestand wordt gebruikt om de
overige strings en de omschrijving te vertalen wanneer de extensie wordt
bekeken.

Meer informatie over de taalbestanden kunt u
<a href="https://docs.joomla.org/Specification_of_language_files"
class="new"
title="Special:MyLanguage/Specification of language files (page does not exist)">hier</a>
vinden.

## Test uw module

Nu kunt u alle bestanden archiveren in een zip bestand en deze
installeren via de Joomla Extensie manager
Vervolgens kunt u uw module kiezen in de Module manager wanneer u een
nieuwe site module maakt.

<img
src="https://docs.joomla.org/images/thumb/3/30/Moduletutorial1-en.png/700px-Moduletutorial1-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/3/30/Moduletutorial1-en.png/1050px-Moduletutorial1-en.png 1.5x, https://docs.joomla.org/images/3/30/Moduletutorial1-en.png 2x"
data-file-width="1287" data-file-height="627" width="700" height="341"
alt="Moduletutorial1-en.png" /> <img
src="https://docs.joomla.org/images/thumb/c/c6/Moduletutorial2-en.png/700px-Moduletutorial2-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/c/c6/Moduletutorial2-en.png/1050px-Moduletutorial2-en.png 1.5x, https://docs.joomla.org/images/c/c6/Moduletutorial2-en.png 2x"
data-file-width="1287" data-file-height="627" width="700" height="341"
alt="Moduletutorial2-en.png" />

Op de voorkant van uw site ziet u uw module zoals in de volgende
afbeelding is weergegeven.

<img
src="https://docs.joomla.org/images/thumb/8/83/Moduletutorial3-en.png/700px-Moduletutorial3-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/8/83/Moduletutorial3-en.png/1050px-Moduletutorial3-en.png 1.5x, https://docs.joomla.org/images/8/83/Moduletutorial3-en.png 2x"
data-file-width="1287" data-file-height="627" width="700" height="341"
alt="Moduletutorial3-en.png" />

## Conclusie

Modules ontwikkelen voor Joomla! is een redelijk eenvoudig en duidelijk
proces. Met de technieken in deze handleiding kan zonder veel moeite een
eindeloze variatie aan modules ontwikkeld worden.

Hier kunt u de voorbeeld bestanden vinden:

- <a
  href="https://github.com/joomla-extensions/boilerplate/tree/master/module"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla-extensions/boilerplate/tree/master/module</a>

De voorbeeld bestanden uit deze handleiding kunt u hier vinden:

- <a
  href="https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules</a>

## Toevoegen van helper class met gebruik van namespaces - Deel 2

### Eisen

U hebt Joomla! 4.x nodig voor deze handleiding (op moment van schrijven
Joomla! 4.0.0-alpha6-dev)

## Namespaces

Met PHP 5.3, kwamen de namespaces volledig tot hun recht. Al langer
gebruikt in andere programmeertalen, helpen deze kleine structuren ons
nu ook met de duidelijkheid van onze code.

Namespaces zijn aparte gebieden waarin bepaalde logische zaken (in ons
geval, classes, interfaces, functies en constanten) kunnen worden
opgeslagen. Deze gebieden bieden inkapseling van de code en voorkomen
naamconflicten.

Laten we eens kijken hoe Joomla 4 ze gebruikt. We gebruiken hier een
helper bestand voor.

### Bestandsstructuur

We zullen de volgende bestanden maken of aanpassen:

- `Helper/FooHelper.php` - Dit is het bestand dat we hier als voorbeeld
  zullen gebruiken. We moeten dit aanmaken.
- `mod_foo.php` - In dit betand laden we de namespace.
- `tmpl/default.php` - In dit bestand zullen we demonstreren hoe de data
  van een helper kan worden weergegeven.
- `mod_foo.xml` - We maken een nieuwe map met een nieuw bestand. Deze
  moeten tijdens de installatie geïnstalleerd worden. Om dat te doen
  moeten we ze in dit bestand specificeren.

### Aanmaken van Helper/FooHelper.php

Onze nieuwe helper class moet bij een namespace horen. We bereiken dit
met de volgende code. Het is belangrijk dat deze regel aan het begin van
het bestand staat.

```php
    namespace Joomla\Module\Foo\Site\Helper;
```

Vervolgens maken we een eenvoudige class met een eenvoudige methode.
Natuurlijk kunt u hier veel meer doen. Bekijk de core methode van
Joomla. Hier zijn veel voorbeelden. Ze laten u uit de eerste hand zien
welke opties u heeft.

### Het volledige Helper/FooHelper.php bestand

Het complete `FooHelper.php` bestand ziet er als volgt uit:

```php
<?php
/**
 * @package     Joomla.Site
 * @subpackage  mod_foo
 *
 * @copyright   Copyright (C) 2005 - 2019 Open Source Matters, Inc. All rights reserved.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace Joomla\Module\Foo\Site\Helper;

// No direct access to this file
defined('_JEXEC') or die;

/**
 * Helper for mod_foo
 *
 * @since  4.0
 */
class FooHelper
{
	/**
	 * Retrieve foo test
	 *
	 * @param   Registry        $params  The module parameters
	 * @param   CMSApplication  $app     The application
	 *
	 * @return  array
	 */
	public static function getText()
	{
		return 'FooHelpertest';
	}
}
```

### Editing mod_foo.php

Our new helper class is imported to our current scope at the beginning of the file.

```php
    use Joomla\Module\Foo\Site\Helper\FooHelper;
```

Gebruik als laatste het helper bestand om te testen of het correct is
geladenː

```php
    $test  = FooHelper::getText($params, $app);
```

#### Het volledige mod_foo.php bestand

Het volledige `mod_foo.php` bestand ziet er als volgt uit:

```php
<?php
/**
 * @package    [PACKAGE_NAME]
 *
 * @author     [AUTHOR] <[AUTHOR_EMAIL]>
 * @copyright  [COPYRIGHT]
 * @license    GNU General Public License version 2 or later; see LICENSE.txt
 * @link       [AUTHOR_URL]
 */

// No direct access to this file
defined('_JEXEC') or die;

use Joomla\CMS\Helper\ModuleHelper;
use Joomla\Module\Foo\Site\Helper\FooHelper;

$test  = FooHelper::getText();

require ModuleHelper::getLayoutPath('mod_foo', $params->get('layout', 'default'));
```

### Aanpassen van tmpl/default.php

In dit bestand maken we een kleine aanpassing om aan te tonen dat het
helper bestand correct werkt. U hoeft alleen de waarde van de variabele
aan het eind van de huidige output toe te voegen

```php
    echo '[PROJECT_NAME]' . $test;
```

#### Het volledige tmpl/default.php bestand

Het volledige `tmpl/default.php` bestand ziet er als volgt uit:

```php
<?php
/**
 * @package    [PACKAGE_NAME]
 *
 * @author     [AUTHOR] <[AUTHOR_EMAIL]>
 * @copyright  [COPYRIGHT]
 * @license    GNU General Public License version 2 or later; see LICENSE.txt
 * @link       [AUTHOR_URL]
 */

// No direct access to this file
defined('_JEXEC') or die;

echo '[PROJECT_NAME]' . $test;
```

### Aanpassen mod_foo.xml

Allereerst moet er een regel worden toegevoegd zodat de namespace
automatisch wordt ingesteld in Joomla. Na deze wijziging moet u uw
module opnieuw installeren. Een eenvoudige aanpassing in een al
geïnstalleerde module is niet genoeg. Wanneer u op een locale server
ontwikkelt kunt u ook de bestanden libraries/autoload_psr4.php
verwijderen. Dan zal het automatisch gerecreëerd worden. Na het invoegen
en installeren is de namespace bekend bij de loader
`JPATH_LIBRARIES . '/autoload_psr4.php'`.

```xml
<namespace>Joomla\Module\Foo</namespace>
```

Joomla! moet uw helper bestand kopiëren bij het installeren van de
module. Dus Joomla! moet uw helper bestand kennen. Hiervoor moet u de
volgende regel toevoegen aan uw XML-bestand.

```xml
<folder>Helper</folder>
```

#### Het volledige mod_foo.xml bestand

Het volledige `mod_foo.xml` bestand ziet er als volgt uit:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="module" version="4.0" client="site" method="upgrade">
    <name>MOD_FOO</name>
    <creationDate>[DATE]</creationDate>
    <author>[AUTHOR]</author>
    <authorEmail>[AUTHOR_EMAIL]</authorEmail>
    <authorUrl>[AUTHOR_URL]</authorUrl>
    <copyright>[COPYRIGHT]</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <version>1.0</version>
    <description>MOD_FOO_XML_DESCRIPTION</description>
    <namespace>Joomla\Module\Foo</namespace>
    <files>
        <filename module="mod_foo">mod_foo.php</filename>
        <folder>tmpl</folder>
        <folder>Helper</folder>
        <folder>language</folder>
        <filename>mod_foo.xml</filename>
    </files>
</extension>
```

## Test uw module

Nu kunt u alle bestanden archiveren in een zip bestand en deze
installeren via de Joomla Extensie manager
Vervolgens kunt u uw module kiezen in de Module manager wanneer u een
nieuwe site module maakt.

In de voorkant van uw website kunt u de module zien zoals deze in de
volgende afbeelding wordt weergegeven. De tekst uit uw helper bestand
wordt getoond.

<img
src="https://docs.joomla.org/images/thumb/7/72/Moduletutorial23-en.png/700px-Moduletutorial23-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/7/72/Moduletutorial23-en.png/1050px-Moduletutorial23-en.png 1.5x, https://docs.joomla.org/images/7/72/Moduletutorial23-en.png 2x"
data-file-width="1287" data-file-height="627" width="700" height="341"
alt="Moduletutorial23-en.png" />

## Conclusie

Zoals u kunt zien zijn de namespaces niet zo ingewikkeld wanneer u zich
hier een beetje in verdiept. In een groot project kan dit een hoop
voordeel opleveren. We moeten echter naamruimten plannen, omdat dit
anders snel meer tijd kost.

Hier kunt u de voorbeeld bestanden vinden:

- <a
  href="https://github.com/joomla-extensions/boilerplate/tree/master/module"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla-extensions/boilerplate/tree/master/module</a>

De voorbeeld bestanden uit deze handleiding kunt u hier vinden:

- <a
  href="https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules</a>

## Maatwerk: Parameters toevoegen via form fields - Deel 3

### Eisen

U hebt Joomla! 4.x nodig voor deze handleiding (op moment van schrijven
Joomla! 4.0.0-alpha6-dev)

## Maatwerk met form fields en parameters

Formuliervelden creëren een grote mate van maatwerk in Joomla. Ze zijn
de enige manier waarop gebruikers de module aan de eisen van hun site
kunnen aanpassen.

In dit geval gaan we ons eerdere voorbeeld uitbreiden met gebruik van
een url veld voor het invoeren van een domeinnaam die we als link kunnen
gebruiken op de website. Om ervoor te zorgen dat dit geberud gebruiken
we het [URL Form
Field](https://docs.joomla.org/URL_form_field_type "Special:MyLanguage/URL form field type")
type.

Hierna voegen we parameters toe die standaard functionaliteit van Joomla
mogelijk maken.

Laten we eens kijken hoe we die in Joomla 4 gebruiken.

### Bestandsstructuur

We zullen de volgende bestanden aanpassen:

- `mod_foo.xml` - Dit is het bestand waar we de velden aan toevoegen.
- `tmpl/default.php` - In dit betand laten we zien hoe de data uit een
  veld gebruikt kan worden.
- `language/en-GB/en-GB.mod_foo.ini` - Hier gebruiken we de taal string
  zodat het veld correct gelabeld kan worden in verschillende talen.

### Aanpassen mod_foo.xml

Eerst stellen we een custom parameter in.

```xml
<field
    name="domain"
    type="url"
    label="MOD_FOO_FIELD_URL_LABEL"
    filter="url"
/>
```

Vervolgens voegen we Joomla default velden in, zodat we cache,
moduleclass-achtervoegsel en layouts kunnen gebruiken

```xml
<field
    name="layout"
    type="modulelayout"
    label="JFIELD_ALT_LAYOUT_LABEL"
    class="custom-select"
/>

<field
    name="moduleclass_sfx"
    type="textarea"
    label="COM_MODULES_FIELD_MODULECLASS_SFX_LABEL"
    rows="3"
/>

<field
    name="cache"
    type="list"
    label="COM_MODULES_FIELD_CACHING_LABEL"
    default="0"
>
    <option value="1">JGLOBAL_USE_GLOBAL</option>
    <option value="0">COM_MODULES_FIELD_VALUE_NOCACHING</option>
</field>

<field
    name="cache_time"
    type="number"
    label="COM_MODULES_FIELD_CACHE_TIME_LABEL"
    default="0"
/>

<field
    name="cachemode"
    type="hidden"
    default="itemid"
>
    <option value="itemid"></option>
</field>
```

#### Het volledige mod_foo.xml bestand

Het volledige `mod_foo.xml` bestand ziet er als volgt uit:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="module" version="4.0" client="site" method="upgrade">
    <name>MOD_FOO</name>
    <creationDate>[DATE]</creationDate>
    <author>[AUTHOR]</author>
    <authorEmail>[AUTHOR_EMAIL]</authorEmail>
    <authorUrl>[AUTHOR_URL]</authorUrl>
    <copyright>[COPYRIGHT]</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <version>1.0</version>
    <description>MOD_FOO_XML_DESCRIPTION</description>
    <namespace>Joomla\Module\Foo</namespace>
    <files>
        <filename module="mod_foo">mod_foo.php</filename>
        <folder>tmpl</folder>
        <folder>Helper</folder>
        <folder>language</folder>
        <filename>mod_foo.xml</filename>
    </files>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="domain"
                    type="url"
                    label="MOD_FOO_FIELD_URL_LABEL"
                    filter="url"
                />
            </fieldset>
            <fieldset name="advanced">
                <field
                    name="layout"
                    type="modulelayout"
                    label="JFIELD_ALT_LAYOUT_LABEL"
                    class="custom-select"
                />
                <field
                    name="moduleclass_sfx"
                    type="textarea"
                    label="COM_MODULES_FIELD_MODULECLASS_SFX_LABEL"
                    rows="3"
                />
                <field
                    name="cache"
                    type="list"
                    label="COM_MODULES_FIELD_CACHING_LABEL"
                    default="0"
                >
                    <option value="1">JGLOBAL_USE_GLOBAL</option>
                    <option value="0">COM_MODULES_FIELD_VALUE_NOCACHING</option>
                </field>
                <field
                    name="cache_time"
                    type="number"
                    label="COM_MODULES_FIELD_CACHE_TIME_LABEL"
                    default="0"
                />
                <field
                    name="cachemode"
                    type="hidden"
                    default="itemid"
                >
                    <option value="itemid"></option>
                </field>
            </fieldset>
        </fields>
    </config>
</extension>
```

### Aanpassen van tmpl/default.php

We kunnen de waarden van de parameter voor het maken van een link op de
website gebruiken. We krijgen toegang tot de waarde via de variabele
\$params.

```php
    $domain = $params->get('domain', 'https://www.joomla.org');
```

Later stellen we deze waarde in voor het maken van de hyperlink.

```php
<a href="<?php echo $domain; ?>">
	<?php echo '[PROJECT_NAME]' . $test; ?>
</a>
```

#### Het volledige tmpl/default.php bestand

Het volledige `tmpl/default.php` bestand ziet er als volgt uit:

```php
<?php
/**
 * @package    [PACKAGE_NAME]
 *
 * @author     [AUTHOR] <[AUTHOR_EMAIL]>
 * @copyright  [COPYRIGHT]
 * @license    GNU General Public License version 2 or later; see LICENSE.txt
 * @link       [AUTHOR_URL]
 */

// No direct access to this file
defined('_JEXEC') or die;

$domain = $params->get('domain', 'https://www.joomla.org');
?>

<a href="<?php echo $domain; ?>">
	<?php echo '[PROJECT_NAME]' . $test; ?>
</a>
```

### Aanpassen language/en-GB/en-GB.mod_foo.ini

Hier kunnen we de tekst voor de Engelse versie van het label van het
veld specificeren.

```ini
    MOD_FOO_FIELD_URL_LABEL="Url"
```

#### Het volledige language/en-GB/en-GB.mod_foo.ini bestand

Het volledige `language/en-GB/en-GB.mod_foo.ini` bestand ziet er als
volgt uit:

```ini
    MOD_FOO="[PROJECT_NAME]"
    MOD_FOO_XML_DESCRIPTION="Foo Module"
    MOD_FOO_FIELD_URL_LABEL="Url"
```

## Test uw module

Nu kunt u alle bestanden archiveren in een zip bestand en deze
installeren via de Joomla Extensie manager
Vervolgens kunt u uw module kiezen in de Module manager wanneer u een
nieuwe site module maakt.

In het beheergedeelte van uw site ziet u uw module zoals in de volgende
afbeelding is weergegeven.

Op het standaard tabblad, ziet u het custom field waar u de domeinnaam
kan invullen. De tekst voor het label wordt uit het taalbestand gehaald.

<img
src="https://docs.joomla.org/images/thumb/2/22/Moduletutorial31-en.png/700px-Moduletutorial31-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/2/22/Moduletutorial31-en.png/1050px-Moduletutorial31-en.png 1.5x, https://docs.joomla.org/images/2/22/Moduletutorial31-en.png 2x"
data-file-width="1287" data-file-height="627" width="700" height="341"
alt="Moduletutorial31-en.png" />

Op het tabblad Geavanceerd ziet u de standaard opties behalve de cache
mode want dit is een verborgen veld.

<img
src="https://docs.joomla.org/images/thumb/1/1e/Moduletutorial32-en.png/700px-Moduletutorial32-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/1/1e/Moduletutorial32-en.png/1050px-Moduletutorial32-en.png 1.5x, https://docs.joomla.org/images/1/1e/Moduletutorial32-en.png 2x"
data-file-width="1287" data-file-height="627" width="700" height="341"
alt="Moduletutorial32-en.png" />

Op de website ziet u uw module zoals weergegeven in de volgende
afbeelding. De tekst uit het helper bestand wordt weergegeven en u zou
een link moeten zien.

<img
src="https://docs.joomla.org/images/thumb/c/c2/Moduletutorial33-en.png/700px-Moduletutorial33-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/c/c2/Moduletutorial33-en.png/1050px-Moduletutorial33-en.png 1.5x, https://docs.joomla.org/images/c/c2/Moduletutorial33-en.png 2x"
data-file-width="1287" data-file-height="627" width="700" height="341"
alt="Moduletutorial33-en.png" />

## Conclusie

Form fields geven de gebruiker een makkelijk manier om de module aan te
passen aan de instellingen van hun site. Hierdoor kan de scope van de
modules worden vergroot.

Hier kunt u de voorbeeld bestanden vinden:

- <a
  href="https://github.com/joomla-extensions/boilerplate/tree/master/module"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla-extensions/boilerplate/tree/master/module</a>

De voorbeeld bestanden uit deze handleiding kunt u hier vinden:

- <a
  href="https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules</a>

Raadpleeg dit issue voor mogelijke toekomstige wijzigingen:

- <a href="https://github.com/joomla/joomla-cms/pull/23553"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla/joomla-cms/pull/23553</a>

## Gebruik Installeer, update en deinstalleer script - Deel 4

### Eisen

U hebt Joomla! 4.x nodig voor deze handleiding (op moment van schrijven
Joomla! 4.0.0-alpha6-dev)

### Scripts

Installeren, bijwerken en deïnstalleren van een module kunnen
aanvullende acties vereisen die niet bereikt kunnen worden door de basis
handelingen die worden beschreven in het hoofd xml bestand. Joomla biedt
een nieuwe aanpak om dit probleem op te lossen. Het bestaat in het
gebruik van een PHP script bestand dat een class bevat met vijf
methodes:

- preflight dat uitgevoerd wordt voor het installeren en de update
- install
- update
- uninstall
- postflight dat uitgevoerd wordt na installatie en update

Laten we eens kijken hoe Joomla 4 ze gebruikt. We gebruiken hier een
helper bestand voor.

### Bestandsstructuur

We zullen de volgende bestanden maken of aanpassen:

- `script.php` - Dit bestand gebruiken we hier als voorbeeld. Dit moeten
  we dus aanmaken.
- `language/en-GB/en-GB.mod_foo.sys.ini` - In dit bestand zullen we de
  tekst strings toevoegen.
- `mod_foo.xml` - We hebben een nieuw bestand aangemaakt (script.php)
  die tijdens de installatie geinstalleerd moet worden. Daarvoor moeten
  we dat bestand hier specificeren.

### script.php maken

Het schrijven van het script voor een extensie bestaat uit het
declareren van een classe met de naam `mod_ModuleNameInstallerScript`
met behulp van deze 5 methodes. Zie de commentaren in het bestand voor
meer informatie. In de commentaren wordt uitgelegd wanneer een methode
wordt aangeroepen.

In dit voorbeeld wordt alleen tekst gebruikt om aan te geven wanneer
welke methode wordt uitgevoerd. Daarnaast wordt aangegeven hoe de
minimale vereisten gecontroleerd kunnen worden. Natuurlijk kan er veel
meer gedaan worden. Bijvoorbeeld bestanden verwijderen die niet langer
gebruikt worden in de nieuwe versie van de module. Dit kan worden
teruggevonden in het bestand. Een andere mogelijkheid voor dit bestand
is het maken van voorbeeld content, om een succesbericht te laten zien
met het huidige versienummer of een redirect naar de pagina met de
module instellingen na een succesvolle installatie.

#### Het volledige script.php

Het volledige `script.php` bestand ziet er als volgt uit:

```php
<?php
/**
 * @package    [PACKAGE_NAME]
 *
 * @author     [AUTHOR] <[AUTHOR_EMAIL]>
 * @copyright  [COPYRIGHT]
 * @license    GNU General Public License version 2 or later; see LICENSE.txt
 * @link       [AUTHOR_URL]
 */

// No direct access to this file
defined('_JEXEC') or die;

use Joomla\CMS\Language\Text;
use Joomla\CMS\Log\Log;

/**
 * Script file of Foo module
 */
class mod_fooInstallerScript {

    /**
     * Extension script constructor.
     *
     * @return  void
     */
    public function __construct() {
        $this->minimumJoomla = '4.0';
        $this->minimumPhp = JOOMLA_MINIMUM_PHP;
    }

    /**
     * Method to install the extension
     *
     * @param   InstallerAdapter  $parent  The class calling this method
     *
     * @return  boolean  True on success
     */
    function install($parent) {
        echo Text::_('MOD_FOO_INSTALLERSCRIPT_INSTALL');

        return true;
    }

    /**
     * Method to uninstall the extension
     *
     * @param   InstallerAdapter  $parent  The class calling this method
     *
     * @return  boolean  True on success
     */
    function uninstall($parent) {
        echo Text::_('MOD_FOO_INSTALLERSCRIPT_UNINSTALL');

        return true;
    }

    /**
     * Method to update the extension
     *
     * @param   InstallerAdapter  $parent  The class calling this method
     *
     * @return  boolean  True on success
     */
    function update($parent) {
        echo Text::_('MOD_FOO_INSTALLERSCRIPT_UPDATE');

        return true;
    }

    /**
     * Function called before extension installation/update/removal procedure commences
     *
     * @param   string            $type    The type of change (install, update or discover_install, not uninstall)
     * @param   InstallerAdapter  $parent  The class calling this method
     *
     * @return  boolean  True on success
     */
    function preflight($type, $parent) {
        // Check for the minimum PHP version before continuing
        if (!empty($this->minimumPhp) && version_compare(PHP_VERSION, $this->minimumPhp, '<')) {
            Log::add(Text::sprintf('JLIB_INSTALLER_MINIMUM_PHP', $this->minimumPhp), Log::WARNING, 'jerror');

            return false;
        }

        // Check for the minimum Joomla version before continuing
        if (!empty($this->minimumJoomla) && version_compare(JVERSION, $this->minimumJoomla, '<')) {
            Log::add(Text::sprintf('JLIB_INSTALLER_MINIMUM_JOOMLA', $this->minimumJoomla), Log::WARNING, 'jerror');

            return false;
        }

        echo Text::_('MOD_FOO_INSTALLERSCRIPT_PREFLIGHT');
        echo $this->minimumJoomla . ' ' . $this->minimumPhp;

        return true;
    }

    /**
     * Function called after extension installation/update/removal procedure commences
     *
     * @param   string            $type    The type of change (install, update or discover_install, not uninstall)
     * @param   InstallerAdapter  $parent  The class calling this method
     *
     * @return  boolean  True on success
     */
    function postflight($type, $parent) {
        echo Text::_('MOD_FOO_INSTALLERSCRIPT_POSTFLIGHT');

        return true;
    }
}
```

### Aanpassen language/en-GB/en-GB.mod_foo.ini

Er valt hier niet veel uit te leggen. Plaats de tekst voor de vertaling
in dit bestand.

#### Het volledige language/en-GB/en-GB.mod_foo.ini bestand

Het volledige `language/en-GB/en-GB.mod_foo.ini` bestand ziet er als
volgt uit:

```ini
    MOD_FOO="[PROJECT_NAME]"
    MOD_FOO_XML_DESCRIPTION="Foo Module"
    MOD_FOO_INSTALLERSCRIPT_PREFLIGHT="Anything here happens before the installation/update/uninstallation of the module"
    MOD_FOO_INSTALLERSCRIPT_UPDATE="The module has been updated"
    MOD_FOO_INSTALLERSCRIPT_UNINSTALL="The module has been uninstalled"
    MOD_FOO_INSTALLERSCRIPT_INSTALL="The module has been installed"
    MOD_FOO_INSTALLERSCRIPT_POSTFLIGHT="Anything here happens after the installation/update/uninstallation of the module"
```

### Aanpassen mod_foo.xml

Er moet een regel worden toegevoegd, zodat het script automatisch wordt
aangeroepen in Joomla.

```xml
<scriptfile>script.php</scriptfile>
```

#### Het volledige mod_foo.xml bestand

Het volledige `mod_foo.xml` bestand ziet er als volgt uit:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="module" version="4.0" client="site" method="upgrade">
    <name>MOD_FOO</name>
    <creationDate>[DATE]</creationDate>
    <author>[AUTHOR]</author>
    <authorEmail>[AUTHOR_EMAIL]</authorEmail>
    <authorUrl>[AUTHOR_URL]</authorUrl>
    <copyright>[COPYRIGHT]</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <version>1.0</version>
    <description>MOD_FOO_XML_DESCRIPTION</description>
    <namespace>Joomla\Module\Foo</namespace>
    <scriptfile>script.php</scriptfile>
    <files>
        <filename module="mod_foo">mod_foo.php</filename>
        <folder>tmpl</folder>
        <folder>Helper</folder>
        <folder>language</folder>
        <filename>mod_foo.xml</filename>
    </files>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="domain"
                    type="url"
                    label="MOD_FOO_FIELD_URL_LABEL"
                    filter="url"
                />
            </fieldset>
            <fieldset name="advanced">
                <field
                    name="layout"
                    type="modulelayout"
                    label="JFIELD_ALT_LAYOUT_LABEL"
                    class="custom-select"
                />
                <field
                    name="moduleclass_sfx"
                    type="textarea"
                    label="COM_MODULES_FIELD_MODULECLASS_SFX_LABEL"
                    rows="3"
                />
                <field
                    name="cache"
                    type="list"
                    label="COM_MODULES_FIELD_CACHING_LABEL"
                    default="0"
                >
                    <option value="1">JGLOBAL_USE_GLOBAL</option>
                    <option value="0">COM_MODULES_FIELD_VALUE_NOCACHING</option>
                </field>
                <field
                    name="cache_time"
                    type="number"
                    label="COM_MODULES_FIELD_CACHE_TIME_LABEL"
                    default="0"
                />
                <field
                    name="cachemode"
                    type="hidden"
                    default="itemid"
                >
                    <option value="itemid"></option>
                </field>
            </fieldset>
        </fields>
    </config>
</extension>
```

## Test uw module

Nu kunt u alle bestanden archiveren in een zip bestand en deze
installeren via de Joomla Extensie manager. Direct na de installatie
ziet u de volgende informatie. Dit heeft u in het script ingesteld.

<img
src="https://docs.joomla.org/images/thumb/a/a7/Moduletutorial14-en.png/700px-Moduletutorial14-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/a/a7/Moduletutorial14-en.png/1050px-Moduletutorial14-en.png 1.5x, https://docs.joomla.org/images/a/a7/Moduletutorial14-en.png 2x"
data-file-width="1287" data-file-height="627" width="700" height="341"
alt="Moduletutorial14-en.png" />

## Conclusie

Zoals u ziet, biedt Joomla u veel om uw extensie meteen al gemakkelijk
in gebruik te maken.

Hier kunt u de voorbeeld bestanden vinden:

- <a
  href="https://github.com/joomla-extensions/boilerplate/tree/master/module"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla-extensions/boilerplate/tree/master/module</a>

De voorbeeld bestanden uit deze handleiding kunt u hier vinden:

- <a
  href="https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules</a>

## Gebruik de Joomla updaterː Toevoegen van Auto Update - Deel 5

### Eisen

U hebt Joomla! 4.x nodig voor deze handleiding (op moment van schrijven
Joomla! 4.0.0-alpha6-dev)

### Joomla Updater

Het eerste wat u kunt doen is het lezen van de [Het beheren van
component upgrades met Joomla 2.5 - Deel
1](https://docs.joomla.org/J2.5:Managing_Component_Updates "Special:MyLanguage/J2.5:Managing Component Updates")
handleiding om een idee te krijgen hoe het update proces in in Joomla
werkt. Hoewel geschreven voor 2.5, het proces is niet veel veranderd.
Lees ook [Een update-server
inzetten](https://docs.joomla.org/Deploying_an_Update_Server "Special:MyLanguage/Deploying an Update Server") -
dit is wat we in deze handleiding gaan doen.

### Bestandsstructuur

We zullen de volgende bestanden maken of aanpassen:

- `mod_foo.xml` - De enige manier om onze bestaande module te updaten is
  door een update server toe te voegen - bijvoorbeeld
  `http://www.example.com/foo_update.xml` - in het xml bestand.
- `foo_update.xml` - Vervolgens moeten we een xml bestand maken voor de
  updateserver op `http://www.example.com/foo_update.xml` om Joomla te
  laten weten dat er een update beschikbaar is.

#### Aanpassen mod_foo.xml

Om onze update server toe te voegen moeten we de volgende regels in ons
xml-bestand plaatsen:

```xml
<updateservers>
    <server type="extension" priority="1" name="[PROJECT_NAME]">https://www.example.com/mod_foo.xml</server>
</updateservers>
```

#### Het volledige mod_foo.xml bestand

Het volledige `mod_foo.xml` bestand ziet er als volgt uit:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="module" version="4.0" client="site" method="upgrade">
    <name>MOD_FOO</name>
    <creationDate>[DATE]</creationDate>
    <author>[AUTHOR]</author>
    <authorEmail>[AUTHOR_EMAIL]</authorEmail>
    <authorUrl>[AUTHOR_URL]</authorUrl>
    <copyright>[COPYRIGHT]</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <version>1.0</version>
    <description>MOD_FOO_XML_DESCRIPTION</description>
    <namespace>Joomla\Module\Foo</namespace>
    <files>
        <filename module="mod_foo">mod_foo.php</filename>
        <folder>tmpl</folder>
        <folder>Helper</folder>
        <folder>language</folder>
        <filename>mod_foo.xml</filename>
    </files>
    <updateservers>
        <server type="extension" priority="1" name="[PROJECT_NAME]">https://www.example.com/mod_foo.xml</server>
    </updateservers>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="domain"
                    type="url"
                    label="MOD_FOO_FIELD_URL_LABEL"
                    filter="url"
                />
            </fieldset>
            <fieldset name="advanced">
                <field
                    name="layout"
                    type="modulelayout"
                    label="JFIELD_ALT_LAYOUT_LABEL"
                    class="custom-select"
                />
                <field
                    name="moduleclass_sfx"
                    type="textarea"
                    label="COM_MODULES_FIELD_MODULECLASS_SFX_LABEL"
                    rows="3"
                />
                <field
                    name="cache"
                    type="list"
                    label="COM_MODULES_FIELD_CACHING_LABEL"
                    default="0"
                >
                    <option value="1">JGLOBAL_USE_GLOBAL</option>
                    <option value="0">COM_MODULES_FIELD_VALUE_NOCACHING</option>
                </field>
                <field
                    name="cache_time"
                    type="number"
                    label="COM_MODULES_FIELD_CACHE_TIME_LABEL"
                    default="0"
                />
                <field
                    name="cachemode"
                    type="hidden"
                    default="itemid"
                >
                    <option value="itemid"></option>
                </field>
            </fieldset>
        </fields>
    </config>
</extension>
```

Nu Joomla op zoek is naar updates van onze extensie gaan we een update
creëren om het proces te testen. Eerst moeten we het bestand
`foo_update.xml` maken. Tot nu toe hebben we alleen de update server
beschreven. We weten nog niet of er een update is. In het bestand
`foo_update.xml` geven we aan dat er een nieuwe versie is gepubliceerd
en waar deze gedownload kan worden.

### Aanmaken foo_update.xml

Nu moeten we een XML bestand maken op
`http://www.example.com/foo_update.xml` om Joomla te laten weten dat er
een update is.

#### Het volledige foo_update.xml bestand

Het volledige `foo_update.xml` bestand ziet er als volgt uit:

```xml
<?xml version="1.0" ?>
<updates>
    <update>
        <name>Foo</name>
        <description>This is mod_foo 1.0.1</description>
        <element>mod_foo</element>
        <type>module</type>
        <version>1.0.1</version>
        <downloads>
            <downloadurl type="full" format="zip">http://www.example.com/mod_foo_101.zip</downloadurl>
        </downloads>
        <maintainer>Joomla</maintainer>
        <maintainerurl>http://www.example.com</maintainerurl>
        <targetplatform name="joomla" version="4.0"/>
        <client>site</client>
    </update>
</updates>
```

Nadat het bestand is geüpload naar `https://www.example.com/mod_foo.xml`
zal de update weergegeven worden in het beheergedeelte van Joomla.

### Test de update van de module

Maak een kopie van de module zoals die nu is. Wijzig het versie nummer
naar 1.0.1. Zip alle bestanden. Upload nu het zip bestand naar
`http://www.example.com/mod_foo_101.zip`. Nu kan je de module updaten
via de Joomla Updater.

### Commerciële modules

In onze bestanden hebben we een koppeling gemaakt met xml bestand voor
de extensie updates. Vanuit dat bestand krijgen we de locatie van ze zip
voor de module. Dat betekend dat als iemand dit pad terug zou volgen, ze
de fysieke locatie van de broncode van ons module zip bestand kan
achterhalen. Als u dit niet wilt kunt in dit PR een oplossing vinden
<a href="https://github.com/joomla/joomla-cms/pull/15185"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla/joomla-cms/pull/15185</a>

### Conclusie

Met de Joomla Updater kunt u makkelijk alle gebruikers van uw extensie
bereiken en hen vertellen dat er een nieuwe versie is. Zelfs het updaten
is eenvoudig.

Iedere extensie zou een update server moeten gebruiken. Vooral in
verband met veiligheid.

Hier kunt u de voorbeeld bestanden vinden:

- <a
  href="https://github.com/joomla-extensions/boilerplate/tree/master/module"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla-extensions/boilerplate/tree/master/module</a>

De voorbeeld bestanden uit deze handleiding kunt u hier vinden:

- <a
  href="https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/astridx/boilerplate/tree/tutorial/tutorial/modules</a>
