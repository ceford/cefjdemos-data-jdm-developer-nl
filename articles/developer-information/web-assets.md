<!-- Filename: J4.x:Web_Assets / Display title: Webactiva  -->

## Over Webassets

Er is een volledige beschrijving van Web Assets in de Joomla! Programmeursdocumentatie, gereproduceerd op [deze site](jdocmanual?article=docus/web-asset-manager/index) of beschikbaar via de [oorspronkelijke bron](https://manual.joomla.org/docs/general-concepts/web-asset-manager/).

## Aanvullende Voorbeelden

In Jdocmanual bevatten veel van de artikelen codeblokken die profiteren van syntaxisaccentuering. Hier is hoe het wordt bereikt:

```php
/** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
$wa = $this->document->getWebAssetManager();

// Register and attach a custom item in one run
$wa->registerAndUseStyle('highlight', 'https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/styles/default.min.css', [], [], []);

// Register and attach a custom item in one run
$wa->registerAndUseScript('highlight','https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/highlight.min.js', [], [], ['core']);

// Add an inline content as usual, will be rendered in flow after all assets
$wa->addInlineScript('hljs.highlightAll();');
```

Deze code staat in het bestand `default.php` dat wordt gebruikt om deze pagina weer te geven! Het is een beetje lastig te implementeren omdat de aanroep van `hljs.highlightAll()` moet worden gedaan nadat de `css`- en `js`-bestanden zijn geladen en opnieuw wanneer de pagina-inhoud wordt gewijzigd met een JavaScript-aanroep.

Andere syntaxis-markeerders zijn beschikbaar!

*Vertaald door openai.com*

