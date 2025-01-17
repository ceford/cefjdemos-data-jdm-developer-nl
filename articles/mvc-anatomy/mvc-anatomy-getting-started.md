<!-- Filename: J4.x:MVC_Anatomy:_Getting_Started / Display title: MVC Anatomie: Aan de slag -->

## Introductie

Joomla-componenten volgen de Model, View, Controller-benadering, of kortweg MVC. Het Model is bedoeld om het laden en opslaan van gegevens af te handelen. De View is bedoeld om de weergave van gegevens af te handelen. En de Controller is bedoeld om de programflow af te handelen, de interactie tussen de component Model- en View-code.

Als je je eigen component wilt bouwen, zijn er twee leermethoden die je kunt aannemen:

- **Bouw het stap voor stap:** door een eenvoudige tutorial te volgen die elke fase beschrijft.
- **Ontleed het bestand voor bestand:** door een werkend component uit elkaar te halen om te leren hoe elk deel werkt.

Deze tutorial gebruikt de laatstgenoemde benadering. De component die voor de tutorial wordt gebruikt, heet com_countrybase. Het wordt gebruikt om enkele basisgegevens over de landen van de wereld te beheren en weer te geven. Het is op zichzelf al nuttig en kan worden gebruikt als basis voor het bouwen van iets geavanceerders.

## Doelstellingen schetsen

Wat je project ook is, je zou moeten beginnen met een overzicht van je doelen en jezelf misschien afvragen of die doelen bereikt kunnen worden met de Joomla-kerncomponenten of een beschikbare extensie. Voor com_countrybase is een tabel met landeninformatie nodig, samen met invoer- en uitvoerpagina's. En misschien een module om een dagelijkse valutawisselkoers weer te geven. En zelfs een plugin die wordt aangeroepen door een cli-opdracht om valutagegevens op regelmatige tijdstippen bij te werken. Deze tutorial behandelt alleen de component.

De component heeft een databasetabel nodig om landeninformatie op te slaan en een tabel om valutagegevens op te slaan. Elk zal een lijstweergave en een bewerkingsweergave nodig hebben. Dit is niet iets dat kan worden bereikt met Joomla-kerncomponenten, dus het is duidelijk een geval voor een aangepaste component.

## Starttafels

Er zijn veel gegevens over landen beschikbaar uit allerlei bronnen in allerlei formaten. Aangezien er minstens 250 landen in de wereld zijn, zou het behoorlijk vervelend zijn om voor elk land afzonderlijk gegevens in te voeren via een gegevensinvoerformulier. Dus heb ik starttabellen voorbereid door gegevens te verzamelen uit verschillende bronnen, ze te combineren in een spreadsheet en vervolgens sql-instructies te exporteren om de tabellen te maken.

De gegevens zijn gebaseerd op ISO 3166 landcodes, maar enkele zeer bekende landen zijn niet opgenomen, zoals Engeland, Schotland en Wales. U hoeft zich hier geen zorgen over te maken. De tabellen worden geleverd met de com_countrybase component.

De bestanden voor de com_countrybase component zijn beschikbaar op GitHub. Je kunt een [ZIP](https://github.com/ceford/j4xdemos-com-countrybase/archive/refs/heads/master.zip) bestand downloaden en installeren om het te zien werken in het Administrator-menu. Maak een menu-item aan als je het wilt zien werken in je Sitetemplate. Pak het zip-bestand ook uit in je projectbestandsruimte, niet in je testwebsiteboom, om de componentbestandstructuur en -inhoud te onderzoeken met je favoriete IDE of tekstbewerkingshulpmiddel.

## Screenshot

Deze lijst van landenbeheerder bevat vijf items om de afbeeldingsgrootte te minimaliseren. Joomla geeft normaal gesproken 20 items weer.

![List of countries](../../../en/images/mvc-anatomy/com-countrybase-countries.png)

## Sjablooncomponent

Om u te helpen met uw eigen component aan de slag te gaan, is er een [boilerplate component](https://github.com/ceford/j4xdemos-com-bpsrc/archive/refs/heads/master.zip) beschikbaar op Github. Download en pak dit uit in uw projectbestandenruimte, niet in de boomstructuur van uw testwebsite. Na het downloaden voert u alle wijzigingen uit die in de README worden aangegeven en u bent klaar om te beginnen.

Er zijn ook een aantal gratis en commerciële extensiegeneratoren die je kunt proberen om een skeletcomponent te genereren voor je eigen doeleinden. [Joomla! Component Builder](https://www.joomlacomponentbuilder.com/) is gratis en lijkt uitgebreid.

*Vertaald door openai.com*

