<!-- Filename: Setting_up_Eclipse_PDT_2020_and_Git_for_Pulls / Display title: Eclipse PDT 2020 en Git instellen voor Pull-requests -->

## Inleiding

Ik begon vele jaren geleden voor het eerst met het gebruik van Eclipse voor een privé Joomla-project en een privé remote git-repository die niet op Github stond. Ik las de beschikbare tutorials en rommelde gelukkig door totdat ik dit jaar verder ging om te proberen te helpen met core testing en het oplossen van bugs voor Joomla! 4. En daarna een aantal pull requests. Toen besefte ik dat ik niet precies begreep wat ik aan het doen was. Uiteindelijk besloot ik opnieuw te beginnen en het proces te documenteren voor andere semi-ervaren ontwikkelaars die nieuw zijn bij Joomla, vooral de onderdelen die ik de eerste keer niet volledig waardeerde. Eerst op de lijst:

## Bestandsstructuur

Het is niet vanzelfsprekend voordat je Eclipse downloadt en installeert dat er minstens twee plaatsen zijn waar gegevens worden opgeslagen, naast waar de Eclipse-code zich bevindt.

### De Werkruimte

Dit is waar Eclipse zijn eigen gegevens voor elk project opslaat. Het zou niet in de webboom moeten staan! Aangezien ik voornamelijk op een Mac-laptop werk, is de standaardlocatie voor mij **/Users/username/eclipse-workspace**. Er is een submap voor elk project. Dus een zou kunnen zijn **/Users/username/eclipse-workspace/joomla-cms**. Het bevat één map: .metadata. Linux-gebruikers zullen waarschijnlijk weten dat ze home moeten vervangen door Users.

### De lokale git-repository en testwebsite

Dit is waar projectcode wordt opgeslagen. Het kan in de lokale webboom staan als je het voor testen wilt gebruiken. Voor mij is het **/Users/username/Sites/joomla-cms-4**. Linux-gebruikers zullen waarschijnlijk public_html in de plaats van Sites gebruiken. Let op de extra stappen die nodig zijn om de lokale repository als een Joomla-site te laten werken.

### Een aparte testwebsite

Dit is optioneel maar vereist dat de git-repository een aangepast build.xml-bestand heeft, bijvoorbeeld build-local.xml, dat in de bijlage wordt behandeld.

## Vereiste Software

Om een werkende ontwikkelings- en testwebsite in te stellen, moet u eerst de volgende software installeren:

- Apache
- MySQL of Maridb
- PHP
- phpMyAdmin - [PhpMyAdmin startpagina](https://www.phpmyadmin.net/)
- Composer - [Composer startpagina](https://getcomposer.org/)
- Node.js - [Node.js startpagina](https://nodejs.org/en/)
- Eclipse PDT - [Eclipse PHP ontwikkeltools](https://www.eclipse.org/pdt/)
- git (optioneel voor command line git-gebruikers) - [git startpagina](https://git-scm.com/)
- phing (optioneel voor aangepaste builds) - [phing startpagina](https://www.phing.info/)

De eerste drie of vier komen vaak als een bundel voor jouw platform, bekend als een LAMP-, WAMP- of XAMP-stack. Gebruik gewoon wat jouw platform biedt of kijk naar Apache Friends [XAMPP](https://www.apachefriends.org/)

Laten we aannemen dat je alles hebt geïnstalleerd behalve Eclipse.

## Fork joomla-cms op Github

Er is een workflow beschreven in [Mijn eerste pull request naar Joomla! op Github](https://docs.joomla.org/My_first_pull_request_to_Joomla!_on_Github "Special:MyLanguage/My first pull request to Joomla! on Github") die ik niet genoeg kan prijzen. Het laat precies zien wat je moet doen:

![Github work flow](../../../en/images/getting-started/core-work-flow-joomla.png)

Stappen

- Ga naar [Github](https://github.com/) en maak een account aan, gratis en snel.
- Ga in je Github-account naar de joomla/joomla-cms repo: typ joomla/joo... in het zoekvak linksboven en selecteer wanneer opties verschijnen.
- Klik in de joomla/joomla-cms repo op de fork-knop rechts bovenaan. Dat geeft je een geforkte kopie van alle code voor Joomla 4, Joomla 5, ..., alles, in je eigen Github-account.

Terug naar je werkstation.

## Eclipse installeren

Ik heb al twee versies van Eclipse geïnstalleerd, Oxygen van een paar jaar geleden, en Cocoa van 2020. Vanaf hier installeer ik een tweede instantie van Cocoa. Laten we kijken wat er gebeurt:

- Ga naar de [Eclipse site](https://www.eclipse.org/pdt/) en download de versie voor jouw platform.
- Volg de installatieprocedure en start uiteindelijk de Eclipse-applicatie, voor mij **Eclipse 2.app**.
- Waarschuwing: *Eclipse 2.app* is een app die van het internet is gedownload. Weet je zeker dat je het wilt openen? **Openen**
- Selecteer een map als werkruimte - Standaard is /Users/username/eclipse-workspace. **Bladeren** en maak een joomla-cms-4 submap aan.
- Start: Eclipse is geïnstalleerd en toont de welkomstpagina.
- Bekijk de configuratie-instellingen van de IDE. Stel items 1, 2, 5 en 6 in.
- Selecteer het Workbench-icoon rechtsboven.

In plaats van een andere versie van Eclipse te installeren, had ik een nieuwe lege workspace kunnen openen.

Tot nu toe gaat het goed!

## Fork importeren in Eclipse

In je nieuwe lokale installatie van Eclipse:

- Open Voorkeuren en stel Team / Git / Standaardmap voor repository in op /Users/username/git (je kunt die locatie wel of niet gebruiken)
- Open Bestand / Importeren / Git / Projecten uit Git (met slimme import). Volgende ...
- Kloon URI. Volgende ...
- Kopieer de URL-balk van **je Github-fork** en plak deze in het URL-veld. Je hoeft geen authenticatiegegevens toe te voegen. Volgende ...
- Te importeren branches ... eh ... Waarschijnlijk het beste om alles te importeren. Volgende ...
- Map. Hier kun je een andere locatie kiezen om de code te bewaren. Ik bladerde naar /Users/username/public_html/joomla-cms-4 en maakte deze indien nodig aan.
- Initiële branch - als je alle branches hebt geïmporteerd. Ik werk aan Joomla 4 dus ik selecteerde 4.0-dev. En ik merkte op dat mijn kloon **origin** zal zijn. Volgende ...
- Slokje koffie. Klonen duurt een paar minuten.
- Bron importeren. De bron moet de codelocatie zijn. Voltooien.
- Eclipse toont nu de wortel van de codeboom in het Project Explorer-paneel. Klik om meer van de boom te zien. Let op dat deze code nog niet als een Joomla-site zal werken. Onder andere is er geen mediabestand.

## Voeg originele joomla-cms repository toe aan Eclipse

- Toon het Git Repositories-venster: selecteer Venster / Toon Weergave / Overige
- Selecteer Git / Git Repositories - het venster verschijnt onderaan het scherm.
- Vouw joomla-cms / remotes / origin uit - als je wijzigingen aan je code aanbrengt en naar origin pusht, is dit waar het naartoe gaat.
- Klik met de rechtermuisknop op Remotes en selecteer Remote maken...
- Stel de Remote naam in op **joomla-origin** en selecteer **Configureren ophalen**.
- In Configureren Ophalen selecteer **Wijzigen**.
- In Selecteer een URI plak je de url van de originele joomla-cms repository: `https://github.com/joomla/`
- Laat Aanmeldingsgegevens leeg. Voltooien.
- in Configureren Ophalen: **Opslaan en Ophalen**.

## Maak een werkende site

Je kopie van de joomla-cms code heeft meer stappen nodig om deze bruikbaar te maken als een website.

- Open een terminal en ga naar de map met je gekloonde code.
- Voer composer install uit:
  - Linux- en OSX-gebruikers kunnen de volgende bash-alias instellen door het volgende in het bestand `~/.bash_profile` of `~/.zsh` te plaatsen (`\$ source ~/.bash_profile` om direct van kracht te worden):
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
    alias jinstall="jclean; composer install --ignore-platform-reqs; npm ci"
```

- composer staat niet in mijn pad, dus ik verving het door php ~/composer/composer.phar
- jinstall
- dat haalt alle Joomla PHP-afhankelijkheden op, evenals Javascript-afhankelijkheden, compileert alle ES6 Javascript en plaatst de bestanden op hun juiste locaties.
- slok koffie terwijl de afhankelijkheden worden gedownload en mediabestanden worden aangemaakt.
- Klik in Eclipse met de rechtermuisknop op de projectroot en selecteer Vernieuwen. Je zult zien dat je code nu een mediamap heeft.
- Als je geïnteresseerd bent, gebruik dan een bestandsbeheerder om te zien dat de root ook een bestand met de naam .gitignore bevat en de mediamap daarin genoemd wordt. Dit betekent dat het niet wordt gecommit naar je lokale of externe geforkte git-repositories.

Je bent nu klaar voor een Joomla-installatie:

- Maak een database aan met behulp van phpMyAdmin (of de mysql opdrachtregel als je dat liever hebt).
- Ik heb een database gemaakt met de naam joomla-cms-4 met utf8mb4-unicode-ci collatie.
- Maak een nieuwe gebruiker aan: de mijne is jcms4 met een gegenereerd willekeurig wachtwoord (GAOC26r77bBLkkdA). Je moet het wachtwoord noteren voor gebruik tijdens de installatie. Het komt als platte tekst in het configuratiebestand terecht.
- Verleen alle rechten op de database aan deze gebruiker - de standaardinstelling.
- Ga in je favoriete browser naar de root van de nieuwe site: localhost/joomla-cms-4/

### Op mijn Mac met macOS Catalina

- Omgevingsinstallatie incompleet: Meer details. Bah - gewoon oplossen en controleren of de url-balk de url bevat die je hebt getypt - de mijne had op de een of andere manier extra tekens toegevoegd.
- Anders gaat het naar een werkende site - verwijder de installatie-map niet.

### Op mijn Linux-werkstation met Linux Mint 20

Oeps! Er is iets misgegaan!

De keerzijde is dat ik mijn Joomla-code in mijn persoonlijke bestandsruimte heb voor lees-/schrijftoegang door Eclipse. De webserver heeft ook schrijfrechten nodig om configuratie-, cache- en logbestanden te schrijven, maar deze draait als een gebruiker en groep met lage privileges. Aangezien ik op een particulier thuisnetwerk zit, heb ik /etc/apache2/apache2.conf bewerkt om User \${APACHE_RUN_USER} en Group \${APACHE_RUN_GROUP} uit te schakelen en User myusername en Group mygroupname toe te voegen. Start apache opnieuw, dan...

- Het gaat naar een werkende site - verwijder de installatiemap niet.

## Code Revisie

In het kort:

- Ophalen van joomla-origin om ervoor te zorgen dat mijn lokale kloon up-to-date is.
- Team / Samenvoegen / Selecteer tak om samen te voegen - voorzichtig - ik selecteerde joomla-origin/4.0-dev
- Push naar origin om ervoor te zorgen dat mijn remote fork up-to-date is.
- Maak een tak aan voor een aantal codewijzigingen die ik wil maken.
- Voer de codewijzigingen uit
- Als de wijzigingen css- of js-bronbestanden betreffen (deze in sass of es6), ga dan naar een terminalvenster en voer opnieuw jinstall uit.
- TEST je lokale installatie om te zien of er problemen zijn.
- Controleer de codewijzigingen
- Push naar origin om mijn remote fork met mijn wijzigingen bij te werken.
- Ga naar je eigen account op Github en selecteer de tak die je hebt aangemaakt met de bijgewerkte code. Iets engs: er staat **Deze tak is 11084 commits voor, 134 commits achter joomla:staging.** Heb ik iets fout gedaan? Blijkbaar niet!
- Selecteer de Pull Request knop. Zorg ervoor dat je de juiste joomla-tak selecteert om in samen te voegen. Voor mij is dit 4.0-dev. En zorg ervoor dat je je tak met de gewijzigde code hebt geselecteerd. Doen!
- De pull request moet getest en goedgekeurd worden en kan dagen, weken of maanden duren. En de wijziging kan afgewezen worden!

## Ondertussen

Terug op je werkstation:

- Schakel terug naar je originele branch, voor mij: Team / Wissel naar / 4.0-dev
- Herbouwen: voor mij Project / Bouw Project
- Bekijk de eerder gewijzigde bestanden die opnieuw naar je werkplek worden gekopieerd.
- TEST: je werkplek is terug naar de situatie zoals het was voordat je codewijzigingen aanbrachte.

Je bent nu klaar om een andere branch te maken voor een andere reeks codewijzigingen.

## Als Ramp Toeslaat

Op een gegeven moment raakte mijn lokale kloon op de een of andere manier beschadigd en ik had geen idee hoe ik het moest oplossen. Dus verwijderde ik mijn lokale kloon en alle bijbehorende bestanden, leegde de database en ging toen terug naar de hierboven beschreven stap **Fork importeren in Eclipse**. Dat bracht mijn lokale kloon in sync met mijn remote fork, inclusief alle branches die ik had gemaakt voor pull requests. De nieuwe installatie werkte zonder problemen en ik was blij!

## Andere Bronnen

- [Eclipse configureren voor Joomla-ontwikkeling](https://docs.joomla.org/Configuring_Eclipse_for_joomla_development "Special:MyLanguage/Configuring Eclipse for joomla development") (2012-2013) Maar krijg de nieuwste Eclipse PDT-versie!
- [Mijn eerste pull request naar Joomla! op Github](https://docs.joomla.org/My_first_pull_request_to_Joomla!_on_Github "Special:MyLanguage/My first pull request to Joomla! on Github") (2011-2014) Goed overzicht, hoewel een beetje verouderd.
- [Werken met git en github](https://docs.joomla.org/Working_with_git_and_github "Special:MyLanguage/Working with git and github") (2011-2015)
- [Je lokale omgeving instellen](https://docs.joomla.org/J4.x:Setting_Up_Your_Local_Environment "Special:MyLanguage/J4.x:Setting Up Your Local Environment") (2018-2020)
- [Eclipse en Xdebug configureren](https://docs.joomla.org/Configuring_Eclipse_and_Xdebug "Special:MyLanguage/Configuring Eclipse and Xdebug") (2013) Alles over debuggen.
- [Werken met Git en Eclipse](https://docs.joomla.org/Working_with_Git_and_Eclipse "Special:MyLanguage/Working with Git and Eclipse") (2014) Methode voor oude edities van Eclipse.
- [Geautomatiseerde tests uitvoeren vanuit Eclipse](https://docs.joomla.org/Running_Automated_Tests_from_Eclipse "Special:MyLanguage/Running Automated Tests from Eclipse") (2020) Voor gevorderde gebruikers?
- [Xdebug configureren voor PHP-ontwikkeling/Linux](https://docs.joomla.org/Configuring_Xdebug_for_PHP_development/Linux "Special:MyLanguage/Configuring Xdebug for PHP development/Linux") (2016) Installeren en configureren.
- [Eclipse IDE configureren voor PHP-ontwikkeling/Linux](https://docs.joomla.org/Configuring_Eclipse_IDE_for_PHP_development/Linux "Special:MyLanguage/Configuring Eclipse IDE for PHP development/Linux") (2019) Installeren en configureren voor Linux.
- [Xdebug configureren voor PHP-ontwikkeling](https://docs.joomla.org/Configuring_Xdebug_for_PHP_development "Special:MyLanguage/Configuring Xdebug for PHP development") (2014) Stappen voor Linux, Windows en Mac OS X.
- [Webinar: Eclipse gebruiken voor Joomla! Ontwikkeling](https://docs.joomla.org/Webinar:_Using_Eclipse_for_Joomla!_Development "Special:MyLanguage/Webinar: Using Eclipse for Joomla! Development") (2014) Dit 45-minuten durende video-webinar, opgenomen op 30 april 2009, geeft een overzicht van Eclipse-functies voor Joomla! ontwikkeling.
- [Je werkstation instellen voor Joomla-ontwikkeling](https://docs.joomla.org/Setting_up_your_workstation_for_Joomla_development "Special:MyLanguage/Setting up your workstation for Joomla development") (2020) Kort overzicht van vereiste software en alternatieve IDEs.

## Bijlage

Op een gegeven moment had ik mijn lokale git-repository buiten mijn webroot en moest ik eventuele wijzigingen die ik maakte kopiëren naar een apart geïnstalleerde lokale site. Dat vereiste een aangepast buildbestand, hier ter referentie getoond:

### Maak een build-local.xml-bestand aan

De Eclipse-kloon bevat een build.xml-bestand, maar dit wordt gebruikt om een te downloaden zip-bestand voor een nieuwe installatie te testen en te bouwen. Wat ik wil doen, is alle wijzigingen die ik aan mijn gekloonde code aanbreng kopiëren naar mijn testsite op mijn laptop. Let op dat ik alleen wijzigingen wil aanbrengen in PHP-code en niet in Javascript of CSS. Om dit te doen, heb ik een apart bestand gemaakt genaamd build-local.xml in de hoofdmap van het project:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="joomla-cms" basedir="." default="main">
    <property file=".project" />

    <property name="joomladir" value="/Users/username/public_html/joomla-cms"  override="true" />

    <property name="srcdir" value="${project.basedir}" override="true" />

    <!-- Fileset for all files -->
    <fileset dir="${srcdir}" id="allfiles">
        <include name="administrator/**" />
        <include name="api/**" />
        <include name="cli/**" />
        <include name="components/**" />
        <include name="images/**" />
        <include name="includes/**" />
        <include name="language/**" />
        <include name="layouts/**" />
        <include name="libraries/**" />
        <include name="modules/**" />
        <include name="plugins/**" />
        <include name="templates/**" />
        <include name="index.php" />

        <exclude name="**/.*" />
    </fileset>

    <!-- ============================================  -->
    <!-- (DEFAULT) Target: main                        -->
    <!-- ============================================  -->
    <target name="main" description="main target">
        <copy todir="${joomladir}">
            <fileset refid="allfiles" />
        </copy>
    </target>
</project>
```

### Voeg een build-tool toe

- Klik met de rechtermuisknop op de projectroot en selecteer Eigenschappen.
- Selecteer Builders / Nieuw / Programma / OK.
- Naam: Lokale build met phing
- Locatie: waar phing is geïnstalleerd. Voor mij is dat /usr/local/php5/bin/phing, hoewel ik PHP 7.4 gebruik.
- Werkmap: Blader door Werkruimte en selecteer joomla-cms - komt uit als \${workspace_loc:/joomla-cms}
- Argumenten: -f build-local.xml
- Toepassen / OK
- In Builders: Toepassen en Sluiten
- Project / Bouw Project
- Zie wat er gebeurt:

```sh
    Buildfile: /Users/username/git/joomla-cms/build-local.xml
     [property] Loading /Users/username/git/joomla-cms/.project

    joomla-cms > main:

    BUILD FINISHED

    Total time: 0.2121 seconds
```

*Vertaald door openai.com*

