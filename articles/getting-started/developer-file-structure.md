<!-- Filename: J4.x:Developer:_File_Structure / Display title: Voorbeeldbestandsstructuur -->

## Inleiding

Als individu die begint met het ontwikkelen van Joomla-extensies op een persoonlijke computer, laptop of desktop, moet je een geschikte bestandsstructuur opzetten voor je extensiecode. De locatie van je Joomla-site is vooraf bepaald door je besturingssysteem en Apache-installatie. De locatie van de code die je maakt, is aan jou.

## Testlocatie van de Website

In dit voorbeeld bevinden Joomla-testsites zich in submappen van de documentroot. Dit maakt het mogelijk om veel websites te maken voor allerlei verschillende projecten zonder de noodzaak om virtuele hosts aan te maken. Sommige testsites zijn mogelijk helemaal niet gerelateerd aan Joomla. De siteroot voor verschillende platforms:

- Mac: /Users/gebruikersnaam/Sites
- Linux: /home/gebruikersnaam/public_html
- Windows: ...

Dit is een schermafbeelding van een deel van een Sites maplijst die een selectie van vele testsites toont:

![multiple sites on mac](../../../en/images/getting-started/developer-file-structure-mac-sites.png)

Elk wordt benaderd via de naam van zijn submap. Voorbeelden:

- `http://localhost/j4test`
- `http://localhost/j520`

Er zijn omstandigheden waarin je de voorkeur kunt geven aan het creëren van afzonderlijke virtuele sites. Dat wordt hier niet behandeld.

## Bestandsstructuur van de Test Website

Als je dit nog niet hebt gedaan, moet je vertrouwd raken met de structuur van een Joomla-website. De volgende illustratie toont een typische Joomla-bestands- en mapstructuur, met de Administrator-map uitgebreid om de inhoud ervan te tonen.

![joomla file structure with administrator expanded](../../../en/images/getting-started/developer-file-structure-mac-joomla.png)

Dit is waar de werkende code zal worden geïnstalleerd. De broncode bevindt zich ergens anders.

## Locatie van de Ontwikkelaarsextensie Code

De locatie van je extensiecode is een persoonlijke keuze. Ik houd ervan om mijn extensiecode te bewaren in een bestandsstructuur die geschikt is voor het maken van een installeerbaar zip-bestand. De basis van mijn structuur is /Users/username/git omdat ik git kan spellen en ik gebruik git voor versiebeheer. Je hoeft dat niet te doen - git zal worden behandeld in een aparte tutorial. Mijn git-oudermap bevat veel submappen die elk aparte git-mappen kunnen gebruiken voor versiebeheer. Dit is een screenshot die een gedeeltelijke lijst van projecten toont:

![joomla file structure project folders](../../../en/images/getting-started/developer-file-structure-mac-project-folders.png)

Merk op dat sommige van de mapnamen beginnen met `j4xdemos`, wat ik heb aangenomen als het eerste deel van de namespace die ik gebruik voor mijn projecten die zijn gemaakt voor Joomla 4 tutorialdoeleinden. Het is niet noodzakelijk als onderdeel van de mapnaam, maar het is iets om over na te denken: het eerste deel van je namespace moet iets unieks zijn voor jou of je organisatie. Ik heb sindsdien `cefjdemos` aangenomen als mijn namespace-prefix omdat het persoonlijker is en niet specifiek voor een Joomla-versie.

### Projectmapstructuur

Als we j4xdemos-com-mywalks als voorbeeld nemen, bevindt alle code die in de extensie zal worden opgenomen zich in de com_mywalks-map. Wanneer dit wordt gecomprimeerd, wordt het com_mywalks.zip-bestand dat is gemaakt gebruikt voor installatie in Joomla. Geen van de andere bestanden in de root worden opgenomen in het zip-bestand, maar ze kunnen wel worden opgenomen in de GitHub Repository. Bijvoorbeeld, README.md is een Markdown-formaat tekstbestand dat een korte beschrijving van de extensie bevat. De mapnaam j4xdemos-com-mywalks is niet significant. Het wordt nergens anders gebruikt dan om de mappen alfabetisch te sorteren.

In de volgende illustratie is de map j4xdemos-com-mywalks geopend in VSCodium om de projectcode-structuur te tonen. Het bestand mywalks.xml is een manifestbestand dat Joomla vertelt wat waar te installeren. De admin- en site-mappen bevatten code die naar administrator/components/com_mywalks en components/com_mywalks gaat.

![Project folder open in vscodium](../../../en/images/getting-started/developer-file-structure-mac-vscodium.png)

Het zou duidelijk moeten zijn dat zelfs een kleine component behoorlijk veel mappen en bestanden nodig heeft. Er zijn sjablonen voor uitbreidingsgereedschappen beschikbaar om snel een skeletcomponent te maken. Deze worden elders behandeld. ToDo

Klaar om wat code te maken?

*Vertaald door openai.com*

