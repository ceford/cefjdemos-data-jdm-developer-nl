<!-- Filename: Visual_Studio_Code_Primer / Display title: Visual Studio Code -->

## VS Code - Een Populaire Gratis IDE

Van [Wikipedia](https://en.wikipedia.org/wiki/Visual_Studio_Code):

> Visual Studio Code, ook wel aangeduid als VS Code, is een broncode-editor gemaakt door Microsoft voor Windows, Linux en macOS. Kenmerken zijn onder meer ondersteuning voor debugging, syntaxisaccentuering, intelligente code-aanvulling, snippets, code-herstructurering en geïntegreerde Git. Gebruikers kunnen het thema, de sneltoetsen, voorkeuren wijzigen en extensies installeren die extra functionaliteit toevoegen.

## Installatie

De standaardpagina van de [VS Code](https://code.visualstudio.com/) site heeft een keuzelijst voor elk ondersteund platform. De kans is groot dat jouw platform vooraf is geselecteerd. Dus download en installeer en je bent klaar om te beginnen.

### Aan de slag

De VS Code *Aan de slag* pagina heeft enkele *Start* items, een lijst met *Recente* items, en een korte lijst met *Rondleidingen*. Als je helemaal nieuw bent met VS Code, worden deze aanbevolen om te bekijken. Ze kosten maar een paar minuten.

De VS Code-documentatie is beschikbaar via het menu *Help / Documentation*. De Introductievideo's zijn zeker de moeite waard om te bekijken. Elk duurt 2 tot 6 minuten en geeft een uitstekende introductie tot de functies van VS Code:

[VS Code Video's](https://code.visualstudio.com/docs/getstarted/introvideos)

De officiële documentatie is de plek om naartoe te gaan als je specifieke informatie wilt opzoeken.

### VS Code-extensies

VS-code kan voor elk type tekst worden gebruikt, inclusief een breed scala aan programmeertalen. Het werkt met JavaScript zonder dat er uitbreidingen hoeven te worden toegevoegd. Andere talen worden gedetecteerd door de context, dus als je begint met het maken en opslaan van PHP-code, krijg je waarschijnlijk de melding om een PHP-ondersteuningspakket te installeren.

Klik op het icoon *Uitbreidingen* in de linker *Activiteitenbalk* om te zien wat je hebt geïnstalleerd en wat wordt aanbevolen. Je hebt de PHP Debug-uitbreiding nodig!

### De VS Code Schermindeling

Sommige termen die in de volgende instructies worden gebruikt:

- **Activiteitenbalk:** de smalle balk aan de linkerkant van het scherm. Selecteer een pictogram om de Primaire Zijbalk te openen of te sluiten.
- **Primaire Zijbalk:** toont details van de geselecteerde activiteit wanneer deze open is.
- **Statusbalk:** onderaan het scherm. Het toont wat er gaande is.
- **Paneel:** een gebied onder de teksteditoren om andere informatie weer te geven.

Selecteer een lay-outpictogram rechtsboven om een van deze items te openen of te sluiten.

## Een Joomla-extensie coderen

Om een extensie te maken is je doel om een zip-bestand te maken dat je kunt installeren op een werkende Joomla-site. Dus je hebt een map nodig om je code in op te slaan. Dit zou binnen je persoonlijke bestandsruimte op je laptop of desktopcomputer moeten zijn, die je gebruikt voor lokale ontwikkeling. Het zou niet in je website-structuur moeten staan. Bijvoorbeeld, je zou *~/jextensions* kunnen gebruiken om submappen voor verschillende extensies te bevatten. Ik gebruik *~/git* omdat het kort en makkelijk te spellen is, hoewel het potentieel verwarrend kan zijn omdat elke submap een aparte git-repository gebruikt.

### Voorbeeldcode

Als je wat voorbeeldcode wilt om mee te werken, is er een extensie beschikbaar op GitHub genaamd *mod_debugme*. Zoals de naam al aangeeft, is het een module met enkele bugs. Naast de modulecode is er een *build.xml*-bestand om een manier te illustreren om het bouwen voor testen en het maken van een zip-bestand te automatiseren.

De module is ontworpen om de volgende paar (standaard 3) evenementen (verjaardagen) te tonen uit een lijst die is opgeslagen in een databasetabel. Je kunt je voorstellen dat dit wordt gebruikt op een kantoor- of familiesite in de verwachting van taart.

Het kan het beste zijn om te beginnen met het gebruiken van git-opdrachten vanuit de opdrachtregel. Maak eerst een map voor je code en kloon dan de externe repository:

```sh
    mkdir ~/git
    cd ~/git
    git clone https://github.com/ceford/j4xdemos-mod-debugme
```

Het antwoord zou slechts enkele seconden moeten duren.

```sh
    Cloning into 'j4xdemos-mod-debugme'...
    remote: Enumerating objects: 23, done.
    remote: Counting objects: 100% (23/23), done.
    remote: Compressing objects: 100% (16/16), done.
    remote: Total 23 (delta 3), reused 23 (delta 3), pack-reused 0
    Unpacking objects: 100% (23/23), done.
```

Je zou even de tijd moeten nemen om naar de inhoud van de map te kijken:

```sh
    cd j4xdemos-mod-debugme
    ls -al
    total 16
    drwxr-xr-x   6 ceford  staff   192  2 Sep 17:48 .
    drwxr-xr-x   3 ceford  staff    96  2 Sep 17:48 ..
    drwxr-xr-x  12 ceford  staff   384  2 Sep 17:48 .git
    -rw-r--r--   1 ceford  staff  1402  2 Sep 17:48 README.md
    -rw-r--r--   1 ceford  staff   927  2 Sep 17:48 build.xml
    drwxr-xr-x   8 ceford  staff   256  2 Sep 17:48 mod_debugme
```

De *.git* map bevat informatie over de repo. Het *README.md* bestand is een markdown-document dat deze repo beschrijft. Het *build.xml* bestand is een bestand dat wordt gebruikt om de extensie te bouwen met een extern hulpmiddel, Phing - later beschreven. De *mod_debugme* map bevat de code van de extensie.

### Installeren in Joomla

Comprimeer de extensiemap om een installeerbaar zip-bestand te maken:

```sh
    zip -r mod_debugme.zip mod_debugme
```

Je kunt nu het zip-bestand installeren op de Joomla-site die je voor testen gebruikt. Na installatie moet je een Sitemodule maken en deze toewijzen aan een modulepositie. Aangezien het een gebroken module is, kun je deze toewijzen aan een positie op *Alle pagina's* terwijl je eraan werkt; of je kunt deze toewijzen aan een positie op een enkele pagina; of je kunt het plaatsen in een artikel dat een eigen menu-item heeft.

Na installatie, verwijder het zipbestand.

### Schakel Debug Modus in

In de Globale Configuratie van Joomla, stel *Debug Systeem* in op *Ja* en *Foutopsporing Rapportage* op *Maximum*.

Wanneer je een pagina opent die de buggevoelige module bevat, zie je een stack trace die je vertelt waar een fout is veroorzaakt.

![Stack trace](../../../en/images/getting-started/vscode-primer-stack-trace.png)

Soms bevindt de codeerfout zich op de eerste regel van de stacktrace. Anders, als de fout wordt geactiveerd in bibliotheekcode, bijvoorbeeld door ongeldige gegevens door te geven aan een databasefunctie, kan de codeerfout verder naar beneden in de lijst van functieroepen staan.

## Open Uitbreidingsmap in VS Code

In VS Code, gebruik de menu-optie Bestand / Map openen om de map te vinden en te openen die je lokale kopie van de *mod_debugme* extensiecode bevat. Je zou iets dergelijks moeten zien:

![VS Code screen](../../../en/images/getting-started/vscode-primer-screen.png)

Je kunt het probleem mogelijk diagnosticeren door gewoon de code te lezen. In het geval van de fout *Class "DebugHelper" not found* zul je zien dat een *use* statement een paar regels eerder is uitgecommentarieerd. Het vergeten om een *use* statement in te voegen is een veelvoorkomende fout tijdens de initiële ontwikkeling!

Los dat probleem op en comprimeer en installeer de module vervolgens opnieuw. Die stap wordt een beetje vervelend wanneer je meerdere problemen hebt. Daar komen bouwtools van pas.

## De Phing Build Tool

Phing is een opdrachtregelprogramma, beschikbaar voor alle platforms, dat wordt gebruikt om softwarepakketten te bouwen met behulp van instructies die zijn opgeslagen in een xml-bestand, standaard genaamd build.xml. Voor het werken met extensiecode kan het voor twee dingen worden gebruikt:

- kopieer gewijzigde bestanden van je extensiebronmap naar de juiste locaties in je website-map.
- genereer een nieuw zip-bestand voor nieuwe installaties.

Download en installeer [Phing](https://www.phing.info/). Andere build-tools zijn beschikbaar! Je kunt Phing installeren in je eigen bin-map of in een systeem bin-map. Je moet het pad naar je Phing-code noteren. In dit voorbeeld is het *~/bin/phing-latest.phar*. Je kunt het uitproberen vanaf de commandoregel nadat je bent overgeschakeld naar de map die je extensiecode bevat:

```sh
    cd ~/git/j4xdemos-mod-debugme
    php ~/bin/phing-latest.phar
```

Antwoord:

```sh
    Buildfile: /Users/ceford/git/j4xdemos-mod-debugme/build.xml

    mod_debugme > main:
          ... Any copied files will be mentioned here
          [zip] Building zip: /Users/ceford/zips/mod_debugme.zip

    BUILD FINISHED

    Total time: 0.0863 seconds
```

## VS Code Taken

Om Phing vanuit VS Code te draaien, moet je een *tasks.json*-bestand aanmaken in de *.vscode*-map in de root van de map *j4xdemos-mod-debugme*. Als deze laatste niet bestaat, maak deze dan eerst aan. Maak vervolgens het *tasks.json*-bestand en voer de volgende code in:

```json
    {
        // See https://go.microsoft.com/fwlink/?LinkId=733558
        // for the documentation about the tasks.json format
        "version": "2.0.0",
        "tasks": [
          {
            "label": "Build mod_debugme",
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

Windows-gebruikers moeten de Windows-specifieke opdracht corrigeren. Je kunt nu de extensie bouwen via het menu *Terminal / Run Build Task*. Het resultaat van de opdracht zou in het Terminalpaneel onder het bewerkgebied moeten verschijnen.

```sh
      *  Executing task: php ~/bin/phing-latest.phar

    Buildfile: /Users/ceford/git/gitdemo/j4xdemos-mod-debugme/build.xml

    mod_debugme > main:

          [zip] Nothing to do: /Users/ceford/zips/mod_debugme.zip is up to date.

    BUILD FINISHED

    Total time: 0.1031 seconds

     *  Terminal will be reused by tasks, press any key to close it.
```

Er kunnen onbegrijpelijke foutmeldingen zijn. De meest waarschijnlijke oorzaak is het hebben van ongeldige paden naar mappen in het *build.xml* bestand of een map is niet aangemaakt. Weer een ander soort probleem om te debuggen!

## Fouten opsporen

Je zou de eerste bug moeten kunnen oplossen door de code te inspecteren. Meer gecompliceerde problemen vereisen het doorlopen van de code met de debugger. Dat stelt je in staat om variabelen te inspecteren om te zien of ze de waarden bevatten die je verwacht, bijvoorbeeld wanneer je argumenten aan bibliotheekfuncties doorgeeft.

### *php.ini* Instellingen

Om debugging met Xdebug op te zetten, moet je enkele vermeldingen aan het begin van je *php.ini* bestand maken.

```sh
    zend_extension="xdebug.so"
    xdebug.mode="debug"
    xdebug.client_port=9003
    xdebug.start_with_request=yes
    xdebug.log_level=0
```

Nadat u wijzigingen hebt opgeslagen, start u uw Apache-server opnieuw op.

### Venster Website Toevoegen

Je extensiemap bevat slechts een paar bestanden, de ***bronnen*** van de bestanden die op je website zijn geïnstalleerd. Runtime-debuggen houdt in dat je breekpunten instelt in je ***site***-bestanden, dus je hebt toegang tot die bestanden nodig. Je kunt het *Bestand / Map toevoegen aan Werkruimte...* menu gebruiken om de sitemap aan je werkruimte toe te voegen. Er is echter een grote kans dat je uiteindelijk wijzigingen aanbrengt in sitebestanden in plaats van bronbestanden. Daarom is het waarschijnlijk het beste om een apart VS Code-venster te openen voor het debuggen.

- **Nieuw venster openen:** Kies in het VS Code-menu *Bestand / Nieuw Venster* en selecteer de map die je Joomla-website bevat.
- **Map openen:** Kies in het nieuw geopende venster *Bestand / Map openen...* in het VS Code-menu. Zoek je website-map en selecteer deze. Je zou een lijst van alle bestanden in je Joomla-website in de primaire zijbalk moeten zien.

### Startconfiguratie

Om debuggen daadwerkelijk te laten werken in VS Code, heb je een startconfiguratie nodig. Maak in de root van je website een map genaamd *.vscode* (let op het voorvoegsel stop) met daarin een bestand genaamd *launch.json* met de volgende inhoud:

```json
    {
        "configurations": [
            {
                "name": "Listen for XDebug",
                "type": "php",
                "request": "launch",
                "hostname": "0.0.0.0",
                "port": 9003,
                "stopOnEntry": true,
                "pathMappings": {
                    "/Users/ceford/Sites/j421rc2": "${workspaceFolder}"
                }
            }
        ]
    }
```

Vergeet niet om het item pathMappings in dit voorbeeld te vervangen door de daadwerkelijke pathMappings op uw eigen site. Het item stopOnEntry zal de uitvoering op de allereerste regel van de uitgevoerde PHP-code pauzeren.

### Foutopsporing *mod_debugme*

Nu ben je klaar om de bugs in de geïnstalleerde module te vinden en op te lossen.

- **Modulecode vinden:** Zoek de eerste bug op regel 16 van JROOT/modules/mod_debugme/mod_debugme.php.
- **Breakpoint instellen:** Klik in de ruimte links van het nummer 16. Een lichtrode blob verschijnt als u erover zweeft en wordt volledig rood na klikken om aan te geven dat er een breakpoint is ingesteld.
- **Start debug:** Selecteer *Uitvoeren / Start Debuggen* in het VS Code-menu. Herlaad uw site in uw browser. Uw VS Code-venster zou opnieuw moeten verschijnen met de code gestopt op de eerste regel van het site *index.php* bestand. Bovenaan het scherm zijn enkele pictogrammen om het debugproces te bedienen. Ze zouden voor zich moeten spreken. Zo niet, zoek ze dan op in de VS Code Help / Documentatie.
- **Doorgaan:** Selecteer de doorgaan-knop - de code zal doorgaan naar uw eerste breakpoint. Onderzoek de code om te zien wat het probleem is.
- **Hover:** Als u over een variabele zweeft die een waarde heeft gekregen, verschijnt er een kleine Tooltip die de attributen van die variabele samenvat. Er is geen Tooltip voor variabelen die geen waarde hebben gekregen.
- **Variabelen:** De linkerkolom bevat meer informatie over de status van de code bij het breakpoint. Er zijn te veel om hier te behandelen. Verken ze indien nodig!
- **Debuggen stoppen:** Het is waarschijnlijk het beste om het Doorgaan-pictogram te selecteren, anders wordt de webpagina blanco geleverd. Anders kunt u de Stop-knop of het menu Uitvoeren / Debuggen stoppen gebruiken.

### Los een bug op

**Vergeet niet:** Los de bug niet op in de websitecode! Los het op in de broncode!

Herstel de broncode en gebruik vervolgens *Terminal / Taak uitvoeren...*.

Herstart foutopsporing.

### Tips

Een paar niet zo voor de hand liggende problemen:

- Je lost de eerste bug op, maar het crasht nog steeds op die regel. Kijk in mod_debugme.xml om te zien waar de src van genamespaceerde klassen is gedefinieerd. Wanneer het in de bron is opgelost, moet je het zip-bestand opnieuw installeren of *administrator/cache/autoload_psr4.php* verwijderen. Bij afwezigheid bouwt Joomla dat bestand opnieuw op uit manifestbestanden. Maar als er een onjuiste of ontbrekende invoer in staat, wordt het niet gerepareerd totdat de extensie opnieuw is geïnstalleerd.
- Soms moet je een breakpoint een paar regels vóór de regel waar de fout optreedt instellen, vooral als je de waarden wilt controleren die aan functieaanroepen worden doorgegeven.
- Tabel *xxx.yyy\\debugme* bestaat niet. Ah ja, de code om een tabel aan te maken bij installatie en te verwijderen bij deïnstallatie is nooit gemaakt. Je zult een SQL-query moeten uitvoeren in phpMyAdmin met behulp van de inhoud van het *mod\\debugme.sql*-bestand. Vergeet niet om *\#\\* in de tabelnamen te wijzigen in je databasevoorvoegsel. En als het nog steeds faalt, controleer dan de tabelnaam in de code.

## Schermafbeelding

Als alles is opgelost, is dit wat je misschien zult zien:

![Site view of debugged module working](../../../en/images/getting-started/vscode-primer-debugme-fixed.png)

Taartdagen?

## Referenties

Uit de Joomla! Documentatie: [Visual Studio Code](https://docs.joomla.org/Visual_Studio_Code "Visual Studio Code") die ook de configuratie van andere tools behandelt, bijvoorbeeld CodeSniffer en PHPUnit.

*Vertaald door openai.com*

