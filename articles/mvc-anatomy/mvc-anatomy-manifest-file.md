<!-- Filename: J4.x:MVC_Anatomy:_Manifest_File / Display title: MVC Anatomie: Manifestbestand -->

## Metadata

Het component manifestbestand moet manifest.xml of componentname.xml heten, in dit geval countrybase.xml. Merk op dat het com_-gedeelte niet is inbegrepen. Het eerste deel van het bestand bevat metadata:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="component" method="upgrade">
    <name>com_countrybase</name>
    <author>Clifford E Ford</author>
    <authorEmail>john.doe@example.com</authorEmail>
    <authorUrl>example.com</authorUrl>
    <creationDate>2025-01-02</creationDate>
    <copyright>(C) 2025 Clifford E Ford</copyright>
    <license>GNU General Public License version 3 or later; see LICENSE.txt</license>
    <version>0.0.1</version>
    <description>COM_COUNTRYBASE_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Component\Countrybase</namespace>
```

De meeste metadata spreekt voor zich. Zaken om op te merken:

- Het type extensie in dit geval is **component**. Er zijn andere extensietypes, zoals module of plugin.
- De methode kan installeren of upgraden zijn. Laatstgenoemde laat herinstallatie toe na code-update.
- Het versienummer is belangrijk! Het moet het onmogelijk maken om een oudere versie opnieuw te installeren. En het wordt gebruikt om te controleren of database-updates nodig zijn.
- Het namespacepad src heeft drie delen:
  - Het eerste deel is een door de leverancier bepaald prefix, dus Joomla voor door Joomla geleverde code, Acme voor code geleverd door de derde partij extensieleverancier Acme, en in dit geval Cefjdemos, een naam gekozen voor Joomla code demonstraties.
  - Het tweede deel geeft het type extensie aan. Het kan Component of Module of Plugin of ... zijn.
  - Het derde deel is de specifieke extensienaam, in dit geval Countrybase.

## Database

Het manifestbestand definieert de locaties van installatie-, update- of verwijder-sql-bestanden. Ze komen terecht in een sql-map in de beheerdersmap.

```xml
    <install>
        <sql>
            <file driver="mysql" charset="utf8">sql/install.mysql.sql</file>
        </sql>
    </install>
    <uninstall>
        <sql>
            <file driver="mysql" charset="utf8">sql/uninstall.mysql.sql</file>
        </sql>
    </uninstall>
    <update>
        <schemas>
            <schemapath type="mysql">sql/updates/mysql</schemapath>
        </schemas>
    </update>
```

## Scriptbestand

Een scriptbestand kan worden gebruikt voor pre-flight of post-flight doeleinden. Het kan bijvoorbeeld worden gebruikt om de minimale systeemvereisten te controleren vóór de installatie of om parameters in te stellen na de installatie.

## Mediabestanden

De com_countrybase component heeft geen specifieke css of javascript nodig, maar de code om ze te installeren is inbegrepen voor het geval dat. De css en js mappen bevatten alleen lege index.html bestanden. Zonder deze bestanden worden de mappen niet aangemaakt.

```xml
    <scriptfile>script.php</scriptfile>

    <media destination="com_countrybase" folder="media">
        <file>joomla.asset.json</file>
        <folder>css</folder>
        <folder>js</folder>
    </media>
```

Merk op dat het joomla.asset.json-bestand wordt gebruikt door de webassetmanager, meestal aangeroepen vanuit een templatebestand.

## Sitebestanden

Dit zijn de bestanden die naar root/components/com_countrybase worden gekopieerd. Merk op dat de taalmap naar de componentmap wordt gekopieerd en niet naar de root/language map:

```xml
    <files folder="site">
        <folder>language</folder>
        <folder>forms</folder>
        <folder>src</folder>
        <folder>tmpl</folder>
    </files>
```

## Beheerdersbestanden

Er zijn meer bestanden in het administratieve deel van het manifestbestand omdat het meer te doen heeft:

```xml
    <administration>
        <files folder="admin">
            <filename>access.xml</filename>
            <filename>config.xml</filename>
            <folder>forms</folder>
            <folder>help</folder>
            <folder>language</folder>
            <folder>layouts</folder>
            <folder>services</folder>
            <folder>sql</folder>
            <folder>src</folder>
            <folder>tmpl</folder>
        </files>
        <menu img="class:default">countrybase</menu>
        <submenu>
            <!--
                Note that all & must be escaped to &amp; for the file to be valid
                XML and be parsed by the installer
            -->
            <menu
                link="option=com_countrybase&amp;view=countries"
                img="default"
            >
                COM_COUNTRYBASE_COUNTRIES
            </menu>
            <menu
                link="option=com_countrybase&amp;view=currencies"
                img="default"
            >
                COM_COUNTRYBASE_CURRENCIES
            </menu>
        </submenu>
    </administration>
```

Merk de methode op om de Joomla Administrator-menu-items te maken. Er is een menu-item dat geen link heeft. En twee menu-items die linken naar de twee beschikbare beheerweergaven: de landenlijst en de valutalijst. Ook moeten de string sleutelvertalingen in het com_countrybase.sys.ini bestand staan.

## Wijzigingslogboek

De changelog wordt gebruikt om een overzicht van wijzigingen bij te houden voor elke versie van de extensie. Zie het [Changelogs](jdocmanual?article=docus/install-update/installation-change-log) artikel voor informatie over de inhoud van de changelog.

```xml
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-com-countrybase/master/changelog.xml</changelogurl>
```

## Update Server

De update-servercode vertelt Joomla waar hij informatie over updates kan vinden. Deze wordt op regelmatige intervallen uitgevoerd om te zien of er een update beschikbaar is. Laat dit gedeelte weg als u geen updateserver voor uw eigen component gebruikt.

```xml
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" name="Countrybase Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-com-countrybase/master/updates.xml</server>
    </updateservers>
```

*Vertaald door openai.com*

