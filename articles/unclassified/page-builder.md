<!-- Filename: J4.x:Page_Builder / Display title: Page Builder -->

<span id="main-portal-heading">GSoC 2019
Joomla 4 Page Builder
Documentatie</span> [<img
src="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/75px-Gsoc2016.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/113px-Gsoc2016.png 1.5x, https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/150px-Gsoc2016.png 2x"
data-file-width="373" data-file-height="373" width="75" height="75"
alt="Gsoc2016.png" />](https://docs.joomla.org/GSoC_2019 "GSoC 2019")
Joomla!  4.x

## Inleiding

Stelt u zich voor: iemand wil een template maken, maar zonder HTML te
schrijven. CSS zou alleen gebruikt moeten worden voor de opmaak, niet
voor het vastleggen van een layout. Dan zijn de Page Builder en de
frontend template *Apodis* nuttige hulpmiddelen. De belangrijkste focus
ligt op editor die het mogelijk maakt zelf gedefinieerde posities te
creëren met behulp van een drag & drop GUI. Gebruikers kunnen
voorgedefinieerde elementen - containers, raster, kolommen, module
posities - gebruiken, maar ook eigen elementen toevoegen. 3rd party
developers kunnen hun eigen elementen toevoegen via plug-ins.

## Hoe begin ik?

Om de Page builder te gebruiken moet in de actieve frontend template het
pagebuilder veld in de `templateDetails.xml` zijn opgenomen, zoals hier
in Apodis:

```xml
<fieldset name="pagebuilder" label="TPL_APODIS_PAGEBUILDER">
   <field
		name="grid"
		type="pagebuilder"
		hidden="true"
		label="TPL_APODIS_PAGEBUILDER"
   />
</fieldset>
```

De template zelf moet de RenderHelper laden, die de parameters, gevuld
door de editor, weergeeft. Dit gebeurt in de `index.php` van de
template.

```php
    use Joomla\Component\Templates\Administrator\Helper\RenderHelper;

    $grid = $this->params->get('grid');
```

Hierna moet de output in de HTML body geplaatst worden:

```php
<?php echo RenderHelper::renderElements($grid); ?>
```

## Tekstverwerker

De Page builder tekstverwerker is beschikbaar in de Template stijlen op
het juiste tabblad. Het ondersteunt het toevoegen van nieuwe elementen,
drag en drop om ze te herschikken en te positioneren zoals gewenst en de
mogelijkheid om het formaat van kolommen te wijzigen.

Page builder tekstverwerker:

<img src="https://docs.joomla.org/images/7/76/Pagebuilder-editor-en.jpg"
decoding="async" data-file-width="800" data-file-height="318"
width="800" height="318" alt="Pagebuilder-editor-en.jpg" />

Gebruikers kunnen verschillende elementen toevoegen aan de layout door
*Element toevoegen* knop onderaan de rij te gebruiken. Vervolgens
verschijnt een venster waarin zij beschikbare elementen kunnen
selecteren en direct verdere opties kunnen instellen.

<img
src="https://docs.joomla.org/images/9/9d/Pagebuilder-add-element-en.jpg"
decoding="async" data-file-width="800" data-file-height="480"
width="800" height="480" alt="Add elements to the layout" />

Het is mogelijk om aangepaste CSS classes toe te voegen met behulp van
het instellingen icoon, aanwezig op elk element. Hier kunnen gebruikers
ook offsets toevoegen aan kolommen en naam en module chrome selecteren
voor elke modulepositie. Plug-ins kunnen hun eigen opties voor de
zijbalk aanbieden.

Gebruikers kunnen elementen wijzigen in de instellingen zijbalk:

<img
src="https://docs.joomla.org/images/b/bc/Pagebuilder-element-settings-en.jpg"
decoding="async" data-file-width="800" data-file-height="492"
width="800" height="492" alt="Pagebuilder-element-settings-en.jpg" />

De output is een JSON object, opgeslagen in het parameterveld bij het
type, opties, afmeting en kinderen van ieder element. De tekstverwerker
laad de opgeslagen layout automatisch vanuit de parameter zodat de
gebruiker direct door kan gaan met bewerken.

## Elementen

Er zijn vier standaard elementen in de Page builder:

1.  Container
2.  Raster
3.  Kolom
4.  Module positie

De standaard configuratie is hieronder beschikbaar, deze kan worden
aangepast via plug-ins.

|                       |              |                         |        |                               |
|-----------------------|--------------|-------------------------|--------|-------------------------------|
| Config                | Container    | Grid                    | Column | Module Position               |
| Parents allowed       | Root, Column | Root, Column, Container | Grid   | Root, Grid, Column, Container |
| Contain children      | True         | True                    | True   | False                         |
| Can contain component | True         | True                    | True   | False                         |
| Can contain message   | True         | True                    | True   | False                         |

## Component en Bericht

Niet alleen de module posities zijn belangrijke elementen in de Page
builder. De positie van de component en de weergegeven berichten kunnen
worden ingesteld door slepen en neerzetten op elementen die dit
accepteren. Dit maakt het uiterst eenvoudig om de posities te
selecteren, die de pagina-inhoud gebruiken die gebruikers gewend zijn.
Slechts één van beide kan op één element worden geplaatst. Een klik op
de 'x' verwijdert de positie en deze kan opnieuw worden vervangen.

Gebruikers kunnen het component of berichten positie op Page builder
elementen plaatsen:

<img
src="https://docs.joomla.org/images/3/37/Pagebuilder-set-component-and-message-en.jpg"
decoding="async" data-file-width="800" data-file-height="402"
width="800" height="402"
alt="Users can set the component or message position on Page builder elements" />

## Apodis

De Page builder is geïntegreerd in een nieuwe frontend template genaamd
Apodis. Dit maakt het mogelijk voor gebruikers om hun eigen template
stijlen te selecteren en te slepen en neer te zetten met behulp van de
tekstverwerker. Het nieuwe template ontwerp plaatst de Page builder
elementen in de index.php en toont de gegenereerde output direct. Dit is
nuttig voor onafhankelijke testen, waar style sheets het standaard
gedrag niet kunnen beïnvloeden. Zo werkt het: een com_templates helper
functie wordt opgeroepen in index.php en doorloopt de JSON parameter die
is verkregen uit de tabel \#\_template_styles, die type, positie en alle
informatie over opgeslagen elementen van de page builder bevat.
Afhankelijk van het type en de opties, wordt de weergave gewijzigd of
HTML toegevoegd. In de toekomst zal de generator ook plug-ins voor de
page builder en de gewenste rendering observeren.

## Integreer status en methoden met behulp van Vuex

Vuex wordt gebruikt om het mogelijk te maken meerdere componenten hun
status op te laten halen uit een gecentraliseerde Vuex store en kan
reactief en efficiënt updaten wanneer de status van de store verandert.
Het gebruik van Vuex heeft de codebasis en architectuur van de page
builder veel meer modulair, efficiënt en uitbreidbaar voor toekomstige
extensies gemaakt. Het stelt ons ook in staat gebeurtenissen bij te
houden die van status veranderen en helpt ons bij het debuggen met
behulp van Vue Devtools Extension.

## Bootstrap 4

We hebben onze eigen BS4-bestanden opgenomen in de editor van de page
builder voor het geval dat de backend template geen Bootstrap-bestanden
bevat.
