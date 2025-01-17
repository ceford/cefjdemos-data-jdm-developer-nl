<!-- Filename: Creating_a_Smart_Search_plug-in / Display title: Voorbeeld: Slimme Zoekopdracht -->

## Introductie

De Smart Search-plug-ins bevinden zich in de `finder` groep (in plugins/finder/xxxx). De codering van finder-plug-ins is aanzienlijk veranderd in Joomla 4 en 5. Om een nieuwe finder-plug-in te maken, is het waarschijnlijk het beste om een bestaande core-plug-in te kopiëren en de inhoud ervan aan te passen aan het nieuwe doel.

Dit artikel beschrijft een zoeker-plugin die ontworpen is ter ondersteuning van de *Jdocmanual*-extensie. De inhoud die geïndexeerd moet worden bevindt zich in de tabel `#__jdm_articles`. De code is te vinden op [GitHub](https://github.com/ceford/cefjdemos-plg-finder-jdocmanual).

## IDE-installatie - VSCode of VSCodium

In mijn lokale ontwikkelomgeving bevindt de broncode van deze extensie zich in ~/git/cefjdemos-plg-jdm-finder. Deze mapnaam heeft geen betekenis; het is gewoon mijn manier om mijn eigen extensies op orde te houden. De schematische structuur van de map is als volgt:

```
cefjdemos-plg-finder-jdocmanual
|-- .vscode (folder for build settings)
|-- plg_finder_jdocmanual (folder compressed to create an installable zip file)
    |-- language/en-GB/ (language folder, kept with the extension code)
        |-- plg_finder_jdocmanual.ini
        |-- plg_finder_jdocmanual.sys.ini
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Extension (folder for extension specific code)
            |-- Jdocmanual.php (the extension class code)
    |-- jdocmanual.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
```

In VSCodium ziet het er zo uit:

![Plugin development file structure in vscodium](../../../en/images/plugins/jdocmanual-vscodium.png)

## Pas de code aan

Beginnend met een basiszoekerplugin is de eerste taak om alle verwijzingen naar de oorspronkelijke plugin te veranderen in geschikte waarden voor de nieuwe plugin. In dit geval zijn er 63 gevallen van het woord `jdocmanual` in 7 bestanden. Er is een goede kans dat sommige zullen worden gemist bij de eerste controle en de plugin zal niet werken.

## De extensie bouwen

Er zijn twee delen aan de build. Ik heb een build.xml-bestand dat instructies bevat voor [`phing`](https://www.phing.info/), een buildtool voor PHP-projecten. Het kan worden aangeroepen vanuit VSCode/VSCodium of Eclipse of een andere IDE.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="jdocmanual" basedir="." default="main">

    <property name="joomladir" value="/Users/ceford/public_html/jdm3"  override="true" />
    <property name="sitecomdir" value="${joomladir}/components" override="true" />
    <property name="admincomdir" value="${joomladir}/administrator/components" override="true" />
    <property name="adminmediadir" value="${joomladir}/media/com_jdocmanual" override="true" />
    <property name="plgdir" value="${joomladir}/plugins/finder/jdocmanual" override="true" />
    <property name="srcdir" value="${project.basedir}" override="true" />

    <property name="zipsdir" value="/Users/ceford/git/zips/cefjdemos"  override="true" />

    <!-- Fileset for plugin files -->
    <fileset dir="./plg_finder_jdocmanual" id="plgfiles">
        <include name="**" />
    </fileset>

    <!-- fileset for zip -->
    <fileset dir="./plg_finder_jdocmanual" id="plgfiles">
        <include name="**" />
    </fileset>

    <!-- ============================================  -->
    <!-- (DEFAULT) Target: main                        -->
    <!-- ============================================  -->
    <target name="main" description="main target">

        <copy todir="${plgdir}">
            <fileset refid="plgfiles" />
        </copy>

        <zip destfile="${zipsdir}/plg_finder_jdocmanual.zip">
            <fileset refid="plgfiles" />
        </zip>

    </target>
</project>
```

Het tweede deel is een `tasks.json`-bestand in de .vscode-map. Het vertelt VSCode waar het phing-bestand te vinden is.

```json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
      {
        "label": "Build plg_finder_jdocmanual",
        "type": "shell",
        "command": "php ~/bin/phing-latest.phar",
        "windows": {
          "command": "php ~/bin/phing-latest.phar"
        },
        "group": "build",
        "presentation": {
          "reveal": "always",
          "panel": "shared"
        }
      }
    ]
}
```

In VSCode/VScodium selecteer ik **Terminal / Run Build Task...** in de menubalk en selecteer de juiste taak.

## Installeer de extensie

Na de build moet het zip-bestand van de extensie op dezelfde manier geïnstalleerd worden als elke andere extensie. Het moet worden ingeschakeld en eventuele opties moeten worden ingesteld. Jdocmanual heeft drie opties voor *Taxonomieën om te indexeren*:

- **Handleiding** één of alle beschikbare handleidingen (Gebruikershandleiding, Ontwikkelaarshandleiding, ...)
- **Taal** zoekopdrachten zijn beperkt tot artikelen in dezelfde taal als de paginataal.
- **Type** één of alle typen inhoud (Jdochandleiding, Inhoud, ...)

In het geval van de Jdocmanual-site zijn andere finder-plugins uitgeschakeld omdat er geen andere inhoud is om te indexeren.

Na de eerste installatie is het normaal gesproken voldoende om de build-taak opnieuw uit te voeren na het corrigeren van de code. Installatie van het zip-bestand is alleen vereist als er wijzigingen zijn in het manifest.xml-bestand.

## Indexeer de Inhoud

Jdocmanual is ingesteld om zijn artikelen te indexeren wanneer daarom wordt gevraagd door een Supergebruiker. Dit is anders dan de normale Content-plug-in die inhoud indexeert wanneer een artikel wordt opgeslagen. Een eerste uitvoering duurt een paar minuten om de ~5000 Jdocmanual-artikelen in 8 talen te indexeren.

## Maak een Slim Zoekmodule

In de **Inhoud / Site Modules** selecteer **Nieuw** en installeer een nieuwe **Slimme Zoek** module. Pas de instellingen aan om een geschikte weergave te verkrijgen.

## Testen

Uiteindelijk zou je aangepaste slimme zoekplugin moeten werken. Dit is een voorbeeld van een resultatenpagina voor Jdocmanual die naar een term op deze pagina zoekt. De resultatenpagina laat het zoekformulier in de titelbalk weg omdat het aanwezig is in de pagina-inhoud.

![Smart search result](../../../en/images/plugins/jdocmanual-search-result.png)

Een terzijde: de System - Joomla Accessibility Checker-plug-in geeft aan dat er 3 fouten zijn met betrekking tot het gegevensinvoerformulier *Zoektermen*. Dat vereist een kernreparatie of overschrijving.

*Vertaald door openai.com*

