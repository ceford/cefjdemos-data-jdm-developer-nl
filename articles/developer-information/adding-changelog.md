<!-- Filename: Adding_changelog_to_your_manifest_file / Display title: Een wijzigingenlogboek toevoegen -->

Sinds Joomla 4.0 kunnen extensie-ontwikkelaars gebruikmaken van het vermogen van Joomla om een changelog-bestand te lezen en een visuele weergave van de changelog te geven. Als een bepaalde versie niet in de changelog wordt gevonden, zal de changelog-knop niet worden getoond.

De wijzigingen in een release zullen op deze manier worden gepresenteerd:

![changelog modal view](../../../en/images/developer-information/adding-changelog-example-1.png)

De changelog wordt op twee verschillende plaatsen gebruikt.

## Weergave bijwerken

De installer zal de changelog van de versie die geïnstalleerd kan worden laten zien, indien beschikbaar.

![changelog installer view](../../../en/images/developer-information/adding-changelog-update-view.png)

Door hier op de Changelog-knop te klikken, wordt de changelog van de nieuwe beschikbare versie weergegeven.

## Beheerweergave

De extensiebeheerder toont de wijzigingslijst van de momenteel geïnstalleerde extensie als deze beschikbaar is.

![changelog installer view](../../../en/images/developer-information/adding-changelog-extension-view.png)

Door op het versienummer hier te klikken, wordt de changelog van de momenteel geïnstalleerde versie weergegeven.

## Voeg changelogurl-tag toe aan manifestbestanden

De eerste stap is om uw manifestbestanden bij te werken die Joomla vertellen waar de details van de changelog te vinden zijn. Voeg de volgende node toe aan uw manifest XML-bestanden:

```
<changelogurl>https://example.com/updates/changelog.xml</changelogurl>
```

Let op: De URL in de changelogurl-tag mag geen spaties of regeleinden voor of na hebben. Zie codevoorbeelden.

### Servermanifest bijwerken

Zie dit voorbeeld van een update-server manifestbestand dat Joomla informeert over een update van een component genaamd "com_lists". Hierdoor ziet u de Wijzigingenknop in de updateweergave.

```
<?xml version="1.0" encoding="utf-8"?>
<updates>
 <update>
  <name>Student List</name>
  <description>List of students</description>
  <element>com_lists</element>
  <type>component</type>
  <version>4.0.0</version>

  <changelogurl>https://example.com/updates/changelog.xml</changelogurl>

  <tags>
   <tag>stable</tag>
  </tags>
  <maintainer>Example Miller</maintainer>
  <maintainerurl>https://example.com/</maintainerurl>
  <section>Updates</section>
  <targetplatform name="joomla" version="4.?" />
  <client>1</client>
  <folder></folder>
 </update>
</updates>
```

### Extensie manifest

Voeg daarnaast de tag `changelogurl` toe aan het extensiemanifest-XML. Op die manier wordt de extensieversie gekoppeld aan de wijzigingen in de beheermodus.

```
<?xml version="1.0" encoding="utf-8"?>
<extension type="component" method="upgrade">
    <name>COM_LISTS</name>

... Other stuff ...

    <changelogurl>https://example.com/updates/changelog.xml</changelogurl>

    <updateservers>
        <server type="extension" name="My Extension's Updates">https://example.com/lists-updates.xml</server>
    </updateservers>
</extension>
```

## Changelog-bestand maken

Het changelog-bestand moet de volgende 3 knooppunten hebben:

* element
* type
* versie

Deze informatie wordt gebruikt om de juiste changelog voor een bepaalde extensie te identificeren.

Een versie-knooppunt binnen een changelog-knooppunt is altijd verplicht. Anders zie je een foutmelding zoals SyntaxError: JSON.parse: onverwacht teken op regel 1 kolom 1 van de JSON-gegevens.

```
<element>com_lists</element>
<type>component</type>
<version>4.0.0</version>
```

Verder wordt de changelog gevuld met een of meer wijzigingstypes. De volgende wijzigingstypes worden ondersteund:

* beveiliging: Alle beveiligingsproblemen die zijn verholpen
* reparatie: Alle bugs die zijn verholpen
* taal: Dit is voor taalwijzigingen
* toevoeging: Alle nieuwe functies die zijn toegevoegd
* verandering: Alle wijzigingen
* verwijderen: Alle functies die zijn verwijderd
* opmerking: Alle extra informatie om de gebruiker te informeren

Elke node kan zo vaak als nodig worden herhaald.

De indeling van de tekst kan gewone tekst of HTML zijn, maar in het geval van HTML moet deze worden ingesloten in CDATA-tags zoals getoond in het voorbeeld.

```
<changelogs>
    <changelog>
        <element>com_lists</element>
        <type>component</type>
        <version>4.0.0</version>
        <security>
            <item>Item A</item>
            <item><![CDATA[<h2>You MUST replace this file</h2>]]></item>
        </security>
        <fix>
            <item>Item A</item>
            <item>Item b</item>
        </fix>
        <language>
            <item>Item A</item>
            <item>Item b</item>
        </language>
        <addition>
            <item>Item A</item>
            <item>Item b</item>
        </addition>
        <change>
            <item>Item A</item>
            <item>Item b</item>
        </change>
        <remove>
            <item>Item A</item>
            <item>Item b</item>
        </remove>
        <note>
            <item>Item A</item>
            <item>Item b</item>
        </note>
</changelog>
<changelog>
    <element>com_lists</element>
    <type>component</type>
    <version>0.0.2</version>
    <security>
        <item>Big issue</item>
    </security>
</changelog>
</changelogs>
```

Dit bestand bevat 2 changelogs:

* Versie 0.0.2 (voor het testen van de beheerweergave)
* Versie 4.0.0 (voor het testen van de bijwerkweergave)

Een changelog kan zo veel versies hebben als nodig is

*Vertaald door openai.com*

