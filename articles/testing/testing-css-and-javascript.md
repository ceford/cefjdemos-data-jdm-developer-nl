<!-- Filename: J4.x:SCSS_and_Sass / Display title: Testen van CSS en JavaScript -->

## Inleiding

In een productie Joomla-site levert Joomla gecomprimeerde CSS- en Javascript-bestanden (degene met `.min` in de bestandsnaam). Servers sturen de gecomprimeerde versies als ze beschikbaar zijn (degene die eindigen op een `.gz` achtervoegsel). Deze bestanden worden gemaakt van ongecomprimeerde bronnen. In het geval van CSS-bestanden worden ze vaak gemaakt door SCSS-voorgangers te verwerken. Voor het testen van code die nieuwe of bijgewerkte CSS of JavaScript bevat, is het noodzakelijk om de CSS-bestanden en de gecomprimeerde en geminimaliseerde versies vanaf de gewijzigde bronnen opnieuw te bouwen.

## Testinstallatie

Voor testdoeleinden heb je een testserver nodig waarop Git, Node.js en Composer zijn geïnstalleerd. Ga naar de [Joomla CMS](https://github.com/joomla/joomla-cms) repository op GitHub en volg de instructies bij *How to get a working installation from the source* onderaan de README-tekst.

Na voltooiing van de Joomla-installatie, verwijder de Installatiemap niet! Dat zal ook de map `build` verwijderen die nodig is voor het herbouwen van CSS- en JavaScript-bestanden.

De kern `.scss` bestanden bevinden zich in de volgende directories:

- media/templates/site/cassiopeia/scss
- media/templates/administrator/atum/scss

Het CSS-generatiescript, de SCSS-compiler en andere vergelijkbare build-tools bevinden zich in de `build/`-map. De build-map is alleen beschikbaar vanuit de Joomlaǃ-bron, deze is niet inbegrepen in een officiële Joomlaǃ-release.

De installatie-instructies bevatten het volgende:

```sh
git checkout 5.2-dev (or whatever branch you wish to work with)
composer install
npm ci
```

De opdracht `npm ci` is een synoniem voor `npm clean-install` en wordt gebruikt in elke situatie waarin je zeker wilt zijn van een schone installatie van je afhankelijkheden.

## Dagelijks gebruik

Er zijn alternatieve opdrachten voor gebruik in situaties waarin alleen SCSS- of JavaScript-bestanden zijn gewijzigd:

```sh
npm run build:css¶
    This command compiles SCSS files to CSS and also creates the minified files.

npm run build:js¶
    This command compiles and transpiles the JavaScript files to the correct format and creates minified files.
```

De opdrachten doorzoeken de media- en templates-mappen en bouwen de uiteindelijke bestandversies die nodig zijn voor een Joomla-installatie.

## Sass, SCSS en CSS

> Sass is een stylesheet-taal die wordt gecompileerd naar CSS. Het stelt je in staat om variabelen, geneste regels, mixins, functies en meer te gebruiken, allemaal met een volledig CSS-compatibele syntaxis. Sass helpt om grote stylesheets goed georganiseerd te houden en maakt het gemakkelijk om ontwerp binnen en tussen projecten te delen. Sass-bestanden hebben de suffix `scss`.

### CSS

CSS-code wordt als volgt uitgedruktː

```css
#header {
    margin: 0;
    border: 1px solid blue;
}
#header p {
    font-size: 14px;
    color: blue;
}
#header a {
    text-decoration: none;
}
```

### SCSS

`SCSS` is een extensie van de syntaxis van CSS. Het wordt gebruikt in de Joomlaǃ kern.

```css
$color:    blue;
#header {
    margin: 0;
    border: 1px solid $color;
    p {
        color: $colorRed;
        font: {
            size: 14px;
        }
    }
    a {
        text-decoration: none;
    }
}
```

Een oudere `Sass`-syntaxis biedt een beknoptere manier om CSS te schrijven.

```css
$color:    blue
#header
    margin: 0
    border: 1px solid $color
        p
            color: $colorRed
            font:
                size: 14px
        a
            text-decoration: none
```

Je kunt meer informatie vinden in de [Sass Documentatie](http://sass-lang.com/documentation/syntax/).

## Van bestaande CSS naar SCSS

Als je het Cassiopeia-sjabloon wilt aanpassen, is het een goed idee om het sjabloon te kopiëren en het daarna aan te passen, zodat Joomla!-updates je aanpassingen niet overschrijven. Om je eigen CSS-bestanden toe te voegen aan een kopie van het Cassiopeia-sjabloon, hernoem je gewoon je `.css`-bestanden naar `.scss`. Je kunt dan gebruikmaken van de mogelijkheden die SCSS biedt:

- Taaluitbreidingen zoals variabelen, nesten en mixins
- Veel nuttige functies voor het manipuleren van kleuren en andere waarden
- Geavanceerde functies zoals controledirectieven voor bibliotheken
- Goed opgemaakt, aanpasbaar output

Zie de [Sass-site](https://sass-lang.com/) voor volledige details.

### Importeer uw .css als .scss-bestanden

Stel dat je je eigen bestanden wilt opnemen en ze wilt laten parseren door de SCSS-compiler - je hoeft je .css niet naar SCSS te herschrijven, want gewone `.css` werkt ook. Wat je moet doen:

- hernoem je `.css` bestanden naar `.scss`
- voorzie ze van een `_` aan het begin
- voeg een `@import` statement toe aan de onderkant van `media/templates/site/copy_of_cassiopeia/scss/template.scss` zodat het bestaande declaraties overschrijft:

```css
// Bootstrap functions
@import "../../../media/vendor/bootstrap/scss/functions";
//...
// 50 or more lines of import statements
// Finally add the cassiopeia colours
:root {
  @each $color, $value in $cassiopeia-colors {
    --#{$color}: #{$value};
  }
// More lines containing scss color statements, then your own styles:
@import "mystyles";
}
```

Als je nu je hoofdsjabloon.scss-bestand compileert, eindig je met één geoptimaliseerde en geminimaliseerde hoofdsjabloon.css. Je hebt ook je http-verzoeken verminderd en de site zou sneller moeten laden.

*Vertaald door openai.com*

