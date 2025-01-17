<!-- Filename: J4.x:Developer:_Required_Software / Display title: Vereiste Software -->

## Inleiding

Deze tutorial is gericht op nieuwkomers in Joomla-codeontwikkeling die extensies op een lokale computer willen voorbereiden. Het maakt niet uit of je Windows, Mac of Linux gebruikt. Alle benodigde software is beschikbaar voor al deze platforms. Om aan de slag te gaan op je eigen laptop of desktop, die meestal de domeinnaam *localhost* heeft, moet je een standaardset van afzonderlijke software-items installeren.

- **Apache**, een webserver. Andere webservers zijn beschikbaar, maar nieuwkomers moeten bij Apache blijven.
- **MySQL** of **MariaDB**, databaseservers. PostgreSQL wordt ook ondersteund, maar is niet voor nieuwkomers.
- **PHP**, de laatste Joomla aanbevolen versie of de minimumversie voor je platform.
- **phpMyAdmin** een tool die wordt gebruikt voor het beheren van MySQL en MariaDB databases.
- **xDebug** een extensie voor PHP die wordt gebruikt voor debugging.
- Een IDE zoals **VSCode**. Andere zijn beschikbaar en worden behandeld in een apart artikel.

## Softwarestapels

De eerste vier items in deze lijst worden vaak aangeduid als een stack en kunnen LAMP, MAMP of WAMP worden genoemd, waarbij de letters in het acroniem het volgende betekenen:

- **L, M of W** platform. L voor Linux, M voor Mac en W voor Windows.
- **A: Apache** webserver.
- **M: MySQL of MariaDB** database. De twee zijn uitwisselbaar.
- **P: PHP** scripttaal. Een veelgebruikte scripttaal. Er is geen alternatief omdat Joomla in PHP is gecodeerd.

## Verpakte Stapels

Een goede manier om te beginnen is door een pakket te gebruiken dat de essentiële software combineert:

- **WAMP** voor Windows is gratis van de [Wampserver](https://www.wampserver.com/en/) site.
- **Bearsampp** voor Windows is gratis van de [Bearsampp](https://bearsampp.com/) site. Het heeft meer tools.
- **XAMPP** Voor Windows, Mac en Linux is gratis van de [Apache Friends](https://www.apachefriends.org/) site. Er is een lokale handleiding voor [XAMPP](jdocmanual?article=user/hosting/local-hosting-with-xampp).
- **MAMP** voor Mac en Windows is beschikbaar in een gratis en commerciële versie van de [MAMP](https://www.mamp.info/en/mac/) site.

## Geen stapels

Als je een Linux- of Macintosh-computer hebt, zul je merken dat je elk van de vereiste software-items onafhankelijk kunt installeren vanuit de externe repositories die je besturingssysteem ondersteunen. Het is mogelijk dat ze al geïnstalleerd en klaar voor gebruik zijn. Om dit te testen, open je je favoriete webbrowser en voer je **localhost** in de url-balk in. Je ziet een tijdelijke aanduidingspagina of een verbindingsfoutpagina.

## Document rootdirectory van de webserver

Bij installatie zal uw Apache-server een standaard document-rootdirectory hebben ingesteld. De locatie verschilt per platform en u moet weten waar deze zich bevindt of een virtuele host maken om deze daar te plaatsen waar u het wilt. Voorbeeld van standaardlocaties:

- Mac OS: "/Library/WebServer/Documents"
- Linux: /var/www/html
- Windows: ...

Om later problemen met bestandsrechten te voorkomen, is het vaak handig om een virtuele host aan te maken die naar de public_html-map van je eigen bestandsruimte wijst. Dat kan zijn /home/gebruikersnaam/public_html op Linux of /Users/gebruikersnaam/Sites op Mac.

Dit is een voorbeeld van een Mac virtuele host invoer in het bestand /etc/apache2/vhosts/localhost.conf:

```bash
<VirtualHost *:80>
        DocumentRoot "/Users/username/Sites"
        ServerName localhost
        ErrorLog "/private/var/log/apache2/localhost-error_log"
        CustomLog "/private/var/log/apache2/localhost-access_log" common
        <Directory "/Users/username/Sites">
            AllowOverride All
            Require all granted
        </Directory>
</VirtualHost>
```

Als alternatief kunt u mogelijk een symbolische link maken van de standaard documentroot naar de public_html-map van uw persoonlijke bestandsruimte.

*Vertaald door openai.com*

