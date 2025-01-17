<!-- Filename: J4.x:Setting_Up_Your_Local_Environment / Display title: Een Lokale Omgeving Opzetten -->

## Snelstartgids

Voor een testopstelling heb je een softwarestack nodig die Apache, MySQL en PHP bevat. De benodigde stappen worden elders in deze handleiding behandeld. Bij twijfel gebruik je een softwarestack die geschikt is voor jouw platform.

## Extra Gereedschappen

Voor het testen van Joomla pull requests en bijdragen aan de kerncode van Joomla heb je extra tools nodig, allemaal gemakkelijk te installeren, vaak met een enkel commando:

1. **Composer** - voor het beheren van Joomla's PHP-afhankelijkheden. Lees de [Composer-documentatie](https://getcomposer.org/doc/00-intro.md) voor hulp bij de installatie.
2. **Node.js** - voor het compileren van Joomla's JavaScript- en SASS-bestanden. Lees de [Node.js-documentatie](https://nodejs.org/en/) voor hulp bij de installatie.
3. **Git** - voor versiebeheer. Lees de [git-documentatie](https://git-scm.com/) voor hulp bij de installatie.

## Stappen om een Lokale Testsite Op te Zetten

1. Ga naar de [Joomla! repository op GitHub](https://github.com/joomla/joomla-cms).
2. Selecteer de Joomla-branch waaraan je wilt werken. Dit selecteert de juiste README-instructies.
3. Scroll naar beneden op de pagina naar het gedeelte **Stappen om de lokale omgeving op te zetten:** van de README-tekst.
4. Volg de daar vermelde stappen. Je kunt de naam van de gekloonde joomla-cms map veranderen zodat je zoveel klonen kunt maken als je wilt voor verschillende doeleinden.
5. Maak een database en installeer Joomla, maar verwijder de installatiemap niet aan het einde van het installatieproces.

## Joomla bijwerken

Van tijd tot tijd moet u mogelijk een Joomla-kloon bijwerken. Dit is een eenregelige opdracht die meestal snel is. Het is echter meestal noodzakelijk om de CSS- en JavaScript-bestanden opnieuw op te bouwen, wat een ingewikkelder en langer proces is.

Linux- en OSX-gebruikers kunnen de volgende bash-alias instellen door het volgende in het bestand *~/.bashrc* te plaatsen:

```sh
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
```

Dit zal alle gecompileerde bestanden verwijderen en een verse installatie uitvoeren als één commando door `jinstall` aan te roepen binnen je Joomla-installatie. Je kunt ook het `jclean` commando gebruiken om terug te schakelen naar een andere Joomla-branch.

**Opmerking:** U moet mogelijk `composer install` uitvoeren met de optie `--ignore-platform-reqs` om platformvereisten die in Composer zijn gespecificeerd te negeren. Dat wil zeggen, als u de LDAP-extensie van PHP niet geïnstalleerd heeft.

### Databasewijzigingen

Als een Joomla-update databasewijzigingen bevat, moet je mogelijk de individuele wijzigingen vinden en ze handmatig uitvoeren of opnieuw beginnen met een nieuwe kloon.

## Node/npm-scripts

De installatie van Joomla vanaf een repository-kloon gebruikte twee commando's:

- **composer install** Installeer alle benodigde composer pakketten.
- **npm ci** Installeer alle benodigde npm pakketten.

Node.js wordt geleverd met een pakketbeheerder genaamd NPM die een `run`-opdracht heeft. Er zijn enkele scripts beschikbaar om het bouwproces sneller te maken als alleen CSS- of JavaScript-bestanden zijn gewijzigd.

### npm run build:css

Deze opdracht compileert SASS-bestanden naar CSS en maakt ook de geminificeerde bestanden.

### npm run build:js

Deze opdracht compileert en transpileert de JavaScript-bestanden naar het juiste formaat en maakt verkleinde bestanden aan.

### npm uitvoeren kijken

Deze opdracht is hetzelfde als de `build:js` opdracht, maar zal veranderingen in de gaten houden en automatisch bijgewerkte bestanden bouwen in de media-directory. SASS-bestanden zijn nog niet inbegrepen.

### npm run lint:js

Deze opdracht voert een syntaxiscontrole uit op alle ES6 JavaScript-bestanden volgens de JavaScript-code standaard. Voor meer informatie zie de [Joomla codeerstandaarden handleiding](https://developer.joomla.org/coding-standards/introduction.html).

### npm run test

Deze opdracht zal een JavaScript-testpakket uitvoeren.

## Mogelijke Problemen

Bij het uitvoeren van composer install kun je deze fouten tegenkomen.

```sh
    Problem 1
        - Installation request for joomla/ldap 2.0.0-beta -> satisfiable by joomla/ldap[2.0.0-beta].
        - joomla/ldap 2.0.0-beta requires ext-ldap * -> the requested PHP extension ldap is missing from your system.
    Problem 2
        - Installation request for symfony/ldap v5.1.5 -> satisfiable by symfony/ldap[v5.1.5].
        - symfony/ldap v5.1.5 requires ext-ldap * -> the requested PHP extension ldap is missing from your system.
```

De oplossing is om de composer install uit te voeren met de optie `--ignore-platform-reqs` om platformvereisten die in Composer zijn gespecificeerd, te negeren. Dat wil zeggen, als je de LDAP-extensie van PHP niet hebt geïnstalleerd.

```sh
    composer install --ignore-platform-reqs
```

Als je een inlogfout ontvangt, verwijder dan het bestand `administrator/cache/autoload_psr4.php`.

*Vertaald door openai.com*

