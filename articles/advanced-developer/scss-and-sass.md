<!-- Filename: J4.x:SCSS_and_Sass / Display title: SCSS en Sass -->

De meeste standaard stylesheet van Joomla 4.x zijn geschreven met behulp
van <a
href="https://en.wikipedia.org/wiki/https://en.wikipedia.org/wiki/Sass_(stylesheet_language))"
class="extiw"
title="wikipedia:https://en.wikipedia.org/wiki/Sass (stylesheet language))">SCSS</a>
en worden vervolgens gecompileerd om de
<a href="https://en.wikipedia.org/wiki/CSS" class="extiw"
title="wikipedia:CSS">CSS</a> bestanden genereren.

## Waar vind je de .scss stylesheets en compiler?

De core `.scss` bestanden staan in verschillende mappen. Dit zijn met
name deze bestandenː

- templates/cassiopeia/scss/template.scss
- administrator/templates/atum/scss/bootstrap.scss
- administrator/templates/atum/scss/font-awesome.scss
- administrator/templates/atum/scss/template.scss
- administrator/templates/atum/scss/template-rtl.scss
- media/plg_installer_webinstaller/scss/client.scss

Het script dat de CSS genereert, de SCSS compiler, en andere
vergelijkbare bouw hulpmiddelen kunt u vinden in de `build` map van de
Joomla! source op GitHub
<a href="https://github.com/joomla/joomla-cms" class="external text"
target="_blank" rel="nofollow noreferrer noopener">GitHub</a>. Zie [Git
voor
Programmeurs](https://docs.joomla.org/Git_for_Coders "Special:MyLanguage/Git for Coders")
voor meer informatie over het gebruik van GitHub. De build map is alleen
beschikbaar vanuit de Joomla! source, het is geen onderdeel van de
officiële Joomla versie.

## Voorwaarden

Voor het compileren van uw eigen SCSS bestanden met de Joomla! core moet
Node.js op uw locale machine geïnstalleerd zijn. Om Node.js te
installeren gaat u naar
<a href="https://nodejs.org/en/" class="external text" target="_blank"
rel="nofollow noreferrer noopener">Node.js</a>, de officiële website,
download de juiste setup voor uw besturingssysteem en installeer het met
behulp van de installatie wizard. NPM (Node Package Manager) wordt
gebruikt om de JavaScript testomgeving in te richten en te managen.

## Dependencies (pakketten) installeren

Open een command line en navigeer naar de root map van Joomla 4.x

Execute command `npm install`

    $ npm install
    ...
    added 1354 packages in 193.687s
    $

Dit zal de dependencies installeren in de map `/node_modules`. Als er
geen map `/nod_modules` bestaat zal deze gemaakt worden door npm. Het is
niet erg als uw een aantal waarschuwingen ziet, maar u zou geen
foutmeldingen moet tegenkomen.

## Opnieuw de CSS stylesheet genereren

Om alle CSS-bestanden van een Joomlaǃ-core distributie opnieuw te
genereren, moet u de generatiescripts uitvoeren als een
<a href="https://en.wikipedia.org/wiki/Command-line_interface"
class="extiw"
title="wikipedia:Command-line interface">CLI-toepassing</a>.

### Alle Joomla! core scss bestanden

U kunt alle Joomlaǃ core css-bestanden compileren met de opdracht
` node build --compile css `. Bijvoorbeeld:

    $ npm run build:css
    Prefixing for:  last 1 version
    Without `from` option PostCSS could generate wrong source map and will not find Browserslist config. Set it to CSS file path or to `undefined` to prevent this warning.
    template.css was updated.
    Prefixing for:  last 1 version
    template-rtl.css was updated.
    Prefixing for:  last 1 version
    font-awesome.css was updated.
    Prefixing for:  last 1 version
    client.css was updated.
    Prefixing for:  last 1 version
    bootstrap.css was updated.
    Prefixing for:  last 1 version
    template.css was updated.
    $

### Alle bestanden in een map

U kunt alle bestanden in één map compileren met de opdracht
`node build --compile-css path-to-directory`. Bijvoorbeeldː
`node build --compile-css templates/cassiopeia/scss`.

    $ node build --compile-css templates/cassiopeia/scss
    Prefixing for:  last 1 version
    Without `from` option PostCSS could generate wrong source map and will not find Browserslist config. Set it to CSS file path or to `undefined` to prevent this warning.
    template.css was updated.

### Een specifiek bestand

U kunt één bestand compileren met de opdracht
`node build --compile-css pathtofile`. Bijvoorbeeldː
`node build --compile-css templates/cassiopeia/scss/template.scss`.

    $ node build --compile-css templates/cassiopeia/scss/template.scss
    Prefixing for:  last 1 version
    Without `from` option PostCSS could generate wrong source map and will not find Browserslist config. Set it to CSS file path or to `undefined` to prevent this warning.
    template.css was updated.

Opmerking:

- De bestandsextensie .scss moet in kleine letters zijn.
- Het css bestand moet worden gemaakt.

## Compileren van uw eigen SCSS bestanden voor uw template

Om scss-bestanden voor uw eigen template te compileren, moet u een kopie
van het script `JOOMLA4/build/build-modules-js/compilecescss.js`
gebruiken en aanpassen aan uw omgeving.

Als alternatief kunt u een SCSS Compiler-plug-in gebruiken die uw
`.scss` -bestanden automatisch compileert bij het opnieuw laden van de
pagina. In de Joomlaǃ Extension Directory JED vindt u hiervoor een
plug-inː <a
href="https://extensions.joomla.org/extensions/extension/?searchall=scss&amp;filter%5Btags%5D%5B%5D=&amp;filter%5Bcore_catid%5D=&amp;filter%5Bincludes%5D=&amp;filter%5Bversions%5D=&amp;filter%5Btype%5D=&amp;filter%5Bhasdemo%5D=&amp;filter%5Bnewupdated%5D=&amp;filter%5Bscore%5D=&amp;dir=DESC&amp;limitstart=&amp;controller=filter&amp;view=extension&amp;layout=list&amp;Itemid=145&amp;clearorders=0&amp;clearfilters=1"
class="external text" target="_blank" rel="noreferrer noopener">Joomla!
Extension Directory</a>

Meer alternatieven en hulpmiddelen kunt u vinden op
<a href="http://sass-lang.com/install" class="external free"
target="_blank"
rel="nofollow noreferrer noopener">http://sass-lang.com/install</a>

De SCSS compiler die gebruikt is voor Joomla! 4.x core is
<a href="https://github.com/sass/node-sass" class="external text"
target="_blank" rel="nofollow noreferrer noopener">node-sass</a>. In de
Joomlaǃ core kunt u hem vinden in de map
`JOOMLA4/node_modules/node-sass`.

## Verschil tussen CSS, SCSS en Sass

`SCSS` of `Sass` vereenvoudigt CSS-notatie en definieert variabelen. Dit
zorgt voor een duidelijkere presentatie, minder code en een eenvoudiger
revisie door het gebruik van variabelen.

### CSS

Met CSS wordt code geschreven zoals hieronder, in volledige lengteː

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

### Sass

Sass is een CSS preprocessor met syntax verbeteringen. Style sheets in
de uitgebreide syntaxis worden verwerkt door de preprocessor en omgezet
in gewone CSS-style sheets.

Er zijn twee syntaxis beschikbaar voor Sass: `SCSS` en `Sass`.

#### SCSS

`SCSS` wordt gebruikt in de Joomlaǃ core. `SCSS` is een uitbreiding van
de syntax van CSS.

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

#### Sass

De oudere `Sass` syntax biedt een meer beknopte manier om CSS te
schrijven.

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

U kunt meer informatie vinden in de <a
href="http://sass-lang.com/documentation/file.SASS_REFERENCE.html#syntax"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">Sass Documentatie</a>.

## Van bestaande CSS naar SCSS / CSS-bestanden importeren

Misschien wilt u uw bestaande CSS-bestanden en classes toevoegen aan uw
op SCSS gebaseerde Cassiopeia template of beginnen met wat u hebt. Alle
CSS declarations zijn compatibel met SCSS, dus u kunt uw `.css`
bestanden gewoon hernoemen naar `.scss` en u kunt ze compileren en
gebruiken. U kunt vervolgens stap voor stap gebruikmaken van de functies
die SCSS te bieden heeft:

- Taal extensies zoals variabelen, nesting en mixins
- Veel nuttige functies voor het manipuleren van kleuren en andere
  waarden
- Geavanceerde functies zoals besturingsrichtlijnen voor bibliotheken
- Goed opgemaakte, aanpasbare uitvoer

Zie
<a href="https://sass-lang.com/" class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://sass-lang.com/</a>

Opmerking: Wanneer u de Cassiopeia template wilt aanpassen is het een
goed om een kopie van de template te maken en deze aan te passen -
hierdoor zal Joomla! uw aanpassingen niet overschrijven bij een update.

### Importeer uw .css als .scss bestanden

Laten we nu aannemen dat u uw aangepaste bestanden wilt opnemen en ze
wilt laten verwerken door de SCSS-compiler - u hoeft uw .css NIET naar
SCSS te herschrijven omdat gewone `.css` ook werkt. Het enige dat u
hoeft te doen is

- uw `.css` bestanden hernoemen naar `.scss` en
- een `_` voor de naam zetten en
- een `@import` statement toevoegen boven de genoemde hoofd
  `templates/cassiopeia/scss/template.scss`

We gaan er vanuit dat u uw aangepaste `mystyles.css` bestand hernoemd
hebt naar `_mystyles.scss` in de map `/templates/cassiopeia/scss`. Open
nu `/templates/cassiopeia/scss/template.scss` en plaatst de verwijzing
naar uw `_mystyles.scss` bestand onder aan in het bestand zodat het
eerdere declaraties overschrijft.

    // Bootstrap functions
    @import "../../../media/vendor/bootstrap/scss/functions";


    // Variables
    @import "variables";

    // Flying Focus
    @import "../../../media/vendor/flying-focus-a11y/scss/flying-focus";

    // Bootstrap
    @import "../../../media/vendor/bootstrap/scss/variables";
    @import "../../../media/vendor/bootstrap/scss/bootstrap";

    // FontAwesome
    @import "../../../media/vendor/font-awesome/scss/font-awesome";

    // B/C for Icomoon
    @import "../../../media/system/scss/icomoon";

    // Alert
    @import "../../../media/system/scss/jalert";

    // Blocks
    @import "blocks/global"; // Leave this first
    @import "blocks/alerts";
    @import "blocks/banner";
    @import "blocks/demo-styling";
    @import "blocks/footer";
    @import "blocks/form";
    @import "blocks/frontend-edit";
    @import "blocks/header";
    @import "blocks/icons";
    @import "blocks/iframe";
    @import "blocks/layout";
    @import "blocks/modals";
    @import "blocks/modifiers";
    @import "blocks/utilities";
    @import "blocks/css-grid"; // Last to allow fallback

    // Vendor overrides
    @import "vendor/awesomplete";
    @import "vendor/bootstrap/buttons";
    @import "vendor/bootstrap/card";
    @import "vendor/bootstrap/custom-forms";
    @import "vendor/bootstrap/collapse";
    @import "vendor/bootstrap/dropdown";
    @import "vendor/bootstrap/lists";
    @import "vendor/bootstrap/modal";
    @import "vendor/bootstrap/nav";
    @import "vendor/bootstrap/pagination";
    @import "vendor/bootstrap/table";
    @import "vendor/chosen";
    @import "vendor/dragula";
    @import "vendor/minicolors";
    @import "vendor/tinymce";

    @import "mystyles";

Wanneer u nu uw hoof template.scss bestand compileert houdt u één hoofd
template.css bestand over, geoptimaliseerd en verkleind. Hierdoor
reduceert u ook de http requests waardoor de site sneller zal laden.
