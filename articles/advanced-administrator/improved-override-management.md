<!-- Filename: J4.x:Improved_Override_Management / Display title: Verbeterd Override management -->

<span id="main-portal-heading">GSoC 2018  
Verbeterd Override management  
Documentatie</span> [<img
src="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/75px-Gsoc2016.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/113px-Gsoc2016.png 1.5x, https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/150px-Gsoc2016.png 2x"
data-file-width="373" data-file-height="373" width="75" height="75"
alt="Gsoc2016.png" />](https://docs.joomla.org/GSOC_2018 "GSOC 2018")
Joomla!  4.x

## Inleiding

Dit project voegt een updatecontrole functie toe aan Joomla. Hierdoor
ontvangt de gebruiker een melding wanneer, tijdens een update, een
core-bestand is aangepast waarvoor een override is gemaakt in hun
template. Om veiligheid- en functionaliteitsproblemen te voorkomen
kunnen zij hun overrides aanpassen voor iemand het in de gaten heeft.

**Project repository link:**
<a href="https://github.com/joomla-projects/gsoc18_override_management"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla-projects/gsoc18_override_management</a>

## Krijg een voorsprong door Verbeterd override management

### Opmerkingen

Wanneer u nog geen ervaring hebt in Joomla ontwikkeling en nog niet zo
veel weet over de Template manager en overrides, lees:

1.  [Hoe Templatebeheer
    gebruiken](https://docs.joomla.org/J3.x:How_to_use_the_Template_Manager "Special:MyLanguage/J3.x:How to use the Template Manager")
2.  [Layout overrides in
    Joomla](https://docs.joomla.org/Layout_Overrides_in_Joomla "Special:MyLanguage/Layout Overrides in Joomla")

Nu u bekend bent met hoe de Templatebeheer werkt en de verschillende
override types in Joomla kunnen we de project kenmerken bespreken.

- Ondersteunde overrides
- Verschillen weergave
- Override - Quick Icon notificatie Plug-in
- Installeren - Override Plug-in
- Geüpdatet - Override Geschiedenis

## Soorten overrides die ondersteund worden door deze functie

Niet ondersteund worden: Alternatieve Layouts (waarbij de bestandsnaam
wordt gewijzigd), js en css overrides. Deze functie ondersteund de
override bestanden die in de lijst staan in de Maak Overrride tab.
Voorbeelden:

<img
src="https://docs.joomla.org/images/thumb/a/aa/Selection_035-en.png/800px-Selection_035-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/a/aa/Selection_035-en.png/1200px-Selection_035-en.png 1.5x, https://docs.joomla.org/images/thumb/a/aa/Selection_035-en.png/1600px-Selection_035-en.png 2x"
data-file-width="1907" data-file-height="956" width="800" height="401"
alt="Selection 035-en.png" />

## Verschillen weergave tussen core en override bestanden

Deze functie toont de verschillen tussen het core bestand en het
override bestand. Wanneer u een override bestand opent om aan te passen,
ziet u twee knoppen of schakelaars in de rechterbovenhoek van de pagina
wanneer het core bestand is afgesloten.

Knoppen zoals deze:

<img src="https://docs.joomla.org/images/6/66/Selection_027-en.png"
decoding="async" data-file-width="421" data-file-height="197"
width="421" height="197" alt="Buttons" />

Hier kunt u bepalen of u de vergelijkingsweergave of weergave van het
core bestand wilt tonen of verbergen. In de volgende afbeelding ziet u
de locatie van het core bestand en de vergelijkingsweergave in het
template beheer.

<img
src="https://docs.joomla.org/images/thumb/f/f2/Selection_028-en.png/800px-Selection_028-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/f/f2/Selection_028-en.png/1200px-Selection_028-en.png 1.5x, https://docs.joomla.org/images/thumb/f/f2/Selection_028-en.png/1600px-Selection_028-en.png 2x"
data-file-width="1904" data-file-height="952" width="800" height="400"
alt="Selection 028-en.png" />

U kunt het core bestand niet wijzigen.

### Hoe de vergelijkingsweergave werkt

Wanneer u op een override bestand klikt om dit aan te passen wordt de
functie methode `getCoreFile` aangeroepen. Deze krijgt twee parameters
`path` van het override bestand in de template mee. Voorbeeld:
`/html/layouts/joomla/form/field/user.php` en de client path van het
bestand waar het bij hoort `Site` of `Administrator`. Vervolgens wordt,
op basis van deze informatie, het pad naar het core bestand terug
gegeven als het bestaat. Voor het tonen van de vergelijkingsweergave
tussen het core- en het override bestand wordt de
<a href="https://github.com/kpdecker/jsdiff" class="external text"
target="_blank" rel="nofollow noreferrer noopener">jsdiff</a>
bibliotheek gebruikt.

## Override - Quick Icon notificatie Plug-in

Een quick icon notificatie plug-in die de notificatie in het
Controlepaneel toont en aangeeft hoeveel geüpdatete overrides er voor
alle templates zijn. Wanneer er updates zijn voor overrides zal de quick
icon er ongeveer uitzien zoals in het voorbeeld hieronder:

<img
src="https://docs.joomla.org/images/thumb/5/51/Selection_020-en.png/800px-Selection_020-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/5/51/Selection_020-en.png 1.5x"
data-file-width="1080" data-file-height="309" width="800" height="229"
alt="Quick Icon" />

Wanneer u op het icoon klikt wordt u naar de Templates gestuurd waar een
lijst met alle templates wordt getoond met hun omschrijving. U zult een
nieuwe kolom zien met de titel **Overrides**. Hier wordt het aantal
override updates weergegeven die bij die template horen. Als er niets
geüpdatet is in de template override ziet u een Up-to-date label.

<img
src="https://docs.joomla.org/images/thumb/d/d6/Selection_022-en.png/800px-Selection_022-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/d/d6/Selection_022-en.png/1200px-Selection_022-en.png 1.5x, https://docs.joomla.org/images/thumb/d/d6/Selection_022-en.png/1600px-Selection_022-en.png 2x"
data-file-width="1897" data-file-height="662" width="800" height="279"
alt="Templates" />

### Hoe de quick icon werkt

Het maakt een AJAX oproep naar `TemplateController.php` wat de
informatie teruggeeft en een notificatie laat zien wanneer er updates
zijn van overrides.

**Waarschuwing**

De quick icon notificatie plug-in werkt alleen en kan alleen data
ophalen wanneer de `installer/override` plug-in geactiveerd is. Wanneer
`installer/override` is uitgeschakeld zult u deze foutmelding in het
quick icoon zien.

<img
src="https://docs.joomla.org/images/thumb/9/9d/Selection_024-en.png/800px-Selection_024-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/9/9d/Selection_024-en.png/1200px-Selection_024-en.png 1.5x, https://docs.joomla.org/images/9/9d/Selection_024-en.png 2x"
data-file-width="1300" data-file-height="333" width="800" height="205"
alt="Enable" />

Wanneer u op het quick icoon klikt wordt u doorgestuurd naar de
`installer/override` plug-in instellingen waar u deze kunt aanpassen.

## Installeren - Override Plug-in

Deze plug-in is het belangrijkste onderdeel van deze functie. Het maakt
het mogelijk de juiste geüpdatete overrides te vinden tijdens de
installatie of update van de extensie en de Joomla update.

<img
src="https://docs.joomla.org/images/thumb/f/fe/Selection_025-en.png/800px-Selection_025-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/f/fe/Selection_025-en.png/1200px-Selection_025-en.png 1.5x, https://docs.joomla.org/images/thumb/f/fe/Selection_025-en.png/1600px-Selection_025-en.png 2x"
data-file-width="1887" data-file-height="719" width="800" height="305"
alt="Installer Override" />

### Hoe installeren override plug-in werkt

De plug-in werkt op 6 gebeurtenissen:

- onExtensionBeforeUpdate
- onExtensionAfterUpdate
- onInstallerBeforeInstaller
- onInstallerAfterInstaller
- onJoomlaBeforeUpdate
- onJoomlaAfterUpdate

Welke alle override core bestanden `md5_file()` hash verzamelt voor en
na de update en vervolgens beide waardes vergelijkt. Vervolgens vindt
het het correcte gewijzigde of bijgewerkte bestand en slaat die
infromatie op in de `#__templates_overrides` tabel.

## Geüpdatet - Override geschiedenis

U kunt dit vinden op de Geüpdatet bestanden tab in een template. Het is
een lijstweergave die de geüpdatete overrides laat zien die bij de
template horen.

<img
src="https://docs.joomla.org/images/thumb/b/b9/Selection_029-en.png/800px-Selection_029-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/b/b9/Selection_029-en.png/1200px-Selection_029-en.png 1.5x, https://docs.joomla.org/images/thumb/b/b9/Selection_029-en.png/1600px-Selection_029-en.png 2x"
data-file-width="1901" data-file-height="737" width="800" height="310"
alt="Selection 029-en.png" />

Er zijn veel opties beschikbaar om deze lijst te beheren. U kunt de
status van de override bestandsgeschiedenis zien, de aanmaakdatum, datum
van de wijziging en update acties als: of het hoort bij Joomla update,
extensie update of een extensie installatie.

**Operkingen**

Deze informatie is alleen een geschiedenis, dus als u de bijgewerkte
overrides hebt aangevinkt, kunt u de geschiedenis verwijderen wanneer
deze niet meer nodig is.

**Bekijk de video uitleg en leer hoe u deze functie kunt gebruiken.**
