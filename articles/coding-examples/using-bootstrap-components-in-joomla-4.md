<!-- Filename: J4.x:Using_Bootstrap_Components_in_Joomla_4 / Display title: Gebruik van Bootstrap-componenten in Joomla 4 -->

## Inleiding

### Bootstrap-componenten

Sommige van de componenten die in de Bootstrap-documentatie worden beschreven, gebruiken alleen CSS. De Breadcrumbs-component bijvoorbeeld, wordt gerenderd met CSS en vereist geen ondersteuning voor JavaScript. Andere reageren op gebruikersacties zoals klikken of zweven, en hebben JavaScript-ondersteuning nodig. Deze laatste worden hier aangeduid als interactieve componenten. Dit artikel legt uit hoe je ze kunt gebruiken in artikelen en een aangepaste module.

Zie de [Bootstrap-documentatie](https://getbootstrap.com/docs/5.0/getting-started/introduction/)

### Joomla 4 Introduceert een Modulaire Benadering voor Interactieve Componenten

- Wat is een modulaire aanpak?
- De functionaliteit wordt opgedeeld in individuele componenten die ondersteund worden door afzonderlijke bestanden. Er is geen **één bestand** aanpak zoals dat was met Bootstrap in Joomla 3. De modulaire aanpak is efficiënter en biedt prestatieverbeteringen (stuur alleen de code die nodig is in plaats van alles te leveren voor het geval een pagina een component nodig heeft).

### Interactieve Componenten Gebruiken: Coders

- Laad wat je nodig hebt per geval! Er zijn hulpmiddelen om individuele componenten op te zetten met de juiste argumenten.
- Bekijk de lijst van Interactieve Bootstrap-componenten.

### Het gebruik van interactieve componenten: Niet-coders

- Het gebruik van componenten in artikelen is niet zo eenvoudig omdat de hulpmiddelenfuncties niet kunnen worden aangeroepen vanuit een artikel of standaard Aangepaste HTML-module. Drie mogelijke oplossingen worden later in deze tutorial voorgesteld:
  - Een aangepaste module
  - Een aangepaste plugin
  - Een aangepaste module-override
- Sla de lijst van Bootstrap interactieve componenten over of scan deze. Je zult deze functieaanroepen niet direct gebruiken.

## Interactieve Componenten van Bootstrap

### Waarschuwing

Aannemende dat je het HTML-gedeelte al in je lay-out hebt, moet je ook de interactiviteit (het JavaScript-gedeelte) opnemen:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.alert', '.selector');
```

- De **.selector** verwijst naar de CSS-selector voor de waarschuwing. Je kunt deze functie meerdere keren aanroepen met verschillende CSS-selectors.
- Geen extra opties beschikbaar

### Knop

Aannemende dat je het HTML-gedeelte al in je Layout hebt, moet je ook de interactiviteit (het JavaScript-gedeelte) opnemen:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.button', '.selector');
```

- De **.selector** verwijst naar de CSS-selector voor de knop. Je kunt deze functie meerdere keren aanroepen met verschillende CSS-selectors
- Geen extra opties beschikbaar

### Carrousel

Aannemende dat je het HTML-gedeelte al in je lay-out hebt, moet je ook de interactiviteit (het JavaScript-gedeelte) toevoegen:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.carousel', '.selector', []);
```

- De **.selector** verwijst naar de CSS-selector voor de carrousel. Je kunt deze functie meerdere keren aanroepen met verschillende CSS-selectors
- Het derde argument verwijst naar de beschikbare opties voor de carrousel

Opties voor de carrousel kunnen zijn:

- **interval**, nummer, standaardwaarde:**5000**, De hoeveelheid tijd tussen het automatisch roteren van een item. Als false wordt ingesteld, zal de carrousel niet automatisch draaien.
- **keyboard**, boolean, standaardwaarde:**true** Of de carrousel moet reageren op toetsenbordgebeurtenissen.
- **pause**, string\|boolean, **hover** Stopt het draaien van de carrousel bij mouseenter en hervat het draaien bij mouseleave.
- **slide**, string\|boolean, standaardwaarde:**false** Speelt de carrousel automatisch af nadat de gebruiker het eerste item handmatig heeft geroteerd. Als "carousel" ingesteld is, speelt de carrousel automatisch af bij laden.

### Samenvouwen

Aangezien je het HTML-gedeelte al in je lay-out hebt, moet je ook de interactiviteit toevoegen (het JavaScript-gedeelte):

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.collapse', '.selector', []);
```

- De **.selector** verwijst naar de CSS-selector voor de collapse. Je kunt deze functie meerdere keren aanroepen met verschillende CSS-selectors.
- Het derde argument verwijst naar de beschikbare opties voor collapse.

Opties voor de inklap kunnen zijn:

- **ouder**, string, standaard:**false** Als een ouder is opgegeven, worden alle inklapbare elementen onder de opgegeven ouder gesloten wanneer dit inklapbare item wordt weergegeven.
- **schakel**, boolean standaard:**true** Schakelt het inklapbare element bij oproep.

### Vervolgkeuzelijst

Aangezien je het HTML-gedeelte al in je lay-out hebt, moet je ook de interactiviteit (het JavaScript-gedeelte) opnemen:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.dropdown', '.selector', []);
```

- De **.selector** verwijst naar de CSS-selector voor de dropdown. Je kunt deze functie meerdere keren aanroepen met verschillende CSS-selectoren.
- Het derde argument verwijst naar de beschikbare opties voor de dropdown.

Opties voor het inklappen kunnen zijn:

- **flip**, boolean, standaard:**true** Sta toe dat Dropdown omklapt in geval van overlapping op het referentie-element.
- **boundary**, string, standaard:**scrollParent** Overloopgrens van het dropdownmenu.
- **reference**, string, standaard:**toggle** Referentie-element van het dropdownmenu. Accepteert **toggle** of **parent**.
- **display**, string, standaard:**dynamic** Standaard gebruiken we Popper voor dynamische positionering. Schakel dit uit met **static**.

### Modaal

Als je ervan uitgaat dat je het HTML-gedeelte al in je Lay-out hebt, moet je ook de interactiviteit (het JavaScript-gedeelte) toevoegen:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.modal', '.selector', []);
```

- De **.selector** verwijst naar de CSS-selector voor de modal. Je kunt deze functie meerdere keren aanroepen met verschillende CSS-selectors.
- Het derde argument verwijst naar de beschikbare opties voor de modal.

Opties voor de modal kunnen zijn:

- **achtergrond**, string\|boolean standaard:**true** Bevat een modale-achtergrondelement. Geef als alternatief 'static' op voor een achtergrond die de modal niet sluit bij klikken.
- **toetsenbord**, boolean standaard:**true** Sluit de modal wanneer de escape-toets wordt ingedrukt.
- **focus**, boolean standaard:**true** Sluit de modal wanneer de escape-toets wordt ingedrukt.

### Offcanvas

Als je ervan uitgaat dat je het HTML-gedeelte al in je lay-out hebt, moet je ook de interactiviteit (het JavaScript-gedeelte) opnemen:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.offcanvas', '.selector', []);
```

- De **.selector** verwijst naar de CSS-selector voor de offcanvas. Je kunt deze functie meerdere keren aanroepen met verschillende CSS-selectoren.
- Het derde argument verwijst naar de beschikbare opties voor offcanvas.

Opties voor de offcanvas kunnen zijn:

- **achtergrond**, boolean, standaard:**true** Breng een achtergrond aan op het lichaam terwijl offcanvas open is.
- **toetsenbord**, boolean, standaard:**true** Sluit de offcanvas wanneer de escape-toets wordt ingedrukt.
- **scroll**, boolean, standaard:**false** Sta het scrollen van het lichaam toe terwijl offcanvas open is.

### Venster met extra informatie

Als u ervan uitgaat dat u het HTML-gedeelte al in uw lay-out hebt, moet u ook de interactiviteit (het JavaScript-gedeelte) opnemen:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.popover', '.selector', []);
```

- De **.selector** verwijst naar de CSS-selector voor de popover. Je kunt deze functie meerdere keren aanroepen met verschillende CSS-selectors.
- Het derde argument verwijst naar de beschikbare opties voor de popover.

Opties voor de popover kunnen zijn:

- **animatie**, boolean, standaard:**true** Een CSS-vervagingstransitie toepassen op de popover.
- **container**, string\|boolean, standaard:**false** Voegt de popover toe aan een specifiek element. Bijvoorbeeld: **body**.
- **inhoud**, string, standaard:**null** Standaard inhoudswaarde als data-bs-content attribuut niet aanwezig is.
- **vertraging**, nummer, standaard:**0** Vertraging bij het tonen en verbergen van de popover (ms) is niet van toepassing op de handmatige triggerstijl.
- **html**, boolean, standaard:**true** HTML invoegen in de popover. Als **false**, wordt de innerText eigenschap gebruikt om inhoud toe te voegen aan de DOM.
- **plaatsing**, string, standaard:**right** Hoe de popover te positioneren - **auto** \| **top** \| **bottom** \| **left** \| **right**. Wanneer auto is gespecificeerd, zal het de popover dynamisch heroriënteren.
- **selector**, string, standaard:**false** Als een selector is opgegeven, zullen popoverobjecten gedelegeerd worden naar de gespecificeerde doelen.
- **sjabloon**, string, standaard:**null** Basis-HTML om te gebruiken bij het maken van de popover.
- **titel**, string, standaard:**null** Standaard titelwaarde als het **title**-tag niet aanwezig is.
- **trigger**, string, standaard:**click** Hoe de popover wordt geactiveerd - **click** \| **hover** \| **focus** \| **manueel**.
- **offset**, geheel getal, standaard:**0** Offset van de popover ten opzichte van zijn doel.

### Scrollspionasje

Aangezien je het HTML-gedeelte al in je layout hebt, moet je ook de interactiviteit (het JavaScript-gedeelte) toevoegen:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.scrollspy', '.selector', []);
```

- De **.selector** verwijst naar de CSS-selector voor de scrollspy. Je kunt deze functie meerdere keren aanroepen met verschillende CSS-selectors.
- Het derde argument verwijst naar de beschikbare opties voor scrollspy.

Opties voor de Scrollspy kunnen zijn:

- **offset** nummer Pixels om mee te verschuiven vanaf de bovenkant bij het berekenen van de positie van scroll.
- **method** string Vindt in welke sectie het bespioneerde element zich bevindt.
- **target** string Geeft het element aan waarop de Scrollspy-plug-in moet worden toegepast.

### Tabblad

Aangenomen dat je het HTML-gedeelte al in je lay-out hebt, moet je ook de interactiviteit (het JavaScript-gedeelte) opnemen:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tab', '.selector', []);
```

- De **.selector** verwijst naar de CSS-selector voor het tabblad. Je kunt deze functie meerdere keren aanroepen met verschillende CSS-selectors.
- Het derde argument verwijst naar de beschikbare opties voor het tabblad.

Opties voor het tabblad kunnen zijn:

### Tooltip

Aangezien je het HTML-gedeelte al in je opmaak hebt, moet je ook de interactiviteit (het JavaScript-gedeelte) opnemen:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tooltip', '.selector', []);
```

- De **.selector** verwijst naar de CSS-selector voor de tooltip. Je kunt deze functie meerdere keren aanroepen met verschillende CSS-selectors.
- Het derde argument verwijst naar de beschikbare opties voor de tooltip.

Opties voor de tooltip kunnen zijn:

- **animatie**, boolean past een CSS fade-transitie toe op de popover.
- **container**, string\|boolean Voegt de popover toe aan een specifiek element: { container: **body** }.
- **vertraging**, number\|object vertraagt het tonen en verbergen van de popover (ms) - is niet van toepassing op handmatige trigger type. Als een nummer wordt opgegeven, wordt de vertraging toegepast op zowel verbergen/tonen. De objectstructuur is: vertraging: { show: 500, hide: 100 }.
- **html**, boolean Voegt HTML in de popover in. Als false wordt jQuery's tekstmethode gebruikt om inhoud in de dom in te voegen.
- **plaatsing**, string\|functie hoe de popover te positioneren - **boven** \| **onder** \| **links** \| **rechts**.
- **selector** string Als een selector wordt opgegeven, worden popover-objecten gedelegeerd naar de gespecificeerde doelen.
- **template**, string Basis HTML om te gebruiken bij het maken van de popover.
- **titel**, string\|functie standaard titelwaarde als het **titel**-label niet aanwezig is.
- **trigger**, string hoe popover wordt geactiveerd - hover \| focus \| handmatig.
- **beperkingen**, array Een array van beperkingen - doorgestuurd naar Popper.
- **offset**, string Verplaatsing van de popover ten opzichte van het doel.

### Toast

Uitgaande van het feit dat je het HTML-gedeelte al in je lay-out hebt, zul je ook de interactiviteit (het JavaScript-gedeelte) moeten toevoegen:

```php
    \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.toast', '.selector', []);
```

- De **.selector** verwijst naar de CSS-selector voor de toast. Je kunt deze functie meerdere keren aanroepen met verschillende CSS-selectors.
- Het derde argument verwijst naar de beschikbare opties voor toast.

Opties voor de toast kunnen zijn:

- **animatie**, boolean, standaard:**true** Pas een CSS-fade-overgang toe op de toast.
- **automatisch verbergen**, boolean, standaard:**true** Verberg de toast automatisch.
- **vertraging**, nummer, standaard:**5000** Vertraging bij het verbergen van de toast (ms).

### Zie ook

- **Accordion** gebruikt Collapse om panelen met gegevens weer te geven.
- **Lijstgroep** kan worden gecombineerd met Tab om inhoud met tabbladen weer te geven.

## Bootstrap-componenten gebruiken in artikelen

De HTML-markering voor de meeste componenten kan worden opgenomen in een artikel of een module die zelf in een artikel kan worden opgenomen. Het probleem is dat de HTMLHelper-aanroep om de JavaScript-ondersteuning in te stellen daar niet kan worden opgenomen. Er zijn verschillende mogelijke benaderingen voor dit probleem. Drie benaderingen worden hier voorgesteld: het gebruik van een aangepaste Module, het gebruik van een Plugin of het gebruik van een Template-override.

**Waarschuwing:** De TinyMCE- en JCE-editors verwijderen witruimte bij Opslaan en maken het bewerken van code moeilijk! De eenvoudige oplossing is om naar de rechterbovenhoek van je Beheerdersscherm te gaan en **Gebruikersmenu → Account bewerken** te selecteren en de Editor in te stellen op Code Mirror.

## Aanpak 1: Gebruik van een Aangepaste Module

Dit is waarschijnlijk de minst foutgevoelige aanpak omdat de ondersteuningsopties voor de Bootstrap-component worden geselecteerd met selectievakjes. De betrokken stappen zijn als volgt:

- Download, installeer en schakel deze [Custom Module](https://github.com/ceford/j4xdemos-mod-custom-bscompos/raw/master/mod_custom_bscompos.zip) in
- Ga vanuit het Administratormenu naar **Inhoud **→** Sitemodules → Nieuw**
- Selecteer **Custom BS Components**
- Voer een Titel in
- Zet de Editor om naar de platte-tekstmodus en plak of typ de HTML-code voor het onderdeel dat je wilt gebruiken.
- Scroll in het tabblad Opties naar de lijst van BS Componenten en selecteer het soort component in deze instantie van de module. Let op dat je meer dan één kunt selecteren als je meer dan één component gebruikt.
- Selecteer een modulepositie: ofwel
  - een sjabloongedefinieerde positie als je de module op een specifieke locatie wilt gebruiken of
  - typ een positie in als je de module binnen een specifiek artikel wilt gebruiken: typ in het artikel {loadposition whatever}
- Opslaan en ga naar de site om te Testen!

### Selectors

Voor sommige componenten wordt door een specifieke **class** in de HTML-code een JavaScript-actie getriggerd. In andere componenten wordt de actie getriggerd door een **data-bs-whatever** attribuut. De volgende zijn de huidige triggers en kunnen veranderen:

- **Alert** getriggerd door class="alert ..."
- **Knop** getriggerd door data-bs-toggle="button"
- **Carrousel** getriggerd door data-bs-ride="whatever"
- **Inklappen** getriggerd door data-bs-toggle="collapse"
- **Dropdown** getriggerd door data-bs-toggle="dropdown"
- **Modal** getriggerd door data-bs-toggle="modal"
- **Offcanvas** getriggerd door data-bs-toggle="offcanvas"
- **Popover** getriggerd door class="btn ..." of
    - tag (kan worden gewijzigd in class="haspopover ...") EN
    - data-bs-toggle="popover"
- **Scrollspy** getriggerd door data-bs-spy="scroll"
- **Tabblad** getriggerd door data-bs-toggle="tab"
- **Toast** getriggerd door class="toast ..."
- **Tooltip** getriggerd door class="btn ..." of
    - tag (kan worden gewijzigd in class="hastooltip ...") EN
    - data-bs-toggle="tooltip"

### Voorbeeld 1: Waarschuwing

Waarschuwingen kunnen in HTML-code worden gebruikt zonder ondersteuning van JavaScript. Dit is alleen nodig voor de sluitmogelijkheid. Voorbeeld van HTML-code:

```html
<div class="alert alert-warning alert-dismissible fade show" role="alert">
  <strong>Holy guacamole!</strong> You should check in on some of those fields below.
  <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
</div>
```

Voorbeeldresultaat van het opnemen van een module in een artikel:

![Bootstrap alert](../../../en/images/coding-examples/coding-examples-alert.png)

Merk op dat zonder JavaScript-ondersteuning de waarschuwing er precies zo uitziet als hierboven, maar een klik op de sluitknop \[X\] de waarschuwing niet zal verwijderen. Ook zal de waarschuwing bij elke pagina-lading verschijnen.

### Voorbeeld 2: Knoppen

Knoppen kunnen in HTML-code worden gebruikt zonder JavaScript-ondersteuning. Dit is alleen nodig voor de soms subtiele verandering van stijl die wordt toegepast op knoppen met een verandering van toestand, gestyled als actief. Bootstrap voorbeeldcode:

```html
<p><button type="button" class="btn btn-primary" data-bs-toggle="button" autocomplete="off">Toggle button</button>
<button type="button" class="btn btn-primary active" data-bs-toggle="button" autocomplete="off" aria-pressed="true">Active toggle button</button>
<button type="button" class="btn btn-primary" disabled data-bs-toggle="button" autocomplete="off">Disabled toggle button</button></p>
```

```html
<p><a href="#" class="btn btn-primary" role="button" data-bs-toggle="button">Toggle link</a>
<a href="#" class="btn btn-primary active" role="button" data-bs-toggle="button" aria-pressed="true">Active toggle link</a>
<a href="#" class="btn btn-primary disabled" tabindex="-1" aria-disabled="true" role="button" data-bs-toggle="button">Disabled toggle link</a></p>
```

Met deze stijl in de template user.css:

```css
    .btn.btn-primary.active {
        background-color: green;
    }
```

![Bootstrap buttons](../../../en/images/coding-examples/coding-examples-buttons.png)

De knoppen wisselen tussen blauw en groen.

### Voorbeeld 3: Carrousel

De Carousel biedt een diavoorstelling die door een reeks afbeeldingen of tekstpanelen cycli. Het volgende voorbeeld gebruikte afbeeldingen uit de Joomla 4 Sample. Bootstrap-code:

```html
<div id="carouselExampleCaptions" class="carousel slide" data-bs-ride="carousel">
    <div class="carousel-indicators">
        <button class="active" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="0" aria-current="true" aria-label="Slide 1"></button>
        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="1" aria-label="Slide 2"></button>
        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="2" aria-label="Slide 3"></button>
    </div>
    <div class="carousel-inner">
        <div class="carousel-item active"><img class="d-block w-100" src="images/sampledata/cassiopeia/nasa1-1200.jpg" alt="..." />
            <div class="carousel-caption d-none d-md-block">
                <h5>First slide label</h5>
                <p>Some representative placeholder content for the first slide.</p>
            </div>
        </div>
        <div class="carousel-item"><img class="d-block w-100" src="images/sampledata/cassiopeia/nasa2-1200.jpg" alt="..." />
            <div class="carousel-caption d-none d-md-block">
                <h5>Second slide label</h5>
                <p>Some representative placeholder content for the second slide.</p>
            </div>
        </div>
        <div class="carousel-item"><img class="d-block w-100" src="images/sampledata/cassiopeia/nasa3-1200.jpg" alt="..." />
            <div class="carousel-caption d-none d-md-block">
                <h5>Third slide label</h5>
                <p>Some representative placeholder content for the third slide.</p>
            </div>
        </div>
    </div>
    <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="prev">
        <span class="visually-hidden">Previous</span>
    </button>
    <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="next">
        <span class="visually-hidden">Next</span>
    </button>
</div>
```

Resultaat:

![Bootstrap carousel](../../../en/images/coding-examples/coding-examples-carousel.jpg)

### Voorbeeld 4: Samenvouwen

Inklappen wordt veel gebruikt in Joomla en je hebt mogelijk geen module of plugin nodig om een actie te activeren. De klik opent een paneel met extra informatie. Voorbeeld Bootstrap-code:

```html
<p>
    <a class="btn btn-primary" role="button" href="#collapseExample" data-bs-toggle="collapse" aria-expanded="false" aria-controls="collapseExample"> Link with href </a>
    <button class="btn btn-primary" type="button" data-bs-toggle="collapse" data-bs-target="#collapseExample" aria-expanded="false" aria-controls="collapseExample">
        Button with data-bs-target
    </button>
</p>
<div id="collapseExample" class="collapse">
    <div class="card card-body">
        Some placeholder content for the collapse component. This panel is hidden by default but revealed when the user activates the relevant trigger.
    </div>
</div>
```

Resultaat:

![Bootstrap collaps](../../../en/images/coding-examples/coding-examples-collapse.png)

### Voorbeeld 5: Dropdown

Dropdowns zijn schakelbare, contextuele overlays voor het weergeven van lijsten met links en meer. Voorbeeld van Bootstrap-code:

```html
<div class="btn-group">
    <button class="btn btn-danger dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false"> Action </button>
    <ul class="dropdown-menu">
        <li><a class="dropdown-item" href="#">Action</a></li>
        <li><a class="dropdown-item" href="#">Another action</a></li>
        <li><a class="dropdown-item" href="#">Something else here</a></li>
        <li><hr class="dropdown-divider" /></li>
        <li><a class="dropdown-item" href="#">Separated link</a></li>
    </ul>
</div>
```

Resultaat:

![Bootstrap dropdown](../../../en/images/coding-examples/coding-examples-dropdown.png)

### Voorbeeld 6: Modaal

Het modale component opent een dialoogvenster in het midden van het scherm. Er zijn nogal wat opties om de grootte en inhoud van de modal te controleren. Zie de Bootstrap-documentatie voor meer details. Voorbeeld van Bootstrap-code:

```html
<p><button class="btn btn-primary" type="button" data-bs-toggle="modal" data-bs-target="#exampleModal"> Launch demo modal </button></p>
<div id="exampleModal" class="modal fade" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 id="exampleModalLabel" class="modal-title">Modal title</h5>
                <button class="btn-close" type="button" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                ...
            </div>
            <div class="modal-footer">
                <button class="btn btn-secondary" type="button" data-bs-dismiss="modal">Close</button>
                <button class="btn btn-primary" type="button">Save changes</button>
            </div>
        </div>
    </div>
</div>
```

Resultaat:

![Bootstrap modal](../../../en/images/coding-examples/coding-examples-modal.png)

### Voorbeeld 7: Offcanvas

Op dit moment wordt deze component niet ondersteund in Joomla. Houd dit in de gaten - binnenkort beschikbaar!

### Voorbeeld 8: Popover

Pop-overs zijn zoals tooltips, maar met een titel. Ze hebben enkele toegankelijkheids- en prestatieproblemen en moeten daarom met voorzichtigheid worden gebruikt. Voorbeeld van Bootstrap-code:

```html
<p><button class="btn btn-lg btn-danger" title="Popover title" type="button" data-bs-toggle="popover" data-bs-content="And here's some amazing content. It's very engaging. Right?">Click to toggle popover</button></p>
```

Resultaat:

![Bootstrap alert](../../../en/images/coding-examples/coding-examples-popover.png)

### Voorbeeld 9: Scrollspy

Voorbeeldcode:

```html
<div class="row">
    <div class="col-4">
        <nav id="navbar-example3" class="navbar navbar-light bg-light flex-column">
            <a class="navbar-brand" href="#">Navbar</a><nav class="nav nav-pills flex-column">
            <a class="nav-link active" href="#item-1">Item 1</a>
            <nav class="nav nav-pills flex-column">
                <a class="nav-link ms-3 my-1" href="#item-1-1">Item 1-1</a>
                <a class="nav-link ms-3 my-1" href="#item-1-2">Item 1-2</a>
            </nav>
            <a class="nav-link" href="#item-2">Item 2</a>
            <a class="nav-link" href="#item-3">Item 3</a>
            <nav class="nav nav-pills flex-column">
                <a class="nav-link ms-3 my-1" href="#item-3-1">Item 3-1</a>
                <a class="nav-link ms-3 my-1" href="#item-3-2">Item 3-2</a>
            </nav>
        </nav>
    </div>
    <div class="col-8">
        <div class="scrollspy-example" tabindex="0" data-bs-spy="scroll" data-bs-target="#navbar-example3" data-bs-offset="0">
            <h4 id="item-1">Item 1</h4>
            <p>Placeholder content for the scrollspy example. This one relates to item 1. Takes you miles high, so high, 'cause she’s got that one international smile. There's a stranger in my bed, there's a pounding in my head. Oh, no. In another life I would make you stay. ‘Cause I, I’m capable of anything. Suiting up for my crowning battle. Used to steal your parents' liquor and climb to the roof. Tone, tan fit and ready, turn it up cause its gettin' heavy. Her love is like a drug. I guess that I forgot I had a choice.</p>
            <h5 id="item-1-1">Item 1-1</h5>
            <p>Placeholder content for the scrollspy example. This one relates to the item 1-1. You got the finest architecture. Passport stamps, she's cosmopolitan. Fine, fresh, fierce, we got it on lock. Never planned that one day I'd be losing you. She eats your heart out. Your kiss is cosmic, every move is magic. I mean the ones, I mean like she's the one. Greetings loved ones let's take a journey. Just own the night like the 4th of July! But you'd rather get wasted.</p>
            <h5 id="item-1-2">Item 1-2</h5>
            <p>Placeholder content for the scrollspy example. This one relates to the item 1-2. Her love is like a drug. All my girls vintage Chanel baby. Got a motel and built a fort out of sheets. 'Cause she's the muse and the artist. (This is how we do) So you wanna play with magic. So just be sure before you give it all to me. I'm walking, I'm walking on air (tonight). Skip the talk, heard it all, time to walk the walk. Catch her if you can. Stinging like a bee I earned my stripes.</p>
            <h4 id="item-2">Item 2</h4>
            <p>Placeholder content for the scrollspy example. This one relates to item 2. Don't need apologies. There is no fear now, let go and just be free, I will love you unconditionally. Last Friday night. Don't be a shy kinda guy I'll bet it's beautiful. Summer after high school when we first met. 'Cause she's the muse and the artist. What? Wait. No, no, no, no. Thought that I was the exception.</p>
            <h4 id="item-3">Item 3</h4>
            <p>Placeholder content for the scrollspy example. This one relates to item 3. Word on the street, you got somethin' to show me, me. All this money can't buy me a time machine. Make it like your birthday everyday. So we hit the boulevard. You make me feel like I'm livin' a teenage dream, the way you turn me on Skip the talk, heard it all, time to walk the walk. Word on the street, you got somethin' to show me, me. It's no big deal, it's no big deal, it's no big deal.</p>
            <h5 id="item-3-1">Item 3-1</h5>
            <p>Placeholder content for the scrollspy example. This one relates to item 3-1. Baby do you dare to do this? This is no big deal. Yeah, you're lucky if you're on her plane. Just own the night like the 4th of July! Standing on the frontline when the bombs start to fall. So just be sure before you give it all to me.</p>
            <h5 id="item-3-2">Item 3-2</h5>
            <p>Placeholder content for the scrollspy example. This one relates to item 3-2. You're original, cannot be replaced. All night they're playing, your song. California girls we're undeniable. Like a bird without a cage. There is no fear now, let go and just be free, I will love you unconditionally. I can see the writing on the wall. You could travel the world but nothing comes close to the golden coast.</p>
        </div>
    </div>
</div>
```

Resultaat:

![Bootstrap scrollspy](../../../en/images/coding-examples/coding-examples-scrollspy.png)

Ook is enige styling nodig in user.css:

```css
    .scrollspy-example {
        height: 350px;
        overflow-y: scroll;
    }
```

Probleem: het menu stemt niet goed overeen met de inhoud in dit voorbeeld!

### Voorbeeld 10: Tabblad

Tabs worden vaak gebruikt als navigatie-elementen in combinatie met dropdowns. Bootstrap voorbeeldcode:

```html
<ul class="nav nav-tabs">
    <li class="nav-item"><a class="nav-link active" href="#" aria-current="page">Active</a></li>
    <li class="nav-item dropdown"><a class="nav-link dropdown-toggle" role="button" href="#" data-bs-toggle="dropdown" aria-expanded="false">Dropdown</a>
        <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Action</a></li>
            <li><a class="dropdown-item" href="#">Another action</a></li>
            <li><a class="dropdown-item" href="#">Something else here</a></li>
            <li><hr class="dropdown-divider" /></li>
            <li><a class="dropdown-item" href="#">Separated link</a></li>
        </ul>
    </li>
    <li class="nav-item"><a class="nav-link" href="#">Link</a></li>
    <li class="nav-item"><a class="nav-link disabled" tabindex="-1" href="#" aria-disabled="true">Disabled</a></li>
</ul>
```

Resultaat:

![Bootstrap tab](../../../en/images/coding-examples/coding-examples-tab.png)

Vergeet niet zowel de Tab- als de Dropdown-opties te controleren zodat het dropdown-gedeelte werkt.

### Voorbeeld 11: Toast

Toastmeldingen zijn lichte meldingen die zijn ontworpen om de pushmeldingen na te bootsen die populair zijn geworden door mobiele en desktopbesturingssystemen. Ze zijn gebouwd met flexbox, dus ze zijn gemakkelijk uit te lijnen en te positioneren. Voorbeeld Bootstrap-code:

```html
<div class="toast fade show" role="alert" aria-live="assertive" aria-atomic="true">
    <div class="toast-header">
        <img class="rounded me-2" src="..." alt="..." />
        <strong class="me-auto">Bootstrap</strong> <small>11 mins ago</small>
        <button class="btn-close" type="button" data-bs-dismiss="toast" aria-label="Close"></button>
    </div>
    <div class="toast-body">Hello, world! This is a toast message.</div>
</div>
```

Resultaat:

![Bootstrap toast](../../../en/images/coding-examples/coding-examples-toast.png)

Merk op dat de Bootstrap-demo die een knop gebruikt om het Toast-bericht te tonen wat extra JavaScript nodig heeft. Het lijkt erop dat deze component een programmeur nodig heeft om er goed gebruik van te maken!

### Voorbeeld 12: Tooltip

Een tooltip is een klein stukje tekst dat verschijnt wanneer je met de muis over een knop- of linkelement beweegt om uit te leggen wat het is of doet. De tooltip kan boven, onder, links of rechts van het element worden gepositioneerd. Als er geen positie is gespecificeerd, is de standaardpositie boven. De tooltip zal naar een andere positie schakelen als er onvoldoende ruimte is op de gespecificeerde positie. Voorbeeld van Bootstrap-code:

```html
<p><button class="btn btn-secondary" title="Tooltip on left" type="button" data-bs-toggle="tooltip" data-bs-placement="left"> Tooltip on left </button>
<button class="btn btn-secondary" title="Tooltip" type="button" data-bs-toggle="tooltip"> Tooltip </button>
<button class="btn btn-secondary" title="Tooltip on top" type="button" data-bs-toggle="tooltip" data-bs-placement="top"> Tooltip on top </button>
<button class="btn btn-secondary" title="Tooltip on right" type="button" data-bs-toggle="tooltip" data-bs-placement="right"> Tooltip on right </button>
<button class="btn btn-secondary" title="Tooltip on bottom" type="button" data-bs-toggle="tooltip" data-bs-placement="bottom"> Tooltip on bottom </button>
<button class="btn btn-secondary" title="<em>Tooltip</em> <u>with</u> <b>HTML</b>" type="button" data-bs-toggle="tooltip" data-bs-html="true"> Tooltip with HTML </button></p>
<p>Tooltip triggered by class: <button class="btn btn-warning" title="Tooltip Message">Alert!</button></p>
```

Resultaat:

![Bootstrap tooltip](../../../en/images/coding-examples/coding-examples-tooltip.png)

## Aanpak 2: Een Inhoudsplugin Gebruiken

De betrokken stappen:

- Download, installeer en activeer deze plugin: [](https://github.com/ceford/j4xdemos-plg-bscompos/raw/master/plg_j4xdemos_bscompos.zip)
- Voeg in het artikel de tekst toe waarop de plugin actie onderneemt, bijvoorbeeld {bscompos modal carousel} zal het laden van de JavaScript triggeren die nodig is om een modale dialoog en een carrousel te ondersteunen. De plugin verwijdert de triggertekst en de omringende (nu) lege tags.
- Voeg de Bootstrap Component HTML-code direct in het artikel of in een module opgenomen in het artikel toe. Hieronder is voorbeeld HTML-code voor een eenvoudige Modal en een Modal met daarin een Carrousel. Let op dat dit niet werkt als de HTML-code zich in een module op een sjabloonlocatie bevindt.
- Dit zal ook werken voor een standaard Aangepaste module als de Optie Inhoud Voorbereiden is ingesteld op Ja.
- Test het!

## Benadering 3: Gebruik van een Sjabloonoverschrijving

De stappen die betrokken zijn:

- Maak een mod_custom-template-override.
- Voeg een mod_custom-module toe die de componentopmaak en triggerklassen bevat.
- Neem de module op in een artikel.

### De mod_custom Template Override

- Ga in de beheerdersinterface naar **Systeem → Site Templates → Cassiopeia Details en Bestanden**.
- Selecteer **Maak Overrides **→** mod_custom **→** default.php**.
- Voeg op de regel direct na defined('\_JEXEC') or die; de volgende code toe:

```php
    $module_class = $params->get('moduleclass_sfx');
    if (!empty($module_class))
    {
        $classes = explode(' ', $module_class);
        foreach ($classes as $class)
        {
        switch ($class)
            {
              case 'bs-alert':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.alert', '.alert');
                break;
              case 'bs-button':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.button', '.btn');
                break;
              case 'bs-carousel':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.carousel', '.selector', []);
                break;
              case 'bs-collapse':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.collapse', '.selector', []);
                break;
              case 'bs-dropdown':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.dropdown', '.selector', []);
                break;
              case 'bs-modal':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.modal', '.selector', []);
                break;
              case 'bs-offcanvas':
                // Not Found
                //\Joomla\CMS\HTML\HTMLHelper::_('bootstrap.offcanvas', '.btn', []);
                break;
              case 'bs-popover':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.popover', '.btn', []);
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.popover', 'a', []);
                break;
              case 'bs-scrollspy':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.scrollspy', '.selector', []);
                break;
              case 'bs-tab':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tab', '.selector', []);
                break;
              case 'bs-tooltip':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tooltip', '.btn', []);
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.tooltip', 'a', []);
                break;
              case 'bs-toast':
                \Joomla\CMS\HTML\HTMLHelper::_('bootstrap.toast', '.selector', []);
                break;
              default:
                // do nothing
            }
        }
    }
```

Deze code zoekt naar klassenamen ingesteld in mod_custom en maakt de HTMLHelper-aanroep om de JavaScript-ondersteuning in te stellen. Merk op dat het laatste item in elke aanroep een selector is die al dan niet kan worden gebruikt om actie te activeren. Veel van de componenten worden geactiveerd door gegevensattributen in de mark-up en gebruiken de selectors hier niet. Voor sommigen is de selector nodig. Bijvoorbeeld, het is logisch om de **.btn** klasse en de **a** tag te gebruiken om Tooltips te activeren.

### Een mod_custom Module voor een Modale Component

- Ga naar **Inhoud**→**Sitemodules**→**Nieuw**.
- Selecteer de **Aangepaste** module.
- Vul het formulier in:
  - Titel: Demomodel
  - Typ in het veld Positie **demomodal** voor gebruik in een artikel;
  - Module-inhoud: Schakel Editor uit voor invoer van platte tekst.
  - Plak de volgende code uit de Bootstrap-documentatie:

```html
<h2>Modal</h2>
<!-- Button trigger modal -->
<p><button class="btn btn-primary" type="button" data-bs-toggle="modal" data-bs-target="#exampleModal"> Launch demo modal </button></p>
<!-- Modal -->
<div id="exampleModal" class="modal fade" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 id="exampleModalLabel" class="modal-title">Modal title</h5>
                <button class="btn-close" type="button" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                ...
            </div>
            <div class="modal-footer">
                <button class="btn btn-secondary" type="button" data-bs-dismiss="modal">Close</button>
                <button class="btn btn-primary" type="button">Save changes</button>
            </div>
        </div>
    </div>
</div>
```

- Selecteer het tabblad Geavanceerd en voer in het veld Moduleklasse **bs-modal** in.
- Optioneel: stel Titel in op Verbergen om de H2 in de geplakte code te gebruiken.
- Opslaan en sluiten (maak je geen zorgen dat de modal er volledig verkeerd uitziet in de editor).

### Een Artikel en Menu-item maken

- Maak een nieuw artikel aan, Demo Modal, en stel in de platte tekst invoermodus de inhoud in op

```html
<div>{loadposition demomodal}</div>
```

- Maak een menu-item voor een enkel artikel.
- Test het:

![Bootstrap modal module in article](../../../en/images/coding-examples/coding-examples-modal-module.png)

### Een Modale Component met een Carrousel

- Maak een nieuw Aangepast module met een nieuwe Titel: Demo Modale Carrousel
- Positie: demomodalcarousel
- **Geavanceerd tabblad**→**Moduleklasse**: bs-modal bs-carousel
- Aangepaste inhoud van de module in platte tekst:

```html
<h2>Modal with Carousel</h2>
<div class="mod-custom custom">
    <!-- Button trigger modal -->
    <button class="btn btn-primary" type="button" data-bs-toggle="modal" data-bs-target="#exampleModal"> Launch demo modal </button>
</div>
<div id="exampleModal" class="modal fade" style="display: none;" tabindex="-1" aria-hidden="true">
    <div class="modal-dialog" role="document">
        <div class="modal-content">
            <div class="modal-header">
                <button class="close" type="button" data-bs-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">×</span>
                </button>
            </div>
            <div class="modal-body">
                <div id="carouselExampleCaptions" class="carousel slide" data-bs-ride="carousel">
                    <div class="carousel-indicators">
                        <button class="active" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="0" aria-current="true" aria-label="Slide 1"></button>
                        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="1" aria-label="Slide 2"></button>
                        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="2" aria-label="Slide 3"></button>
                    </div>
                    <div class="carousel-inner">
                        <div class="carousel-item active">
                            <img class="d-block w-100" src="images/sampledata/cassiopeia/nasa1-1200.jpg" alt="..." />
                            <div class="carousel-caption d-none d-md-block">
                                <h5>First slide label</h5>
                                <p>Some representative placeholder content for the first slide.</p>
                            </div>
                        </div>
                        <div class="carousel-item">
                            <img class="d-block w-100" src="images/sampledata/cassiopeia/nasa2-1200.jpg" alt="..." />
                            <div class="carousel-caption d-none d-md-block">
                                <h5>Second slide label</h5>
                                <p>Some representative placeholder content for the second slide.</p>
                            </div>
                        </div>
                        <div class="carousel-item">
                            <img class="d-block w-100" src="images/sampledata/cassiopeia/nasa3-1200.jpg" alt="..." />
                            <div class="carousel-caption d-none d-md-block">
                                <h5>Third slide label</h5>
                                <p>Some representative placeholder content for the third slide.</p>
                            </div>
                        </div>
                    </div>
                    <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="prev">
                        <span class="visually-hidden">Previous</span>
                    </button>
                    <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="next">
                        <span class="visually-hidden">Next</span>
                    </button>
                </div>
            </div>
        </div>
    </div>
</div>
```

- Maak een nieuw artikel aan met {loadposition demomodalcarousel} in de inhoud.
- Maak een nieuw menu-item voor een enkel artikel aan: Demo Modal Carousel
- Test het:

![Bootstrap modal carousel](../../../en/images/coding-examples/coding-examples-modal-carousel.png)

*Vertaald door openai.com*

