<!-- Filename: XAMPP / Display title: XAMPP -->

## Inleiding

XAMPP is een eenvoudig te installeren pakket dat de Apache-webserver, PHP, XDEBUG en de MySQL-database bundelt. Hiermee kun je de omgeving creëren die je nodig hebt om Joomla! op je lokale machine te draaien. De nieuwste versie van XAMPP is beschikbaar op <a href="http://www.apachefriends.org/en/index.html" class="external text" target="_blank" rel="nofollow noreferrer noopener">de XAMPP-website</a>. Downloads zijn beschikbaar voor Linux, Windows, Mac OS X en Solaris. Download het pakket voor jouw platform.

*Belangrijke Opmerking Betreffende XAMPP en Skype:* Apache en Skype gebruiken beide poort 80 als alternatief voor inkomende verbindingen. Als je Skype gebruikt, ga dan naar het paneel Extra-Opties-Geavanceerd-Verbinding en deselecteer de optie "Gebruik 80 en 443 als alternatieven voor inkomende verbindingen". Als Apache als een service start, zal het poort 80 nemen voordat Skype start en zul je geen probleem ondervinden. Maar om zeker te zijn, schakel de optie in Skype uit.

### Installatie op Windows

Installatie voor Windows is heel eenvoudig. U kunt de XAMPP installer gebruiken uitvoerbaar bestand (bijvoorbeeld, "xampp-windows-x64-7.4.4-0-VC15-installer.exe"). Gedetailleerde installatie-instructies voor Windows zijn beschikbaar <a href="https://www.apachefriends.org/download.html" class="external text" target="_blank" rel="nofollow noreferrer noopener">hier</a>.

Als u Windows XP of 2003 gebruikt, worden deze niet ondersteund door het hoofd-
pakket, maar er zijn compatibele versies van XAMPP voor deze platforms
beschikbaar op de downloadpagina (maar u kunt alleen PHP 5.4 of
lager uitvoeren - en daarom alleen Joomla 3.x en lager testen).

Voor Windows wordt aanbevolen om XAMPP te installeren in "c:\xampp" (niet in
"c:\program files"). Als je dit doet, zullen je Joomla! (en alle andere lokale
website mappen) terechtkomen in de map "c:\xampp\htdocs". (Volgens afspraak gaat alle webinhoud in de "htdocs" map.)

Als je meerdere http-servers hebt (zoals IIS), kun je de luisterpoort van xampp wijzigen. In \apache\conf\httpd.conf, wijzig de regel Listen 80 naar Listen \[poortnummer\] (bijv: "Listen 8080").

Joomla Gemeenschapsmagazine Handleiding

Je kunt een gedetailleerde handleiding vinden over het installeren van XAMPP op Windows, samen met de Joomla 4 Beta, de Joomla Patch Tester en Git in dit <a href="https://magazine.joomla.org/all-issues/june-2020/github-installing-git" class="external text" target="_blank" rel="noreferrer noopener">Joomla Community Magazine artikel</a>.

### Installatie op Linux

#### Installeer XAMPP

Open Terminal en voer in:

```bash
    sudo tar xvfz xampp-linux-1.7.7.tar.gz -C /opt
```

(vervang *xampp-linux-1.7.7.tar.gz* met de versie van xampp die je hebt gedownload). Er is gemeld dat de MYSQL-database van xampp 1.7.4 niet werkt met Joomla 1.5.22.

Dit installeert ... Apache2, mysql en php5 evenals een ftp-server.

```bash
    sudo /opt/lampp/lampp start
```

en

```bash
    sudo /opt/lampp/lampp stop
```

start/stop alle diensten

#### Test uw XAMPP localhost-server

Open uw browser en navigeer naar

```bash
    http://localhost
```

De index.php zal omleiden naar

```bash
    http://localhost/xampp
```

Daar vind je instructies over hoe je standaard gebruikersnamen/wachtwoorden kunt wijzigen. Op een pc die geen bestanden naar het internet of LAN verzendt, is het wijzigen van de standaardinstellingen een persoonlijke keuze.

#### Joomla downloaden

Download de nieuwste Joomla-installatie zip
<a href="https://www.joomla.org/download.html"
class="external autonumber" target="_blank"
rel="noreferrer noopener">[1]</a>

Uitpakken naar je harde schijf

Verbind met localhost met een FTP-client Standaard

```
    nobody
    lampp
```

Maak een map aan voor je Joomla op de localhostserver.

FTP de uitgepakte Joomla-installatiebestanden naar de nieuw aangemaakte Joomla-map.

**Belangrijk:**

- De xammp-installatie stelt de juiste eigendom van de bestanden in en
  machtigingen.
- Het gebruik van de **CHOWN-opdracht** zal **eigendomproblemen met
  xampp veroorzaken**.
- **Het gebruik van nautilus** om mappen/bestanden op localhost te manipuleren zal
  **eigendomproblemen met xampp veroorzaken**.

**Database-informatie**

- Host: localhost
- Standaard Databasenaam: test
- Standaard Databasegebruiker: root
- Er is **geen** standaard Wachtwoord.

Het beheerderswachtwoord is naar keuze.

Het installeren van voorbeeldgegevens wordt aanbevolen voor de beginnende gebruiker.

Na installatie de installatiemap verwijderen en uw browser wijzen naar:

```
    http://localhost/yournewjoomlafolder
```

als

```
    http://localhost/yournewjoomlafolder/administrator
```

#### Een link maken in het Ubuntu-menu

**Om een GUI te maken voor xammp verbonden met je Ubuntu-menu**

Open de Terminal en typ

```bash
    sudo gedit /usr/share/applications/xampp-control-panel.desktop
```

Kopieer vervolgens het volgende in gedit en sla op.

```bash
[Desktop Entry]
Encoding=UTF-8
Name=XAMPP Control Panel
Comment=Start and Stop XAMPP
Exec=gksudo "python /opt/lampp/share/xampp-control-panel/xampp-control-panel.py"
Icon=/usr/share/icons/Tango/scalable/devices/network-wired.svg
Terminal=false
Type=Application
Categories=GNOME;Application;Network;
StartupNotify=true
```

Als het bedieningspaneel niet start, probeer dan het Exec-commando direct in de terminal uit te voeren:

```bash
    gksudo "python /opt/lampp/share/xampp-control-panel/xampp-control-panel.py"
```

Als je de foutmelding ontvangt:

```bash
    Error importing pygtk2 and pygtk2-libglade
```

Installeer de ontbrekende bibliotheken:

```bash
    sudo apt-get install python-glade2
```

#### XDebug PHP-debugger

Het XAMPP-pakket voor Linux bevat de XDebug PHP-debugger niet.  
Om XDebug op Debian of Ubuntu te installeren:

- Installeer het pakket *build-essential*:
```bash
    sudo apt-get update
    sudo apt-get install build-essential
    sudo apt-get install autoconf
```

- Download het
<a href="http://www.apachefriends.org/en/xampp-linux.html"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">ontwikkelingspakket</a> voor jouw
versie van XAMPP en pak het uit over je bestaande installatie:
```bash
    sudo tar xvfz xampp-linux-devel-1.7.7.tar.gz -C /opt 
```

- Bouw XDebug:
```bash
    wget http://xdebug.org/files/xdebug-2.1.3.tgz
    tar xzf xdebug-2.1.3.tgz
    cd xdebug-2.1.3/
    /opt/lampp/bin/phpize
```

Na dit krijg je de volgende uitvoer op je console…

```bash
    Configuring for:
    PHP Api Version:         20090626
    Zend Module Api No:      20090626
    Zend Extension Api No:   20090626 

    ./configure --with-php-config=/opt/lampp/bin/php-config
    make
    sudo make install 
```

Dan zal de output dit zijn... houd alsjeblieft het opgegeven directory in de gaten.

```bash
    Installing shared extensions:     /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/ 
```

Maak een map in je temp-map die het datumbestand zal bevatten
gegenereerd door XDebug:

```bash
    sudo mkdir /opt/lampp/tmp/xdebug
    sudo chmod a+rwx -R /opt/lampp/tmp/xdebug 
```

Alternatieve installaties:

Installeer met behulp van de PHP-extensies community-bibliotheek (PECL) gebundeld met xampp:

```bash
    sudo /opt/lampp/bin/pecl install xdebug
```

Op Ubuntu/Debian kun je installeren met:

```bash
    apt-get install php5-xdebug 
```

(waarschuwing: dit zal ook Apache en PHP installeren vanuit apt-repositories).

**Waarschuwing voor 64bit-gebruikers**

Bij het compileren van XDebug of het installeren via apt-get, ontvangt u een foutmelding bij het (opnieuw) starten van xampp:

```bash
    /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/xdebug.so: wrong ELF class: ELFCLASS64
```

Dit komt omdat xampp 32bit is, maar XDebug 64bit is. Om dit probleem te omzeilen, maak je xdebug.so op een 32bit machine of download je het van:

```bash
    http://code.activestate.com/komodo/remotedebugging/
```

Download het bestand: "PHP Remote Debugging Client" voor "Linux (x86)"
Pak de inhoud van het bestand uit op je computer, dit gecomprimeerde bestand
bevat meerdere mappen met versienummers, bijvoorbeeld: 4.4, 5.0, 5.1 ... 5.3
en verder, ga naar de map met het hoogste versienummer of degene die voor jou werkt, kopieer dan handmatig het bestand "xdebug.so" naar de
volgende locatie, overschrijf indien nodig.

```bash
    /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/
```

Vergeet niet dat deze locatie op uw computer anders kan zijn.

### Installatie op Mac OS X

Mac OS X bevat eigenlijk een Apache-server direct na de installatie, maar de meeste ontwikkelaars geven de voorkeur aan de geïntegreerde tools en configureerbaarheid die door XAMPP worden aangeboden.

Zoals bij de meeste programma's op Mac, is de installatie een fluitje van een cent. Bezoek <a href="https://www.apachefriends.org/en/download.html" class="external text" target="_blank" rel="nofollow noreferrer noopener">Apache Friends - Mac OS X</a> voor de universele binaire download.

Zodra het bestand klaar is met downloaden, open je de schijfkopie en sleep je de XAMPP-map naar het alias van de map "Programma's".

Om de server te starten, open "XAMPP Control.app" en druk op de startknop naast Apache.

##### Een beetje probleemoplossing

Veel Mac-gebruikers hebben wat moeite in dit stadium wanneer ze proberen een andere instantie van Apache op hun machine in te stellen. Als je XAMPP's Apache niet kunt starten, heb je twee opties:  
**Je kunt de luisterpoort van XAMPP wijzigen.** In \Applications\XAMPP\xamppfiles\etc\httpd.conf, wijzig de regel die zegt, "Listen 80" naar Listen \[poortNummer\]. Bijvoorbeeld:

```bash
    Listen 8080
```

**Je kunt de luisterpoort van de vooraf geïnstalleerde Apache-server wijzigen.** Ga in Finder naar "/etc" (CMD+SHIFT+G); vanaf hier kun je door de normaal verborge Apache-bestanden navigeren. Zoek de map met de naam Apache2 en bewerk het "http.conf" bestand. Wijzig de regel die zegt "Listen 80" naar Listen \[poortNummer\]. Bijv.:

```bash
    Listen 8080
```

*Opmerking: Als je ervoor kiest om de poort van de voorgeïnstalleerde Apache-server te wijzigen, moet je mogelijk je computer opnieuw starten om de wijzigingen door te voeren. Je zult ook als beheerder moeten verifiëren om deze bestanden te wijzigen.*

### Test XAMPP-installatie

Zodra XAMPP is geïnstalleerd en je de Apache-service hebt gestart met het XAMPP Configuratiescherm-hulpmiddel, kun je het testen door je browser te openen en te navigeren naar "<a href="http://localhost" class="external free" target="_blank" rel="nofollow noreferrer noopener">http://localhost</a>". Je zou het welkomstscherm van XAMPP moeten zien dat lijkt op dat hieronder.

<img
src="https://docs.joomla.org/images/thumb/f/fc/Phpinfo_on_xampp.png/800px-Phpinfo_on_xampp.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/f/fc/Phpinfo_on_xampp.png/1200px-Phpinfo_on_xampp.png 1.5x, https://docs.joomla.org/images/f/fc/Phpinfo_on_xampp.png 2x"
data-file-width="1498" data-file-height="883" width="800" height="472"
alt="Phpinfo op xampp.png" />

Selecteer de link genaamd "phpinfo()" in het bovenste menu. Dit toont een lange scherm met informatie over de PHP-configuratie, zoals hieronder weergegeven.

<img
src="https://docs.joomla.org/images/thumb/d/db/Phpinfo.png/800px-Phpinfo.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/d/db/Phpinfo.png/1200px-Phpinfo.png 1.5x, https://docs.joomla.org/images/d/db/Phpinfo.png 2x"
data-file-width="1432" data-file-height="1282" width="800" height="716"
alt="Phpinfo.png" />

Op dit punt is XAMPP succesvol geïnstalleerd. Let op het "Loaded Configuration File". We zullen dit bestand in de volgende sectie bewerken om XDebug te configureren.

*Vertaald door openai.com*

