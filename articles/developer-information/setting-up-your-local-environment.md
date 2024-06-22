<!-- Filename: J4.x:Setting_Up_Your_Local_Environment / Display title: Je lokale omgeving opzetten -->

<span id="main-portal-heading">Handleiding  
Hoe zet je een lokale omgeving voor Joomla 4 op</span> Joomla!  4.x

Met de komst van Joomla! 4 hebben we het ontwikkelproces gewijzigd. Het
is niet meer mogelijk om de repository te klonen om een bruikbare Joomla
installatie te verkijgen. We volgen beproefde werkwijzen en voeren een
bouwproces uit voor het CMS.

## Snelle starthandleiding

De stappen om je ontwikkelomgeving op te zetten zijn afhankelijk van je
besturingssysteem. We kunnen geen documentatie schrijven voor ieder OS,
gebruik alstublieft je zoekmachine om een "Hoe te" te vinden.

### Wat je nodig hebt

1.  PHP - vrijwel hetzelfde als je nodig hebt om een Joomla site te
    laten draaien, maar je hebt de PHP CLI (command line interface)
    versie nodig. (Lees ook deze pagina [Configuring a LAMPP server for
    PHP
    development](https://docs.joomla.org/Configuring_a_LAMPP_server_for_PHP_development "Special:MyLanguage/Configuring a LAMPP server for PHP development").)
2.  Composer - voor het beheren van de PHP-afhankelijkheden van Joomla.
    Lees voor hulp bij het installeren van Composer de documentatie:
    <a href="https://getcomposer.org/doc/00-intro.md" class="external free"
    target="_blank"
    rel="nofollow noreferrer noopener">https://getcomposer.org/doc/00-intro.md</a>.
3.  Node.js - voor het compileren van Joomla's JavaScript- en
    SASS-bestanden. Raadpleeg voor hulp bij het installeren van Node.js
    de instructies op
    <a href="https://nodejs.org/en/" class="external free" target="_blank"
    rel="nofollow noreferrer noopener">https://nodejs.org/en/</a>. Let
    op je hebt NodeJS 12 of hoger nodig om Joomla te installeren.
4.  Git - voor versiebeheer.

### Stappen om de lokale omgeving op te zetten

1.  Kloon de repository
2.  Checkout branch *4.0-dev*
3.  Voer `composer install` uit vanaf de root van de git repo. (Je kunt
    *--ignore-platform-reqs* toevoegen als je lokaal geen PHP-LDAP
    geïnstalleerd hebt en het niet nodig hebt.)
4.  Voer `npm ci` uit vanaf de root van de git repo. (Je hebt hiervoor
    npm 6.13.4 of hoger nodig. Voer `npm install -g npm@lts` uit om je
    npm-versie bij te werken naar de LTS-versie.)

Linux- en OSX- gebruikers kunnen de volgende bash-alias opzetten door
het volgende te plaatsen in het *~/.bashrc bestand*:

    alias jclean="rm -rf administrator/templates/atum/css; \
    rm -rf templates/cassiopeia/css; \
    rm -rf administrator/templates/system/css; \
    rm -rf templates/system/css; \
    rm -rf media/; \
    rm -rf node_modules/; \
    rm -rf libraries/vendor/; \
    rm -f administrator/cache/autoload_psr4.php; \
    rm -rf installation/template/css"
    alias jinstall="jclean; composer install; npm ci"

Dit zal alle gecompileerde bestanden van je systeem verwijderen en een
nieuwe installatie uitvoeren als één commando door het aanroepen
`jinstall` van in je Joomla installatie. Je kunt ook het `jclean`
commando gebruiken om naar een Joomla 3.x branch terug te gaan.

## Een iets langere start handleiding

Joomla is vergelijkbaar met een groot aantal andere web tools. Het heeft
een groot PHP-gedeelte en steeds meer JavaScript code. Voor PHP-codering
is niet zo veel voorbereiding nodig, maar JavaScript vraagt om veel
sleutelwerk. De voornaamste reden is dat niemand code schrijft die elke
browser begrijpt, dus de code moet worden omgezet van bijvoorbeeld ES6
naar een compatible versie van JavaScript. Hetzelfde geldt voor CSS.
Voor Joomla gebruiken we SASS, dat wordt geconverteerd naar native CSS,
zodat elke browser het begrijpt. Een ontwikkelomgeving opzetten is iets
gecompliceerder, maar het sleutelen maakt het coderen gemakkelijker.
Dankzij watchers en het automatisch opnieuw laten van je browser kun je
je wijzigingen in real time zien.

### PHP

Het zou voldoende moeten zijn om `composer install` uit te voeren,
aangezien dit de PHP-afhankelijkheden zal installeren die zijn
opgeslagen in het *composer.lock* bestand. Je kunt dit zo vaak doen als
je wilt. Het installeert alleen nieuwe pakketten als het *composer.lock*
bestand is gewijzigd. Voer `composer update` niet uit, want dit zal al
je pakketten bijwerken naar nieuwere versies en het *composer.lock*
bestand updaten.

**Let op** het kan nodig zijn om de `composer install` uit te voeren met
de `--ignore-platform-reqs` optie om de platform vereisten te negeren
die zijn gespecificeerd in Composer. Dit is van toepassing als je geen
PHP's LDAP extensie hebt geïnstalleerd.

### Node/npm scripts

Node.js heeft een pakketmanager, NPM (in sommige opzichten hetzelfde als
Composer). NPM heeft een `run` commando en we hebben een aantal scripts
voorbereid om het gemakkelijker te maken. Je voert de commando's uit
vanaf de root van de repository als je JS of SASS bestanden hebt
aangepast. Voorafgaand moet je eenmaal `npm ci` draaien, om
afhankelijkheden te installeren.

#### npm run build:css

Dit compileert SASS-bestanden naar CSS en maakt ook de minified
bestanden aan.

#### npm run build:js

Dit compileert de JavaScript-bestanden en zet ze om naar het juiste
bestandsformaat, en het maakt de minified bestanden aan.

#### npm run watch

Dit is hetzelfde als het `build:js` commando maar het kijkt naar
wijzigingen en creëert automatisch bijgewerkte bestanden in de media
map. SASS bestanden worden nog niet opgenomen.

#### npm run lint:js

Dit voert een syntax controle uit op alle ES6 JavaScript bestanden ten
opzichte van de javascript code standaard (lees, voor meer informatie
over de Joomla 4 codestijl standaard alstublieft de coding standaard
handleiding op <a
href="https://developer.joomla.org/coding-standards/introduction.html%7C,"
class="external text" target="_blank" rel="noreferrer noopener">het
coding standards manual</a>.

#### npm run test

Dit voert een serie JavaScript tests uit.

## Mogelijke problemen

Bij het installeren van composer kun je tegen deze fouten oplopen

    Problem 1
        - Installation request for joomla/ldap 2.0.0-beta -> satisfiable by joomla/ldap[2.0.0-beta].
        - joomla/ldap 2.0.0-beta requires ext-ldap * -> the requested PHP extension ldap is missing from your system.
    Problem 2
        - Installation request for symfony/ldap v5.1.5 -> satisfiable by symfony/ldap[v5.1.5].
        - symfony/ldap v5.1.5 requires ext-ldap * -> the requested PHP extension ldap is missing from your system.

De oplossing kan zijn om de `composer install` uit te voeren met de
`--ignore-platform-reqs` optie om de platform requirements te negeren
die zijn gespecificeerd in Composer. Dit is van toepassing als je geen
PHP's LDAP extensie hebt geïnstalleerd.

    composer install --ignore-platform-reqs

Als u een login error krijgt als hieronder getoond, verwijder dan het
`library/autoload_psr4.php` bestand, zoals in de tweede afbeelding
getoond..

<img
src="https://docs.joomla.org/images/thumb/b/b3/Install-error.png/400px-Install-error.png"
class="thumbborder" decoding="async"
srcset="https://docs.joomla.org/images/thumb/b/b3/Install-error.png/600px-Install-error.png 1.5x, https://docs.joomla.org/images/thumb/b/b3/Install-error.png/800px-Install-error.png 2x"
data-file-width="1920" data-file-height="1080" width="400" height="225"
alt="Login After Install Error" />
