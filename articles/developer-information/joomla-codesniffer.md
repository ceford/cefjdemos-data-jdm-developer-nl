<!-- Filename: Joomla_CodeSniffer / Display title: Codestandaarden -->

<div class="alert alert-warning">
Het laatste deel van dit artikel moet worden bijgewerkt!
</div>

## Een Overzicht van AI

Coderingstandaarden zijn belangrijk voor softwareontwikkeling omdat ze:

- **Verbeter de codekwaliteit**
  - Codeerstandaarden helpen ervoor te zorgen dat code betrouwbaar, veilig en veilig is. Ze kunnen ook helpen om prestatieproblemen en beveiligingsproblemen die kunnen voortvloeien uit slechte codeerpraktijken te verminderen.
- **Maak code leesbaarder en onderhoudbaarder**
  - Codeerstandaarden helpen om code gemakkelijker te begrijpen, te lezen en te onderhouden. Dit kan het ook gemakkelijker maken voor nieuwe ontwikkelaars om met de code te werken.
- **Versnel de ontwikkeling**
  - Codeerstandaarden kunnen ontwikkelaars helpen om veelvoorkomende fouten te vermijden die het coderingsproces kunnen vertragen.
- **Verbeter samenwerking**
  - Codeerstandaarden kunnen samenwerking tussen ontwikkelaars vergemakkelijken, zelfs in grote teams.
- **Zorg voor consistentie**
  - Codeerstandaarden helpen ervoor te zorgen dat code consistent is binnen projecten. Dit kan het gemakkelijker maken voor ontwikkelaars om samen te werken aan dezelfde projecten.
- **Verbeter de schaalbaarheid**
  - Codeerstandaarden kunnen helpen ervoor te zorgen dat code kan schalen zonder onhandelbaar te worden.
- **Bied duidelijke criteria voor codebeoordelingen**
  - Codeerstandaarden kunnen helpen duidelijke criteria te bieden voor codebeoordelingen, wat kan leiden tot effectievere feedback.

Codingsnormen omvatten vaak regels voor inspringing, regelafstand, haakjesplaatsing en spatiëring.

Joomla gebruikt de [PSR-12](https://www.php-fig.org/psr/psr-12/) codestandaard. Je kunt deze codestandaard in je IDE inschakelen om hints te krijgen als je de codestandaard niet volgt, of gebruik maken van een automatische correctie. We raden aan deze standaard te volgen bij het ontwikkelen van je eigen extensies om compatibel te blijven met de kern en om ervoor te zorgen dat je code hopelijk zal werken met toekomstige versies.

## PHP Code Sniffer

Raadpleeg de huidige [PHP_CodeSniffer](https://github.com/PHPCSStandards/PHP_CodeSniffer/) bron voor informatie over dit hulpmiddel en hoe het te installeren. Wees je ervan bewust dat de originele bron verlaten is, maar dat het mogelijk door zoekmachines wordt vermeld. Er zijn twee scripts:

- **phpcs** detecteert overtredingen van een coderingsstandaard en produceert een rapport.
- **phpcbf** probeert overtredingen van coderingsstandaarden te corrigeren en produceert een rapport over wat het heeft gedaan.

Je kunt de individuele scripts installeren of Composer gebruiken om ze te installeren. Je kunt ze ook vinden in een Joomla-kloon die zich bevindt in libraries/vendor/bin, samen met enkele andere nuttige hulpmiddelen.

## PHP Code Sniffer testen

Als je een globale installatie in je `$PATH` zet, kun je de code-sniffer vanaf de opdrachtregel in de hoofdmap van een project uitvoeren. Probeer dit om een lijst met codestandaarden weer te geven:

```sh
phpcs -i
The installed coding standards are MySource, PEAR, PSR1, PSR2, PSR12, Squiz and Zend
```

Als voorbeeld werd het volgende commando gebruikt in een oudere projectmap die de voormalige Joomla aangepaste coderingsstandaard gebruikte. Onder andere dingen werden tabs in plaats van spaties gebruikt voor de opmaak. Veel vergelijkbare regels zijn hieronder weggelaten voor beknoptheid.

```sh
phpcs --standard=PSR12 .

FILE: /Users/ceford/git/j4xdemos-plg-toc/j4xdemostoc/j4xdemostoc.php
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
FOUND 132 ERRORS AND 3 WARNINGS AFFECTING 116 LINES
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   1 | WARNING | [ ] A file should declare new symbols (classes, functions, constants, etc.) and cause no other side effects, or it should execute logic with side effects, but should not do both. The
     |         |     first symbol is defined on line 22 and the first side effect is on line 10.
   1 | ERROR   | [x] Header blocks must be separated by a single blank line
  22 | ERROR   | [ ] Each class must be in a namespace of at least one level (a top-level vendor name)
  24 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
  25 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
  26 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
  ...
  42 | ERROR   | [x] Expected 1 space after closing parenthesis; found newline
...
  48 | ERROR   | [x] No space found after comma in argument list
  48 | ERROR   | [x] Expected 1 space after closing parenthesis; found newline
...
  67 | ERROR   | [x] Expected 0 spaces before closing parenthesis; 1 found
...
  75 | ERROR   | [x] Space after opening parenthesis of function call prohibited
  75 | ERROR   | [x] Expected 0 spaces before closing parenthesis; 1 found
...
 138 | WARNING | [ ] Line exceeds 120 characters; contains 168 characters
 139 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
 139 | WARNING | [ ] Line exceeds 120 characters; contains 141 characters
...
 156 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
 157 | ERROR   | [x] Expected 1 newline at end of file; 0 found
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
PHPCBF CAN FIX THE 131 MARKED SNIFF VIOLATIONS AUTOMATICALLY
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Time: 123ms; Memory: 8MB
```

Het vorige voorbeeldproject bevatte een enkel php-bestand en geen css- of js-bestanden. phpcs produceert een rapport voor elk php-, css- en js-bestand in een project. Hier zijn enkele voorbeelden voor css- en js-bestanden:

```sh
FILE: /Users/ceford/git/j4x-demos-com-mediacat/com_mediacat/media/css/mediacat.css
----------------------------------------------------------------------------------
FOUND 33 ERRORS AFFECTING 32 LINES
----------------------------------------------------------------------------------
  3 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
...
 51 | ERROR | [x] Whitespace found at end of line
...
 79 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
----------------------------------------------------------------------------------
PHPCBF CAN FIX THE 33 MARKED SNIFF VIOLATIONS AUTOMATICALLY
----------------------------------------------------------------------------------

FILE: /Users/ceford/git/j4x-demos-com-mediacat/com_mediacat/media/css/file-icon-classic.min.css
-----------------------------------------------------------------------------------------------
FOUND 0 ERRORS AND 1 WARNING AFFECTING 1 LINE
-----------------------------------------------------------------------------------------------
 1 | WARNING | File appears to be minified and cannot be processed
-----------------------------------------------------------------------------------------------

FILE: /Users/ceford/git/j4x-demos-com-mediacat/com_mediacat/media/js/mediacat-site.js
-------------------------------------------------------------------------------------
FOUND 38 ERRORS AFFECTING 30 LINES
-------------------------------------------------------------------------------------
  3 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
  5 | ERROR | [x] Expected 1 space after FUNCTION keyword; 0 found
  6 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
...
 13 | ERROR | [x] Opening brace should be on a new line
...
 29 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
 29 | ERROR | [x] Expected at least 1 space before "+"; 0 found
 29 | ERROR | [x] Expected at least 1 space after "+"; 0 found
...
 36 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
-------------------------------------------------------------------------------------
PHPCBF CAN FIX THE 38 MARKED SNIFF VIOLATIONS AUTOMATICALLY
-------------------------------------------------------------------------------------
```

## Commandovariaties

Je kunt hulp krijgen met phpcs-commando's:

```sh
phpcs --help
```

### Een of meer bestanden uitsluiten

Gebruik een kommagescheiden lijst van bestands patronen om bestanden uit te sluiten van code stijlvalidatie. Voorbeeld

```php
phpcs --standard=PSR12 --ignore='libraries/*' .
```

### Eén of meer regels uitsluiten

Joomla staat langere regels toe dan de PSR-standaard, 560 in plaats van 120. De volgende opdracht kan worden gebruikt om de waarschuwingen voor lange regels over te slaan:

```sh
phpcs --standard=PSR12 --ignore='libraries/*' --exclude=Generic.Files.LineLength .
```

Je kunt de regel die wordt overtreden vinden met deze opdracht:

```sh
phpcs -s yourfile.php
```

### Joomla-uitzonderingen

Voor de ontwikkeling van een extensie met behulp van een IDE kun je ervoor kiezen om de PSR12-standaard zonder uitzonderingen te gebruiken. Joomla heeft een [aangepaste regels](https://github.com/joomla/joomla-cms/blob/5.2-dev/ruleset.xml) die veel uitzonderingen toestaat. Dit wordt gebruikt voor validatie van de gehele Joomla-installatie tijdens systeemtesten.

## Overtredingen oplossen

Het enkele php-bestand waarvan `FOUND 132 ERRORS AND 3 WARNINGS AFFECTING 116 LINES` hierboven wordt getoond, kan grotendeels als volgt worden opgelost:

```sh
phpcbf --standard=PSR12 .

PHPCBF RESULT SUMMARY
-----------------------------------------------------------------------------------
FILE                                                               FIXED  REMAINING
-----------------------------------------------------------------------------------
/Users/ceford/git/j4xdemos-plg-toc/j4xdemostoc/j4xdemostoc.php     131    4
-----------------------------------------------------------------------------------
A TOTAL OF 131 ERRORS WERE FIXED IN 1 FILE
-----------------------------------------------------------------------------------

Time: 232ms; Memory: 8MB
```

Het opnieuw uitvoeren van de phpcs-hulpprogramma laat zien welke problemen blijven:

```sh
phpcs --standard=PSR12 .

FILE: /Users/ceford/git/j4xdemos-plg-toc/j4xdemostoc/j4xdemostoc.php
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
FOUND 1 ERROR AND 3 WARNINGS AFFECTING 4 LINES
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   1 | WARNING | A file should declare new symbols (classes, functions, constants, etc.) and cause no other side effects, or it should execute logic with side effects, but should not do both. The first
     |         | symbol is defined on line 23 and the first side effect is on line 11.
  23 | ERROR   | Each class must be in a namespace of at least one level (a top-level vendor name)
 132 | WARNING | Line exceeds 120 characters; contains 168 characters
 133 | WARNING | Line exceeds 120 characters; contains 141 characters
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Time: 127ms; Memory: 8MB
```

Conclusie: een extensie die gecodeerd is voor een eerdere Joomla-versie en een andere codestandaard gebruikt, heeft nu wat werk nodig!

## Installatie in IDEs

De meeste geïntegreerde ontwikkelomgevingen kunnen de code sniffer gebruiken terwijl je werkt of wanneer je een bestand opslaat.

### Installatie in VSCode

In VSCode (en/of VScodium), selecteer het Extensies-icoon, zoek naar PHP_CodeSniffer en installeer het. Het heeft configuratie nodig:

1. Zoek in **Instellingen** naar PHP_CodeSniffer
2. Stel het veld **PHP Code Sniffer → Exec:** in om overeen te komen met de locatie van je phpcs-binary.
3. Selecteer in de **PHP Code Sniffer: Standaard** lijst **PSR12**.

Sluiten en je bent klaar voor actie.

Sommige van de hierboven getoonde problemen kunnen worden opgelost met PSR1-richtlijnen.

```php
<?php

/**
 * @package     Whatever
 *
 * @phpcs:disable PSR1.Classes.ClassDeclaration.MissingNamespace
 */

use joomla\CMS\...

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects
```

Anderen hebben wat nadenken nodig!

Na installatie in VSCode worden code stijl overtredingen gedetecteerd en gemarkeerd met een rode kronkelige onderstreping. Beweeg de muis over het probleem om een bericht en mogelijke oplossing te zien, zoals in dit voorbeeld:

```txt
Header blocks must be separated by a single blank line
PHP_CodeSniffer(PSR12.Files.FileHeader.SpacingAfterBlock)
```

<div class="alert alert-warning">
De volgende secties moeten worden bijgewerkt!
</div>

### Installatie in PhpStorm

Code Sniffer wordt standaard ondersteund in PhpStorm. Ga naar Instellingen en onder **Editor → Inspecties** zie je de lijst met sniffers die je hebt geïnstalleerd.

#### Stel pad in voor Code Sniffer

1. Open Instellingen (CTRL-ALT-S / CMD-,)
2. Ga naar Talen & Frameworks
3. Klik op PHP
4. Klik op Kwaliteitsinstrumenten
5. Klik op het dropdownpijltje van PHP cs fixer
6. De configuratie is standaard ingesteld op Lokaal
7. Klik op de 3 puntjes erachter om het configuratiescherm te openen
8. De eerste optie is het PHP Code Sniffer (phpcs) pad
9. Klik op de 3 puntjes achter het pad om de locatie van het phpcs-bestand te selecteren. Zie hierboven waar phpcs op uw site kan zijn geïnstalleerd
10. Klik op Valideren om zeker te zijn dat het pad correct is en phpcs werkt
11. Klik OK

#### De code stijl activeren

1.  Open Instellingen (CTRL-ALT-S / CMD-,)
2.  Ga naar Editor
3.  Klik op Inspecties
4.  Ga in de lijst naar PHP
5.  Klik op PHP Code Sniffer Validatie
6.  Klik op het selectievakje erachter om het te activeren
7.  Klik op de Herlaad-knop (2 pijlen) om een herlaad van regels vanaf schijf af te dwingen
8.  Joomla zou nu beschikbaar moeten zijn in de lijst. Zie volgende afbeelding.
9.  Klik op OK

![CodeSniffer in PHPStorm](../../../en/images/getting-started/core-phpstorm-code-sniffer.png)

### Installatie in Netbeans

Netbeans heeft de sniffer-functionaliteit geïntegreerd in het kernsysteem.

1.  Start je Netbeans IDE
2.  Open **Tools → Options → PHP → Code Analysis → Code Sniffer**
3.  Je moet het pad instellen naar *phpcs.bat* van het geïnstalleerde PHP_CodeSniffer PEAR-pakket
    - Voor op Unix gebaseerde systemen is het pad iets als /usr/bin/phpcs
    - In XAMPP (windows) kun je het bestand vinden in de PHP-rootmap (bijv. C:\xampp\php\phpcs.bat)
4.  Kies als "Default Standard" "Joomla" om de Joomla! standaard te gebruiken
5.  Nu kun je op *OK* klikken om te beginnen met sniffen
6.  Open vanuit het bovenste menu **Source → Inspect...**
7.  Geniet ervan

### Installatie in Eclipse

1. **Help → Nieuwe Software Installeren...**
2. **Werken met** Vul een van de update site-URL's in.
3. Selecteer de gewenste tools.
4. Start Eclipse opnieuw op.
5. **Venster → Voorkeuren**
6. **PHP-tools → PHP CodeSniffer**

![Eclipse PTI settings](../../../en/images/getting-started/core-eclipse-pti-settings.png)

Je kunt nu code-overtredingen opsporen tegen algemene normen.

![Codesniffer in Eclipse](../../../en/images/getting-started/core-eclipse-pti.png)

### Installatie in Geany

- Open een PHP-bestand. (Anders is het bouwmenu niet toegankelijk.)
- Kies in het bovenste menu **Build → Set Build Commands**.
- Selecteer het tweede veld en geef het een naam naar keuze. Voer deze code in bij de opdracht:<br>
  `phpcs --standard=Joomla "%f" | sed -e 's/^/%f |/' | egrep 'WARNING|ERROR'`
- Voer deze code in het veld voor Reguliere Expressie voor Fouten in: `(.+) [|]\s+([0-9]+)`
- Selecteer OK.
- Als het Berichtvenster niet open is, toon het door het te selecteren in het bovenste Zicht-menu.
- Wanneer je een PHP-bestand bekijkt, druk op F9 om de gevonden fouten te zien.

### Installatie in Atom

- Installeer phpcs en de Joomla-standaarden zoals hierboven beschreven.
- Installeer in **Atom → Preferences → Install** de **linter-phpcs** en al zijn vereisten.
- Pas aan in **Atom → Preferences → Packages → linter-phpcs → Settings** 
  - **Executable Path** naar het pad van je phpcs
  - **Code Standard Or Config File**: PSR12
  - **Tab Width**: 4

*Vertaald door openai.com*

