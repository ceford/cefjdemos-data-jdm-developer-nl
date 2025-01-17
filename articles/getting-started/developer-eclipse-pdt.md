<!-- Filename: J4.x:Developer:_Eclipse_PDT / Display title: Eclipse PDT -->

## Inleiding

Als PHP-ontwikkelaar heb je enkele tools nodig om je te helpen bij je werk. Eclipse is een IDE (Integrated Development Environment) die kan worden gebruikt voor allerlei soorten projecten in allerlei programmeertalen. Eclipse PDT (PHP Developer Tools) bevat de uitbreidingen die nodig zijn voor PHP-ontwikkeling. Enkele van de functies die op de startpagina worden vermeld, zijn:

- Syntaxiskleuring
- Syntaxisvalidatie
- Inhoudassistentie
- Codenavigatie
- PHP-foutopsporing (Zend Debugger / Xdebug)
- PHP-profielen (Zend Debugger / Xdebug)

Voeg daarbij de mogelijkheid om bestanden te kopiëren naar waar ze moeten zijn in je lokale testwebsite en een zip-bestand te maken, er is genoeg om jaren mee te werken. Het kost tijd om te leren hoe je Eclipse PDT moet gebruiken, een dag of zo, maar het loont. Er zijn andere IDE's en code-editors beschikbaar!

### Alternatieven

**IDEs** hebben alle toeters en bellen die nodig zijn om PHP-projecten te bouwen. Platformonafhankelijke voorbeelden waarvan bekend is dat ze door sommige Joomla-ontwikkelaars worden gebruikt, zijn onder andere:

- **JetBrains PhpStorm** Commercieel, Cross Platform.
- **Apache NetBeans** Gratis, Open Source, Cross Platform.
- **Visual Studio Code** Gratis, Cross Platform.

**PHP-editors** zijn goed voor het bewerken van code, maar missen enkele toeters en bellen die nodig zijn voor grote projecten. Voorbeelden zijn:

- **Notepad++** Gratis, alleen voor Windows.

## Eclipse PDT installeren

Ga naar de [Eclipse PDT](https://www.eclipse.org/pdt/) website en download de versie die beschikbaar is voor jouw platform (Linux, Mac of Windows).

Volg de installatie-instructies voor je platform.

## De Eclipse-werkruimte

Er zijn ten minste drie verschillende plaatsen waar je projectcode zich zal bevinden:

- **Projectbronbestanden** zijn bestanden die je zelf maakt en bewerkt. Ze mogen niet in je webstructuur staan. Ze moeten in je persoonlijke bestandsruimte staan. Voorbeelden, waarbij elk project in een aparte submap staat:
  - /Users/username/git (Mac)
  - /home/username/git (Linux)
  - ... (Windows)
- **Webstructuur** locatie hangt af van de installatie van je softwarestack. Je kunt je eigen bestandsruimte gebruiken. Voorbeelden:
  - /Users/username/Sites/j4test (Mac)
  - /home/username/public_html/j4test (Linux)
  - ... (Windows).
- **Eclipse Workspace** is waar Eclipse informatie over individuele projecten opslaat. De standaardlocatie is de root van je eigen bestandsruimte. Voorbeelden:
  - /Users/username/eclipse-workspace (Mac)
  - /home/username/eclipse-workspace (Linux)
  - ... (Windows).

Klaar om Eclipse te starten?

## Start Eclipse

Na het opstarten wordt u gevraagd om verschillende instellingen te bevestigen. Op een gegeven moment wordt u gevraagd om een werkruimte te kiezen. De standaard is /home/username/eclipse-workspace, maar u wilt een submap voor elk project maken, dus selecteer de Bladeren-knop en vervolgens de Nieuwe map-knop. In het onderstaande voorbeeld heb ik een map genaamd j4tutorials aangemaakt.

![Choose eclipse workspace location](../../../en/images/getting-started/eclipse-pdt-choose-workspace.png)

Selecteer de knop **Starten**. Als er iets mis is, kun je later Bestand / Wissel Werkruimte selecteren en het opnieuw proberen. Ongebruikte werkruimten kun je later ook verwijderen.

Bij de eerste opstart wordt een Welkomstscherm weergegeven. Je kunt dit scherm ook bereiken via de menu-items **Help / Welkom**.

![Eclipse welcome screen](../../../en/images/getting-started/eclipse-pdt-welcome.png)

Begin met Beoordeel IDE-configuratie-instellingen. Je kunt een Vinkje of een Kruisje zetten bij degenen die gepast lijken, of je kunt een Vinkje of een Kruisje weghalen of verder gaan als je twijfelt. Je kunt de instellingen later ook opnieuw wijzigen.

## Maak een nieuw PHP-project

Het eerste project dat moet worden toegevoegd is de code van de testwebsite. Je hebt dit nodig om te controleren of je eigen bestanden op de juiste plaatsen worden gekopieerd en voor later debuggen. Selecteer het item in het Welkomstscherm. Als je bent overgestapt naar de Eclipse werkbank, selecteer dan Maak een PHP-project uit de Projectverkenner. In het formulier Nieuw PHP-project:

- Voer een geschikte projectnaam in. Het moet een kort woord zijn dat in de Project Explorer zal verschijnen.
- Selecteer de radio-knop **Project maken op bestaande locatie (van bestaande bron)**.
- **Blader** naar de locatie van uw Joomla testwebsite en selecteer deze.
- Selecteer **Voltooien**.

![Eclipse new php project](../../../en/images/getting-started/eclipse-pdt-new-project.png)

Je websitebestanden worden toegevoegd aan de Projectverkenner. Het kan een minuut of twee duren voordat Eclipse dit proces heeft voltooid, omdat het wat voorbereiding achter de schermen doet. Je kunt het Project selecteren om het eerste mapniveau uit te vouwen.

Twee bestanden worden toegevoegd aan je site map: .buildpath en .project. Omdat ze beginnen met een punt (.) zie je ze misschien niet en hoef je je er geen zorgen over te maken. Het zijn xml-bestanden die informatie bevatten die door Eclipse wordt gebruikt.

## Het PHP Perspectief

De verzameling panelen in het Eclipse-scherm staat bekend als een perspectief. De twee meest gebruikte zijn het PHP-perspectief en het Debug-perspectief. Je kunt tussen hen wisselen met behulp van de pictogrammen rechtsboven in de werkbalk. Je kunt ook het menu gebruiken: Venster / Perspectief / Open Perspectief / PHP of Debug.

De volgende illustraties tonen het PHP-perspectief met een paar bewerkformulieren open.

![Eclipse php perspective](../../../en/images/getting-started/eclipse-pdt-php-perspective.png)

De Eclipse-panelen:

- Het linker paneel is de Projectverkenner waar je door je bestandsstructuur kunt navigeren.
- Het bovenste middenpaneel is waar geopende teksteditors verschijnen.
- Het bovenste rechter paneel toont normaal gesproken een overzicht van het geselecteerde bewerkpaneel. Het kan ook andere informatie tonen.
- Het onderste rechter paneel toont een van een aantal weergaven, selecteerbaar via het menu Venster / Weergave tonen.

De Probleemweergave geeft aan Fouten (100 van 791 items) en Waarschuwingen (100 van 11629). Dit lijkt veel, maar niets om je zorgen over te maken.

## Een nieuw Hello Universe PHP-project

Je bent nu klaar om een nieuw PHP-project te maken voor de extensie die je wilt ontwikkelen. Ter illustratie kun je beginnen met een eenvoudig component genaamd com_hellouniverse. Voor mij zal de Hello Universe-code zich bevinden in /Users/username/git/j4xtutorials-com-hellouniverse en ik zal j4xtutorials gebruiken als mijn namespace-affiliatie (het eerste woord in de componentnaamruimte).

In het Eclipse-menu selecteer je Bestand / Nieuw / PHP-project. Het formulier is het hierboven geïllustreerde. Mijn gegevens:

- Projectnaam: HelloUniverse
- Inhoud: selecteer Project maken op bestaande locatie (van bestaande bron).
- Bladeren: Navigeer naar /Home/username/git en selecteer de knop Nieuwe map.
- Voer in het dialoogvenster Nieuwe map j4xtutorials-com-hellouniverse in en druk op Maken.
- Open die lege map en selecteer Openen.
- De geselecteerde map zou moeten zijn: /Users/username/git/j4xtutorials-com-hellouniverse
- Selecteer Voltooien.

Je ziet nu het nieuwe project in de Project Explorer samen met je websiteproject. Het nieuwe project bevat twee items, beide gerelateerd aan PHP-ondersteuning. Er zijn enkele verborgen items die door Eclipse worden gebruikt: .settings(map), .buildpath en .project.

## Voeg com_hellouniverse Map Toe

Met het HelloUniverse-project geselecteerd:

- Selecteer het menu-item Bestand of klik met de rechtermuisknop op de projectnaam.
- Selecteer de menu-items Nieuw / Map.
- Voer in het Nieuwe Map-formulier com_hellouniverse in het veld Mapnaam: in.
- Selecteer Voltooien.

De map com_hellouniverse is waar je extensiecode zal komen. Alles in die map zal in een zip-bestand terechtkomen dat je kunt gebruiken om te installeren in Joomla. Alles wat niet in die map zit, wordt voor andere doeleinden gebruikt. Twee veelvoorkomende bestanden op dit niveau:

- README.md is een tekstbestand met Markdown-indeling dat de component beschrijft. Zulke bestanden worden vaak gebruikt in combinatie met externe codeopslag, zoals op Github (daarover elders meer).
- build.xml is een bestand dat instructies bevat over wat te doen nadat je wijzigingen in de code hebt aangebracht. Meestal wil je gewijzigde bestanden naar de juiste plaatsen in je websiteboom kopiëren en het zipbestand opnieuw genereren. Vervolgens kun je voor de meeste wijzigingen gewoon de webpagina opnieuw laden waar je aan werkt. Je hoeft de component niet opnieuw te installeren. Dit bespaart enorm veel tijd!

## Voeg admin, site en media mappen toe

- Selecteer de map com_hellouniverse en gebruik de menu-opties Bestand / Nieuw / Map om een nieuwe map te maken genaamd admin.
- Opnieuw, maak deze keer een nieuwe map genaamd media. PAS OP: zorg ervoor dat de bovenliggende map com_hellouniverse is en niet HelloUniverse.
- Opnieuw, maak deze keer een nieuwe map genaamd site. Als deze op de verkeerde plaats is gemaakt, kan deze naar de juiste plaats worden gesleept.

## build.xml

De volgende code bevat een voorbeeld van een build.xml-bestand dat in de root van je HelloUniverse-project moet worden geplaatst.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="hellouniverse" basedir="." default="main">
    <property file=".project" />

    <!-- Source and Destinations -->

    <property name="package"  value="${phing.project.name}" override="true" />
    <property name="srcdir" value="${project.basedir}/com_hellouniverse" override="true" />
    <property name="sitedir" value="/Users/username/Sites/j4tutorials"  override="true" />
    <property name="zipsdir" value="/Users/username/zips"  override="true" />

    <!-- Filesets -->

    <fileset dir="${srcdir}/admin" id="adminfiles">
        <include name="**" />
    </fileset>
    <fileset dir="${srcdir}/media" id="mediafiles">
        <include name="**" />
    </fileset>
    <fileset dir="${srcdir}/site" id="sitefiles">
        <include name="**" />
    </fileset>
    <fileset dir="${srcdir}" id="allfiles">
        <include name="**" />
    </fileset>

    <!-- Targets -->

    <target name="main" description="main target">
        <copy todir="${sitedir}/administrator/components/com_hellouniverse">
            <fileset refid="adminfiles" />
        </copy>
        <copy todir="${sitedir}/media/com_hellouniverse">
            <fileset refid="mediafiles" />
        </copy>
        <copy todir="${sitedir}/components/com_hellouniverse">
            <fileset refid="sitefiles" />
        </copy>
        <zip destfile="${zipsdir}/com_hellouniverse.zip">
            <fileset refid="allfiles" />
        </zip>
    </target>
</project>
```

Een paar verklaringen:

- **srcdir** is de locatie van het bestandssysteem van je broncode, waar bestanden worden gekopieerd van.
- **sitedir** is de locatie van het bestandssysteem van je testsite, waar bestanden naar worden gekopieerd.
- **zipsdir** is een locatie voor het zip-bestand - ik vind het handig om project-zip-bestanden van verschillende projecten samen in een zips-map te bewaren.
- **Bestandensets** zijn bronroepen die naar verschillende bestemmingen worden gekopieerd. betekent alle mappen en bestanden.
- **Doelen** zijn de bestemmingen en wat daarheen moet worden gekopieerd.

Maak een build.xml-bestand met de bovenstaande code:

- Selecteer Bestand / Nieuw / XML-bestand in het Eclipse-menu.
- Zorg ervoor dat HelloUniverse is geselecteerd als de bovenliggende map en voer de naam build.xml in (precies zo).
- Selecteer Voltooien
- Kopieer en plak in de Bronweergave (kies linksonder) de bovenstaande code in plaats van de ene regel die er al staat.
- Opslaan.

Om dit te laten werken, heb je een build-tool nodig: Phing.

## De Phing-buildtool¶

Ga naar de [Phing](https://www.phing.info/) website en download het Phar-archief. Je kunt deze [directe link](https://www.phing.info/get/phing-latest.phar) gebruiken. Sla het bestand op in een phing-map in je persoonlijke bin-map /Users/username/bin/phing. Verander de bestandspermissies naar 755, zodat het kan worden uitgevoerd vanaf de command line. Om te testen, open een terminal, ga naar je bin/phing map en voer ./phing-latest.phar -v in om het versienummer te zien. Onthoud het volledige pad naar het bestand:

- /Gebruikers/gebruikersnaam/bin/phing/phing-laatste.phar

Je zult het later nodig hebben.

## Eclipse Voorkeuren - Bouwers

Dit is waar je Phing configureert om je project te bouwen telkens wanneer je code wijzigingen aanbrengt.

- In Project Explorer, selecteer het HelloUniverse-project.
- Selecteer de menu-opties Bestand / Eigenschappen.
- Selecteer in het Eigenschappen-venster Bouwers en daarna de knop Nieuw.
- Selecteer in het Configuratietype-venster Programma en vervolgens OK.
- In het venster Eigenschappen bewerken startconfiguratie voer een geschikte titel in: Bouw HelloUniverse.
- Voer in het locatieveld het pad in naar het phing-programma of gebruik de knop Blader door bestandsysteem om ernaar te zoeken: /Users/ceford/bin/phing/phing-latest.phar
- Voor het veld Werkdirectory, selecteer de knop Blader door werkruimte en kies vervolgens HelloUniverse.
- Klik op OK, dan Toepassen en Sluiten.

Om te testen: selecteer de menu-items Project / Build Project. De Console-weergave onderaan het scherm toont de voortgang van de build. In deze fase is het waarschijnlijk dat BUILD FAILED in rood wordt weergegeven. Dat wordt verwacht - de componentmappen worden aangemaakt bij de installatie van het zip-bestand. Ze bestaan nog niet omdat er geen code is. Maar het toont aan dat de Phing-buildtool werkt.

## Hoe te Debuggen

Wanneer er iets misgaat, wil je misschien stap voor stap door de code gaan om te zien wat er daadwerkelijk gebeurt. Als je Debug System op Ja hebt ingesteld en Foutenrapportage op Maximum in de Joomla Global Configuration, heb je mogelijk een stacktrace om te laten zien waar een fout in de code optreedt. Of je ziet iets ongewoons in de uitvoer en hebt een goed idee waar de fout zou kunnen zitten. In beide gevallen moet je breekpunten instellen waar de uitvoering zal pauzeren, zodat je de waarden van variabelen kunt zien en stap voor stap door de code kunt gaan.

### Stel een Breakpoint in

Breekpunten moeten worden ingesteld in het websiteproject, J4Tutorials. Als voorbeeld, open in het Project Explorer-paneel het J4Tutorials-project en vind administrator / components / com_users / src / Model en dubbelklik op UsersModel.php om het te openen in een bewerkingsvenster. Ga naar regel 87 en dubbelklik op het nummer 87. Een kleine blauwe markering verschijnt om aan te geven dat er een breekpunt is ingesteld. De code-uitvoering moet hier stoppen wanneer je de lijst van gebruikers bekijkt via de menu-items Administrator / Users / Beheren.

### Een Debug Configuratie Maken

Vanuit het bovenste menu selecteer Uitvoeren / Debugconfiguraties. Voer in het formulier een herkenbare titel in, bijvoorbeeld Debug Admin, om later uit een vervolgkeuzelijst te selecteren. Zorg ervoor dat het geselecteerde bestand /J4Tutorials/administrator/index.php is. Selecteer in het formulier in het paneel Algemeen Debuggen uit de lijst Weergave in favorietenmenu. Controleer of Debugger is ingesteld op Xdebug. Selecteer Toepassen en Sluiten.

Je kunt nu Debug Admin selecteren in de debug keuzelijst van de Werkbalk, het bug symbool pictogram.

![Eclipse debug tools](../../../en/images/getting-started/eclipse-pdt-debug-tools.png)

Foutopsporingspictogrammen, zweef voor labels:

- **Alle Breakpoints Overslaan** Een schakelaar om breakpoints over te slaan behalve de eerste regel van index.php.
- **Hervatten** Ga door met normale uitvoering tot het volgende breakpoint.
- **Beëindigen** Stop met debuggen (maar je moet ook de Xdebug-cookie verwijderen).
- **Binnenstappen** Op een regel met een functieroep, stap in die functie.
- **Overslaan** Op een regel met een functieroep, ga niet die functie binnen.
- **Uitstappen** In een functie, stop niet op elke regel en keer terug naar de aanroeper.

### Debugprocedure

Bij het starten van een debugsessie pauzeert de uitvoering bij de eerste regel van administrator/index.php. Dat is de Home Dashboard pagina. Je moet het Hervatten-symbool selecteren (gele balk en groene rechterdriehoek). Na een vertraging van een seconde of zo is er meer activiteit en worden er meer gepauzeerde Remote launches weergegeven in het linker Debug Admin-paneel. Dit is de Homepagina die ajax gebruikt om update-informatie op te halen. Blijf gewoon op Hervatten klikken totdat de activiteit is gestopt. Selecteer vervolgens in je browser het onderdeel Gebruikers van het Home Dashboard. Eclipse komt tot leven en stopt de uitvoering op het door jou ingestelde breakpoint.

![Eclipse PHP debug perspective](../../../en/images/getting-started/eclipse-pdt-debug-perspective.png)

Kenmerken om op te merken:

- De huidige coderegel is gemarkeerd met een lichtgroene achtergrond.
- Het linkerpaneel toont een stapeltrace - de vorige functieaanroepen die tot de huidige functie hebben geleid. Selecteer een item om de bovenliggende code te zien.
- Het rechterpaneel kan ofwel actieve variabelen of onderbrekingspunten tonen. Vouw objecten uit indien nodig om details te zien.

### Enkele problemen

- Niet stoppen bij breakpoints: Dit gebeurt als je een ander PHP-programma opent, zoals phpMyAdmin. Om dit op te lossen:
  - Ga naar Uitvoeren / Debugconfiguraties en selecteer je Debug Admin-item.
  - Selecteer Configureren... in het tabblad Debugger.
  - In Padmapping selecteer en verwijder je elk pad dat niet gerelateerd is aan je testwebsite.
  - Selecteer Voltooien - je moet mogelijk de debug-sessie beëindigen en opnieuw laden.
- Debuggen wordt hervat na het selecteren van de Beëindigen-knop. Om dit op te lossen:
  - Gebruik de ontwikkelaarstools van je browser om een inspectievenster te openen.
  - Vind de cookies die je browser gebruikt (Opslag in Firefox).
  - Selecteer en verwijder de XDebug-cookie.

## Git gebruiken

Git is een versiebeheersysteem dat veel wordt gebruikt voor het beheren van allerlei soorten tekst, meestal code, maar het kan van alles zijn. Het werkt vanaf een opdrachtregel, maar wordt meestal opgeroepen door grafische tools zoals Eclipse. Als je meer wilt lezen over git, zie de [git website](https://git-scm.com/).

Als je git wilt gebruiken, moet je het installeren voor je platform. Selecteer vervolgens in Eclipse je HelloUniverse-project, klik met de rechtermuisknop en selecteer Team / Project Delen. Selecteer in het Project Delen dialoogvenster de optie **Gebruik of creëer repository in de bovenliggende map van het project** en selecteer het HelloUniverse-project uit de lijst. Er is een veld om aan te geven waar de repository zal worden aangemaakt (/Users/ceford/git/j4xtutorials-com-hellouniverse) - selecteer de aangrenzende Maak Repository knop. Selecteer ten slotte de Voltooien knop.

![Configure Git Repository](../../../en/images/getting-started/eclipse-pdt-configure-git.png)

U zult merken dat er enkele nieuwe decoraties zijn verschenen naast de items in de Project Explorer-weergave:

- De \> markering geeft aan dat er inhoud is gewijzigd, maar nog niet naar de repository is gecommitteerd.
- De ? markering geeft aan dat dit item niet is toegevoegd aan de lijst van te volgen items.

![Git Markers](../../../en/images/getting-started/eclipse-pdt-git-markers.png)

Je bent nu ingesteld voor versiebeheer. Klik met de rechtermuisknop op het project en bekijk hoe het Team-menu eruitziet. Veel te veel om hier te behandelen!

## Eindelijk

Klaar om aan je eigen code te beginnen werken?

*Vertaald door openai.com*

