<!-- Filename: Manifest_files / Display title: Manifest bestanden -->

Joomla!  4.x Joomla!  3.x Joomla!  2.5

Binnen Joomla zijn manifest bestanden voor alle extensies. Deze
bestanden bevatten zowel de algemene installatie informatie als
parameters voor de configuratie van de
[extensie](https://docs.joomla.org/extension "Special:MyLanguage/extension")
zelf. Sinds Joomla! 2.5, zijn weinig verschillen tussen de manifest
bestand formaten voor de verschillende [types
extensies](https://docs.joomla.org/Extension_types_(technical_definitions) "Special:MyLanguage/Extension types (technical definitions)"),
waardoor elk type toegang krijgt tot de volledige kracht van de Joomla!
installer.

## Naamgeving

Het bestand moet heten `manifest.xml` of `.xml` en staan in de root-map
van het installatie pakket.

Joomla 4.x: Automatic namespace mapping will fail if a manifest file
named `manifest.xml` is used. See:
<a href="https://github.com/joomla/joomla-cms/issues/37750"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla/joomla-cms/issues/37750</a>

## Syntaxis

### Root element

De primaire tag van het installatie bestand is:

Dit begin en sluit tag is nu geldig voor alle extensies. De nieuwe tag
vervangt de oude uit Joomla
<img src="https://docs.joomla.org/images/c/c8/Compat_icon_1_5.png"
decoding="async" data-file-width="40" data-file-height="17" width="40"
height="17" alt="Joomla 1.5" />. De volgende attributen zijn toegestaan
binnen de tag:

<table class="wikitable">

<tbody>
<tr class="header">
<th>Attribuut</th>
<th>Waarden</th>
<th>Van toepassing op</th>
<th>Beschrijving</th>
</tr>
&#10;<tr class="odd">
<td>type</td>
<td><code>component</code><br />
<code>file</code><br />
<code>language</code><br />
<code>library</code><br />
<code>module</code><br />
<code>package</code><br />
<code>plugin</code><br />
<code>template</code><br />
<code>element</code></td>
<td>Alle extensies</td>
<td>Dit attribuut beschrijft het type extensie voor de installer.
Gebaseerd op dit type zijn er verdere eisen aan de sub-tags van
toepassing.</td>
</tr>
<tr class="even">
<td>version</td>
<td><code>2.5</code><br />
<code>3.0</code></td>
<td>Alle extensies</td>
<td><p>String die de versie van Joomla aangeeft waarvoor deze extensie
ontwikkeld is.</p></td>
</tr>
<tr class="odd">
<td>method</td>
<td><code>install</code><br />
<code>upgrade</code></td>
<td>Alle extensies</td>
<td>De standaard waarde <code>install</code> zal ook gebruikt worden als
het method attribuut niet gebruikt wordt. De <code>install</code> waarde
betekent dat de installer netjes zal stoppen als het een bestaand
bestand/map vindt van de nieuwe extensie.</td>
</tr>
<tr class="even">
<td>client</td>
<td><code>site</code><br />
<code>administrator</code></td>
<td>Modules</td>
<td>Het client attribuut geeft u de mogelijkheid aan te geven voor welke
client toepassing (website of beheergedeelte) de nieuwe module
beschikbaar is.</td>
</tr>
<tr class="odd">
<td>group</td>
<td><em>string</em></td>
<td>Plugins</td>
<td>De group naam geeft aan voor welke groep plugins de nieuwe plugin
beschikbaar is. De bestaande groepen zijn de mapnamen binnen de map
<code>/plugins</code>. De installer zal nieuwe mappen aanmaken voor
groepsnamen die nog niet bestaan.</td>
</tr>
</tbody>
</table>

### Metadata

De volgende elementen kunnen gebruikt worden om metadata in te voegen.
Geen van deze elementen zijn verplicht; indien ze aanwezig zijn, moeten
ze afstammen van het root element.

     – raw component name (e.g. com_banners).
     – author's name (e.g. Joomla! Project)
     – date of creation or release (e.g. April 2006)
     – a copyright statement (e.g. (C) 2005 - 2011 Open Source Matters. All rights reserved.)
     – a license statement (e.g. NU General Public License version 2 or later; see LICENSE.txt)
     – author's email address (e.g. admin@joomla.org)
     – URL to the author's website (e.g. www.joomla.org)
     – the version number of the extension (e.g. 1.6.0)
     – the description of the component. This is a translatable field. (e.g. COM_BANNERS_XML_DESCRIPTION)
     – the internal name of the component. If omitted, name will be cleaned and used

Opmerking: De en tags zijn ook te vertalen velden zodat de naam en
beschrijving van de extensie aan de gebruiker getoond kan worden in hun
eigen taal.

### Website bestanden

```xml
	<files folder="from-folder">
		<filename>example.php</filename>
		<folder>examples</folder>
	</files>
```

Bestanden die gekopieerd moeten worden naar de website map moeten
geplaatst worden binnen het element. U kunt het optionele `folder`
attribuut gebruiken om een map **in het ZIP pakket** op te geven om
**vanaf** te kopiëren. Ieder bestand dat gekopieerd moet worden moet
vertegenwoordigd zijn door het element. Indien u een volledige map in
één keer wilt kopiëren, dan kunt u het als definiëren.

Voor **plugins**, moet de naam van de plugin geplaatst worden in het
`plugin` attribuut in het element dt wijst naar het bestand dat de class
van de plugin bevat. Gebruik bijvoorbeeld, in geval van een systeem
plugin genaamd "voorbeeld" (volledige naam `plg_system_voorbeeld`),
`voorbeeld.php`.

### Mediabestanden

```xml
	<media folder="media" destination="com_example">
		<filename>com_example_logo.png</filename>
		<folder>css</folder>
		<folder>js</folder>
	</media>
```

Dit voorbeeld kopieert de bestand(en) (`/media/com_example_logo.png`) en
de opgenomen mappen ( `/media/css/` en `/media/js/` ) naar
`/media/com_example/`, waarbij de `com_example` map, indien
noodzakelijk, aangemaakt wordt. U kunt het optionele `folder` attribuut
gebruiken om een map op te geven **in het ZIP pakket** om **vanaf** te
kopiëren (in dit geval, `media`).

Extensies zouden middelen die ze nodig hebben om web-toegankelijk te
zijn (JS, CSS, images etc) moeten opslaan in `media`. Deze functie was
onder andere toegevoegd als stap in de ontwikkeling naar multi-site
ondersteuning en de eventuele stap van code bestanden (PHP) buiten de
vanuit het web toegankelijke gebieden van de server
Merk op: de media sectie wordt niet geparsed voor 'package' type
extensions.

Ref:

- <a
  href="https://groups.google.com/forum/#!msg/joomla-dev-cms/4CAASJqFY-k/PvPj14gP29EJ"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Google Groups - joomla-dev-cms
  thread</a>
- <a
  href="https://groups.google.com/forum/#!msg/joomla-dev-cms/uNmhX98sKbE/p8p68Jke680J"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Google Groups - joomla-dev-cms
  thread</a>

### Administratie sectie

```xml
	<administration>
		<!-- various elements -->
	</administration>
```

De administratie sectie wordt gedefinieerd in het element. Aangezien
alleen
[componenten](https://docs.joomla.org/Component "Special:MyLanguage/Component")
van toepassing zijn op zowel de
[administrator](https://docs.joomla.org/Site_(Application) "Special:MyLanguagehttps://docs.joomla.org/Administrator_(Application)"),
**kunnen alleen component manifest-bestanden dit element bevatten**.

#### Back-end-bestanden

Bestanden die gekopieerd moeten worden naar de back-end map moeten
geplaatst worden binnen het element onder . U kunt het optionele
`folder` attribuut gebruiken om een map op te geven **in het ZIP
pakket** om **vanuit** te kopiëren. Zie *Front-end bestanden* voor meer
regels.

#### Menu-links en sub-menu's

**Versie opmerking:** Voor Joomla 3.4, leidde het niet hebben van een

tag in uw manifest.XML bestand er toch toe dat een menu-item werd
aangemaakt. Deze bug is gerepareerd in Joomla 3.4, dus, als er geen

tag in uw manifest file zit, dan wordt er geen beheer menu-item
aangemaakt voor de component.

```xml
	<menu>COM_EXAMPLE</menu>
	<submenu>
		<!--
			Note that all & must be escaped to &amp; for the file to be valid
			XML and be parsed by the installer
		-->
		<menu link="anoption=avalue&amp;anoption1=avalue1">COM_EXAMPLE_SUBMENU_ANOPTION</menu>
		<menu view="viewname">COM_EXAMPLE_SUBMENU_VIEWNAME</menu>
	</submenu>
```

De tekst voor het hoofd menu-item voor de component is gedefinieerd in
het `<menu>` item, een onderdeel van `<administration>`. Een `<submenu>`
element kan ook aanwezig zijn (ook onderdeel van ), welke meer menu-items
kan bevatten, gedefinieerd door `<menu>`.

Bovendien kan elk `<menu>` item de volgende attributen definiëren:

<table class="wikitable">

<tbody>
<tr class="header">
<th>Attribuut</th>
<th>Beschrijving</th>
</tr>
&#10;<tr class="odd">
<td>link</td>
<td>Een link waarnaar de gebruiker gestuurd wordt als op het menu-item
geklikt wordt. U kunt in plaats hiervan "view" gebruiken.</td>
</tr>
<tr class="even">
<td>img</td>
<td>Het (relatieve) pad naar een afbeelding (16x16 pixels) om naast het
menu-item te verschijnen.
<p><u>Moet URL compatibel zijn als bestandsverwijzing (bijvoorbeeld geen
spaties) !</u></p></td>
</tr>
<tr class="odd">
<td>alt</td>
<td></td>
</tr>
<tr class="even">
<td>view</td>
<td>Een URL parameter toe te voegen aan de link. Bijvoorbeeld,
COM_EXAMPLE
in com_example's XML manifest zou zorgen dat de URL van het menu-item
zou zijn <code>index.php?option=com_example&amp;view=cpanel</code>. U
kunt in plaats hiervan "link" gebruiken.</td>
</tr>
</tbody>
</table>

De waarde binnen de tag is het label van het menu. In tegenstelling tot
Joomla! 1.5, kunt u geen natuurlijke taalstring gebruiken. Als u
bijvoorbeeld "Voorbeeld component" zou opgeven in plaats van
COM_EXAMPLE, dan zou het resultaat zijn dat de naam van uw component zou
verschijnen als voorbeeld-component in het menu en u zou geen vertaling
kunnen maken. Om een vertaling te kunnen leveren moet u een bestand
genaamd en-GB.com_example.sys.ini maken in administrator/languages/en-GB
(u kunt de tag van het manifest bestand tijdens de installatie kopiëren)
of in administrator/components/com_example/language/en-GB. In het
laatste geval, moet u het taalbestand niet opnemen in de tag. Als u de
taal-map in uw tag heeft opgenomen, zal het worden gekopieerd als de
component wordt geïnstalleerd.

De inhoud van dat bestand moet zijn:

```ini
    COM_EXAMPLE="Example Component"
    COM_EXAMPLE_SUBMENU_ANOPTION="Another Option"
    COM_EXAMPLE_SUBMENU_VIEWNAME="Another View"
```

Please note that the language string must be enclosed in double quotes,
as per Joomla!'s translation standards.

Joomla! 1.6 and later sorts the Component menu items based on the actual
translation of the key you supply in your XML manifest. This means that
the sorting order is correct no matter what you call your translation
key and no matter which language the site is being displayed in.
Essentially, Joomla! 1.6 fixed the wrong sorting of the Components menu
experienced under Joomla! 1.5 for the majority (non-English speaking!)
of Joomla! users.

### Dashboards

Specifies the details for displaying a dashboard for the component in
the Administrator area for the site.

- It will make a dashboard icon appear next to the administrator menu
  item for the component
- The dashboard icon will click through to display modules assigned to
  the cpanel-example administrator module position
- The title and icon defined in the XML file will be used as the header
  and icon at the top of the component's dashboard page.

```xml
  <dashboards>
		<dashboard title="COM_EXAMPLE_DASHBOARD_TITLE" icon="icon-lock">example</dashboard>
	</dashboards>
```

### Configuratie

<img
src="https://docs.joomla.org/images/thumb/4/41/Stop_hand_nuvola.svg.png/25px-Stop_hand_nuvola.svg.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/4/41/Stop_hand_nuvola.svg.png/38px-Stop_hand_nuvola.svg.png 1.5x, https://docs.joomla.org/images/4/41/Stop_hand_nuvola.svg.png 2x"
data-file-width="40" data-file-height="40" width="25" height="25"
alt="Stop hand nuvola.svg.png" />Warning!

Het element, onder de root, beschrijft de configuratie opties voor de
extensie. Indien van toepassing, worden de opties getoond door de juiste
beheerder (Pluginbeheer, Modulebeheer of Templatebeheer). **Configuratie
opties voor componenten worden gedefinieerd in een apart bestand genaamd
`config.xml`. Het root element moet zijn , plugins en modules gebruiken
de sectie in het extensie manifest bestand.**

Elke fieldset moet één of meer elementen bevatten, die elk één enkele
[form
field](https://docs.joomla.org/form_field "Special:MyLanguage/form field")
bevatten met een label. Zie [Standaard form field
types](https://docs.joomla.org/Standard_form_field_types "Special:MyLanguage/Standard form field types")
voor een lijst met toegestane form field types en voorbeeld XML form
field definities.

### Namespace

Specify the namespace to be used for autoloading the plugin. The given
namespace will autoload to the root directory of your extension by
default however you can optionally add a "path" attribute to the
namespace element to specify a subpath within your extensions root
directory.

### SQL

```sql
    <install>
        <sql>
            <file driver="mysql" charset="utf8">sql/example.install.sql</file>
        </sql>
    </install>
    <uninstall>
        <sql>
            <file driver="mysql" charset="utf8">sql/example.uninstall.sql</file>
        </sql>
    </uninstall>
```

In bovenstaande voorbeeld, stoppen we de SQL bestanden in de `admin/sql`
map van het installatie pakket. U moet de `sql` map opnemen in de
administration bestanden (zoals beschreven in *Back-end bestanden*).

U kunt SQL tijdens de installatie en/of deïnstallatie uitvoeren met
behulp van respectievelijk de en elementen. Een element moet verschijnen
onder deze elementen. kan ieder aantal elementen bevatten, welk elk één
enkel SQL bestand bevatten om uit te voeren. Hun database driver types
worden beschreven door het `driver` attribuut, hun character sets via
het `charset` attribuut.

#### Het SQL schema bijwerken

Sinds 1.6, is er ook een tag, die u de mogelijkheid geeft een reeks SQL
bestanden te leveren om het huidige schema bij te werken.

```sql
	<update>
		<schemas>
			<schemapath type="mysql">sql/updates/mysql</schemapath>
			<schemapath type="sqlsrv">sql/updates/sqlsrv</schemapath>
		</schemas>
	</update>
```

Bijvoorbeeld om van versie `1.0.0` naar versie `1.0.1` te gaan in een
**MySQL** database, een `1.0.1.sql -` bestand moet in de
`sql/updates/mysql` map worden gemaakt en de tag van het manifest moet
worden bijgewerkt naar

```sql
<version>1.0.1</version>
```

De definitieve structuur van de sql-map zal er als volgt uit zien
(uitgaande van een **MySQL** database)

    sql
     |-->example.install.sql
     |-->example.uninstall.sql
     |-->updates
         |-->mysql
            |-->1.0.1.sql

Soortgelijke bestanden moet worden gemaakt voor latere versies.

### Taalbestanden

In Joomla! 1.5, moesten extensie ontwikkelaars extensie taalbestanden in
het Joomla! hoofd taalbestand opnemen via de ...; tag zoals hieronder
getoond. **Deze methode kan nog steeds gebruikt worden in Joomla!**
<img src="https://docs.joomla.org/images/4/4d/Compat_icon_3_x.png"
decoding="async" data-file-width="40" data-file-height="17" width="40"
height="17" alt="Joomla 3.x" />.

```xml
<!-- Joomla! language tag -->
<languages folder="langfiles">
	<language tag="en-GB">en-GB.com_example.ini</language>
</languages>
```

Sinds Joomla! 1.6 wordt aanbevolen uw extensie taalbestanden in uw
extensie mappen te plaatsen. Joomla! zal dan automatisch uw extensie
taalbestanden laden.

Door het opslaan van extensie taalbestanden in de extensie map, heeft u
voordeel door het isoleren en beschermen van uw extensie taalbestanden.
Een beheerder verwijdert bijvoorbeeld een taal uit uit de Joomla!
installatie. Uw extensie taalbestanden worden niet verwijderd. Ze
blijven op hun plaats en zijn beschikbaar als de taal opnieuw
geïnstalleerd wordt.

De structuur van de taalmap voor website en beheergedeelte is hetzelfde.
U plaats ze in de taal tag (bijvoorbeeld **en-GB** ) van iedere taal in
uw taalmap, bijvoorbeeld **language/en-GB/**. U moet deze mappen in de
website- en beheer-bestanden ook opnemen.

Neem, in uw manifest bestand, eenvoudig de '**language'** map in uw
files sectie op. De sub-mappen voor iedere taal worden dan automatisch
gekopieerd. Voeg, binnen de groep, eenvoudig een element toe naast de
items in de groep zoals in dit voorbeeld getoond wordt:

```xml
<files>
	<filename plugin="alpha">alpha.php</filename>
	<folder>sql</folder>
	<folder>language</folder>
</files>
```

Merk op dat beide manieren kunnen samenwerken. Hier is een voorbeeld uit
de core:

```xml
<files>
	<filename plugin="languagecode">languagecode.php</filename>
	<filename>index.html</filename>
	<folder>language</folder>
</files>
<languages>
	<language tag="en-GB">language/en-GB/en-GB.plg_system_languagecode.ini</language>
	<language tag="en-GB">language/en-GB/en-GB.plg_system_languagecode.sys.ini</language>
</languages>
```

Deze oplossing heeft de volgende voordelen:

Alle *.ini* bestanden in de core map hebben voorrang op de bestanden in
de extensie *language/* map. Een *.sys.ini* bestand zal bijvoorbeeld
altijd geladen worden vanuit core mappen in het beheergedeelte als ze
bestaan, behalve als een extensie geïnstalleerd wordt die een *.sys.ini*
bestand bevat in een taalmap. In dat geval en alleen maar in dat geval,
zal het *.sys.ini* bestand in de extensie map zijn vertaalde inhoud
tonen bij het installeren. Dit is erg handig. Aangezien een ontwikkelaar
twee *sys.ini* bestanden kan hebben met verschillende inhoud.

Daarnaast is het voor een gebruiker veel makkelijker om een *.ini*
bestand in de hoofdmappen toe te voegen voor een extensie die dat niet
in de gewenste taal verstrekt. Er is geen risico dat het verwijderd
wordt als dat de extensie per ongeluk of om een andere reden
gedeïnstalleerd wordt.

Zie ook:

- [Het maken van niet-core
  taalpakketten](https://docs.joomla.org/J2.5:Making_non-core_language_packs "Special:MyLanguage/J2.5:Making non-core language packs")
- [Het maken van taalpakketten voor extensies in Joomla
  2.5](https://docs.joomla.org/Creating_language_packs_for_extensions_in_Joomla_2.5 "Special:MyLanguage/Creating language packs for extensions in Joomla 2.5")

Tijdens het ontwikkelen kunt u foutopsporing in de Joomla! Algemene
instellingen aan zetten. U kunt onderzoeken of er en probleem optreed.
Vanaf 3.2, is dit noodzakelijk om te debuggen aangezien en-GB **altijd**
eerst wordt geladen als u niet in foutopsporingsmodus bent om te
voorkomen dat constanten getoond worden.

### Script bestand

```xml
    <scriptfile>example.script.php</scriptfile>
```

Een optioneel **scriptbestand** (PHP code die voor, tijdens en/of na
installatie, deïnstallatie en upgrade uitgevoerd wordt) kan gedefinieerd
worden door middel van een element.

Dit bestand moet een class genaamd
"InstallerScript" bevatten waarbij de naam van uw extensie is
(bijvoorbeeld com_componentnaam, mod_modulenaam, etc.). Plugins vereisen
dat de groep wordt opgegeven (bijvoorbeeld plgsystempluginnaam).

In <img src="https://docs.joomla.org/images/f/f4/Compat_icon_4_0.png"
decoding="async" data-file-width="40" data-file-height="17" width="40"
height="17" alt="Joomla 4.0" /> and later the structure of the class is
as follows:

```php
<?php

use Joomla\CMS\Installer\InstallerAdapter;

class com_componentnameInstallerScript
{
	/**
	 * Constructor
	 *
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 */
	public function __construct(InstallerAdapter $adapter)
	{
	}

	/**
	 * Called before any type of action
	 *
	 * @param   string  $route  Which action is happening (install|uninstall|discover_install|update)
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 *
	 * @return  boolean  True on success
	 */
	public function preflight($route, InstallerAdapter $adapter)
	{
		return true;
	}

	/**
	 * Called after any type of action
	 *
	 * @param   string  $route  Which action is happening (install|uninstall|discover_install|update)
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 *
	 * @return  boolean  True on success
	 */
	public function postflight($route, $adapter)
	{
		return true;
	}

	/**
	 * Called on installation
	 *
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 *
	 * @return  boolean  True on success
	 */
	public function install(InstallerAdapter $adapter)
	{
		return true;
	}

	/**
	 * Called on update
	 *
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 *
	 * @return  boolean  True on success
	 */
	public function update(InstallerAdapter $adapter)
	{
		return true;
	}

	/**
	 * Called on uninstallation
	 *
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 */
	public function uninstall(InstallerAdapter $adapter)
	{
		return true;
	}
}

?>
```

Note that since Joomla 3.6 Joomla has shipped a basic script that you
can use instead of shipping your own from scratch **JInstallerScript**
which contains various helper methods commonly used through the
community.

### Library Manifests

A simple library manifest might look like this:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="library" method="upgrade" version="4.0">
    <name>My Test library.</name>
    <libraryname>mytest</libraryname>
    <files>
        <folder>Classes</folder>
        <folder>language</folder>
        <filename>mytest.php</filename>
    </files>
</extension>
```

This will install the library into the `JPATH_SITE/libraries/mytest`
folder.

What if your company has several libraries and you want to group them
together under folder `JPATH_SITE/libraries/mycompany`?

Simple - include your company name in the `libraryname` property of each
library like this:

```xml
    <libraryname>mycompany/mylibrary1</libraryname>
```

```xml
    <libraryname>mycompany/mylibrary2</libraryname>
```

These libraries will then be installed in the
`JPATH_SITE/libraries/mycompany/mylibrary1` and
`JPATH_SITE/libraries/mycompany/mylibrary2` folders.

Uninstalling `mylibrary1` will still leave `mylibrary2` installed on
your site.

When using `script files` with such company libraries the installer
class name should look like this:

```php
    class mycompanymylibrary1InstallerScript
```

```php
    class mycompanymylibrary2InstallerScript
```

### Update-servers

```xml
    <updateservers>
        <server type="extension" priority="1" name="Extension Update Site">http://example.com/extension.xml</server>
        <server type="collection" priority="2" name="Collection Update Site">http://example.com/collection.xml</server>
    </updateservers>
```

Update-servers kunnen worden gedefinieerd in het element, onder de root.
Dit element kan een of meer element bevatten, welke ieder de locatie
beschrijft om updates vanaf te downloaden. Elk item kan de volgende
attributen bevatten:

<table class="wikitable">

<tbody>
<tr class="header">
<th>Attribuut</th>
<th>Waarden</th>
<th>Beschrijving</th>
</tr>
&#10;<tr class="odd">
<td>type</td>
<td><code>extension</code><br />
<code>collection</code></td>
<td>Het update-server type</td>
</tr>
<tr class="even">
<td>priority</td>
<td><em>integer</em></td>
<td>De prioriteit van de update-server</td>
</tr>
<tr class="odd">
<td>name</td>
<td><em>string</em></td>
<td>De naam van de update-server</td>
</tr>
</tbody>
</table>

Meer informatie:

- [Het bouwen van een Joomla! extensie - Een update-server
  toevoegen](https://docs.joomla.org/J3.x:Developing_an_MVC_Component/Adding_an_update_server "Special:MyLanguage/J3.x:Developing an MVC Component/Adding an update server")
- [Component updates beheren in Joomla
  2.5](https://docs.joomla.org/J2.5:Managing_Component_Updates "Special:MyLanguage/J2.5:Managing Component Updates")

### Ondersteuning download sleutels

Vanaf Joomla 4.0.0 kunnen gebruikers hun download sleutels in de
updatesites lijst invoeren. Dit biedt hen een enkele plaats om de
download sleutels te beheren. Als een gebruiker een extensie gaat
updaten zal Joomla controleren of er een download sleutel is, als er een
download sleutel is zal Joomla de download sleutel aan de update URL
toevoegen.

Om download sleutels te ondersteunen moet u de `dlid` tag in het
manifest bestand opnemen. De `dlid` tag heeft 2 argumenten:

- prefix
- suffix

De `dlid` tag ziet er in uw manifest bestand zo uit:

```xml
<dlid prefix="dlid=" suffix="&amp;dummy=my.zip"/>
```

De prefix zal voor de download sleutel toegevoegd worden en de suffix na
de download sleutel. Met het voorbeeld hierboven zal de volledige query
die aan de download-link wordt toegevoegd worden:

    dlid=KEY&dummy=my.zip

De sleutel wordt toegevoegd voor het `onInstallerBeforePackageDownload`
event wordt geactiveerd, de volledige URL zal zo aan het event worden
doorgegeven.

## Voorbeelden

Zie, voor een echt voorbeeld <a
href="https://github.com/joomla/joomla-cms/3.6.4/administrator/components/com_banners/banners.xml"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">het manifest-bestand van het
Banner-component in de nieuwste versie van Joomla! 3.6.4</a>.

Meer voorbeelden kunnen worden gevonden in de stand-alone weblinks repo:

- <a
  href="https://github.com/joomla-extensions/weblinks/blob/master/src/administrator/components/com_weblinks/weblinks.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">com_weblinks manifest</a>
- <a
  href="https://github.com/joomla-extensions/weblinks/blob/master/src/modules/mod_weblinks/mod_weblinks.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">mod_weblinks manifest</a>
- <a
  href="https://github.com/joomla-extensions/weblinks/blob/master/src/plugins/search/weblinks/weblinks.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">plg_search_weblinks manifest</a>
- <a
  href="https://github.com/joomla/joomla-cms/blob/3.6.4/templates/protostar/templateDetails.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">tpl_protostar manifest (3.6.4)</a>
- <a
  href="https://github.com/joomla/joomla-cms/blob/3.6.4/administrator/language/en-GB/en-GB.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">en-GB manifest (3.6.4)</a>
