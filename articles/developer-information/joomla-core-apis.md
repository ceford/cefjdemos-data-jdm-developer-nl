<!-- Filename: J4.x:Joomla_Core_APIs / Display title: Joomla Core API's -->

Deze pagina somt de eindpunten op die beschikbaar zijn in Joomla aan de hand van voorbeelden van curl-commando's. Het is voorbereid voor Joomla 4 en vereist controle op conformiteit met de huidige Joomla-versies.

Elke URL vereist authenticatie, tenzij deze is aangewezen als een openbare URL. Voor 
de veiligheid in Joomla 4.0.0 zijn we van plan de standaard Api Applicatie 
een Super User account te laten vereisen (aangezien de API Applicatie helemaal nieuw is), dit 
kan worden versoepeld naarmate de API stabiliseert en goed getest is in de 
gemeenschap. Als je de basis authenticatie plugin gebruikt (momenteel de enige meegeleverde plugin vanaf Joomla 4 alpha 10) vereist het 
de toevoeging aan de curl-commando’s hieronder met --user gebruikersnaam:password

Elke URL moet voorafgegaan worden door de Joomla-host voor het pad (bijv. in plaats van `/api/index.php/v1/article` heb je `http://example.com/api/index.php/v1/article` nodig).

Eventuele namen van eigenschappen tussen accolades ({}) geven aan dat de eigenschap een variabele is die moet worden vervangen.

Tenzij anders vermeld, werden deze API's geïntroduceerd in Joomla 4. Voor meer informatie over de Joomla API-specificatie (in tegenstelling tot deze lijst van URL's en opties) bezoek dan de [Joomla Api Specification](https://docs.joomla.org/Joomla_Api_Specification).

Waar eindpunten de waarde van {app} vereisen, neemt de variabele momenteel twee waarden aan: site (front end) of administrator (back end).

## Nuttige bronnen

Er zijn een aantal gratis bronnen gecreëerd om Joomla Webdiensten te introduceren en u te helpen bij het leren hoe Webdiensten in uw project te implementeren.

- Postman-verzameling van [Joomla Web Services API](https://github.com/alexandreelise/j4x-api-collection)-aanroepen door Alexandre Elise.
- YouTube Joomla 4 tutorial: [Het gebruiken van de Web Services API](https://www.youtube.com/watch?v=lT9qodsvfZg) met Eoin Oliver.
- Joomla Community Magazine: [Joomla Web Services API 101](https://magazine.joomla.org/all-issues/august-2020/joomla-web-services-api-101-tokens,-testing-and-a-taste-test) door Patrick Jackson.

## Banners-component

### Banners

#### Verkrijg Lijst van Banners

curl -X GET /api/index.php/v1/banners

#### Verkrijg Enkelvoudige Banner

curl -X GET /api/index.php/v1/banners/{banner_id}

#### Banner verwijderen

curl -X DELETE /api/index.php/v1/banners/{banner_id}

#### Banner maken

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/banners -d
```

```json
{
    "catid": 3,
    "clicks": 0,
    "custombannercode": "",
    "description": "Tekst",
    "metakey": "",
    "name": "Naam",
    "params": {
        "alt": "",
        "height": "",
        "imageurl": "",
        "width": ""
    }
}
```

#### Banner bijwerken

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/{banner_id} -d

```json
{
    "alias": "naam",
    "catid": 3,
    "description": "Nieuwe Tekst",
    "name": "Nieuwe Naam"
}
```

### Cliënten

#### Verkrijg lijst van klanten

curl -X GET /api/index.php/v1/banners/klanten

#### Haal Enkele Cliënt Op

curl -X GET /api/index.php/v1/banners/clients/{klant_id}

#### Klant Verwijderen

curl -X DELETE /api/index.php/v1/banners/clients/{klant_id}

#### Client aanmaken

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/banners/klanten -d

```json
{
    "contact": "Naam",
    "email": "email@mail.com",
    "extrainfo": "",
    "metakey": "",
    "name": "Klanten",
    "state": 1
}
```

#### Client Bijwerken

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/clients/{client_id} -d

```
{
    "contact": "nieuwe naam",
    "email": "nieuwemail@mail.com",
    "name": "Klanten"
}
```

### Bannercategorieën

#### Lijst van Categorieën ophalen

curl -X GET /api/index.php/v1/banners/categorieën

#### Haal één categorie op

curl -X GET /api/index.php/v1/banners/categorieën/{category_id}

#### Categorie verwijderen

```
curl -X DELETE /api/index.php/v1/banners/categories/{category_id}
```

#### Categorie Maken

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/banners/categorieën -d

```json
{
    "access": 1,
    "alias": "kat",
    "extension": "com_banners",
    "language": "*",
    "note": "",
    "parent_id": 1,
    "published": 1,
    "title": "Titel"
}
```

#### Categorie bijwerken

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/categories/{category_id} -d

```json
{
    "alias": "kat",
    "note": "Enige tekst",
    "parent_id": 1,
    "title": "Nieuwe titel"
}
```

### Geschiedenis van Bannerinhoud

#### Lijst van inhoudgeschiedenissen ophalen

curl -X GET /api/index.php/v1/banners/contenthistory/{banner_id}

#### Schakel de Geschiedenis van Bewaard Inhoud in/uit

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/banners/contenthistory/bewaren/{contenthistory_id}

#### Geschiedenis inhoud verwijderen

curl -X DELETE
/api/index.php/v1/banners/contenthistory/{contenthistory_id}

## Configuratiecomponent

### Applicatie

#### Verkrijg lijst met applicatieconfiguraties

curl -X GET /api/index.php/v1/config/toepassing

#### Applicatieconfiguratie bijwerken

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/config/application -d
```

{
    "debug": true,
    "sitenaam": "123"
}

### Component

#### Verkrijg lijst van componentconfiguraties

```markdown
curl -X GET /api/index.php/v1/config/{component_name}
```

Voorbeeld “component_name” is “com_content”.

#### Werk Component Applicatieconfiguratie Bij

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/config/toepassing -d

{
        "link_titles": 1
    }

## Contactcomponent

### Contact opnemen

#### Lijst met contacten ophalen

curl -X GET /api/index.php/v1/contact

#### Enkele contactpersoon ophalen

curl -X GET /api/index.php/v1/contact/{contact_id}

#### Contact verwijderen

curl -X DELETE /api/index.php/v1/contact/{contact_id}

#### Contact maken

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/contact -d

```json
{
    "alias": "contact",
    "catid": 4,
    "language": "*",
    "name": "Contact"
}
```

#### Contact bijwerken

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/contact/{contact_id} -d

```json
{
    "alias": "contact",
    "catid": 4,
    "name": "Nieuw Contact"
}
```

#### Contactformulier Verzenden

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/contact/formulier/{contact_id} -d

```json
{
    "contact_email": "email@mail.com",
    "contact_message": "enige tekst",
    "contact_name": "naam",
    "contact_subject": "onderwerp"
}
```

### Contactcategorieën

1. Route Contactcategorieën is: "v1/contact/categories"
2. Werken ermee is vergelijkbaar met Banners Categorieën.

### Velden Contact

#### Verkrijg lijst van velden contact

curl -X GET /api/index.php/v1/velden/contact/contact

#### Haal Enkelveldcontact Op

curl -X GET /api/index.php/v1/velden/contact/contact/{veld_id}

#### Veld Contact verwijderen

curl -X DELETE /api/index.php/v1/velden/contact/contact/{veld_id}

#### Veldcontact maken

Het lijkt erop dat er geen tekst is om te vertalen, omdat dit een codeblok is en instructies zijn om codeblokken niet te vertalen.

```json
{
    "access": 1,
    "context": "com_contact.contact",
    "default_value": "",
    "description": "",
    "group_id": 0,
    "label": "contactveld",
    "language": "*",
    "name": "contact-veld",
    "note": "",
    "params": {
        "class": "",
        "display": "2",
        "display_readonly": "2",
        "hint": "",
        "label_class": "",
        "label_render_class": "",
        "layout": "",
        "prefix": "",
        "render_class": "",
        "show_on": "",
        "showlabel": "1",
        "suffix": ""
    },
    "required": 0,
    "state": 1,
    "title": "contactveld",
    "type": "text"
}
```

#### Veld Contact bijwerken

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/velden/contact/contact/{veld_id} -d

{  
    "title": "nieuw contactveld",  
    "name": "contact-veld",  
    "label": "contactveld",  
    "default_value": "",  
    "type": "tekst",  
    "note": "",  
    "description": "Enige nieuwe tekst"  
}

### Velden Contactmail

1. Route Veld Contact Mail is: "v1/fields/contact/mail"
2. Werken hiermee is vergelijkbaar met Velden Contact.

### Velden Contactcategorieën

1. Routevelden Contactcategorieën is: "v1/fields/contact/categories"
2. Werken ermee is vergelijkbaar met Velden Contact.

### Groepen Velden Contact

#### Lijst met groepsvelden contactpersoon ophalen

`curl -X GET /api/index.php/v1/velden/groepen/contact/contact`

#### Haal Veld Contact van Één Groep Op

curl -X GET /api/index.php/v1/velden/groepen/contact/contact/{groep_id}

#### Contact groepsvelden verwijderen

curl -X DELETE
/api/index.php/v1/velden/groepen/contact/contact/{groeps_id}

#### Groepsvelden Contacten maken

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/velden/groepen/contact/contact -d
```

```json
{
    "access": 1,
    "context": "com_contact.contact",
    "default_value": "",
    "description": "",
    "group_id": 0,
    "label": "contact veld",
    "language": "*",
    "name": "contact-veld3",
    "note": "",
    "params": {
        "class": "",
        "display": "2",
        "display_readonly": "2",
        "hint": "",
        "label_class": "",
        "label_render_class": "",
        "layout": "",
        "prefix": "",
        "render_class": "",
        "show_on": "",
        "showlabel": "1",
        "suffix": ""
    },
    "required": 0,
    "state": 1,
    "title": "contact veld",
    "type": "tekst"
}
```

#### Groepsvelden Contact bijwerken

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/velden/groepen/contact/contact/{group_id} -d
```

```json
{
    "title": "nieuwe contactgroep",
    "note": "",
    "description": "nieuwe beschrijving"
}
```

### Groepsvelden Contact Mail

1. Route Group Velden Contact Mail is: "v1/fields/groups/contact/mail"
2. Werken ermee is vergelijkbaar met Groep Velden Contact.

### Groepsvelden Contactcategorieën

1.  Route Group Velden Categorieën Contact is: "v1/fields/groups/contact/categories"
2.  Werken ermee is vergelijkbaar met Groep Velden Contact.

### Geschiedenis van Contactinhoud

1.  Route Contentgeschiedenis is: "v1/contact/contenthistory"
2.  Werken ermee is vergelijkbaar met Banners Contentgeschiedenis.

## Inhoudscomponent

### Artikelen

#### Verkrijg lijst van artikelen

curl -X GET /api/index.php/v1/content/artikelen

#### Enkel Artikel Opvragen

curl -X GET /api/index.php/v1/content/articles/{artikel_id}

#### Artikel Verwijderen

curl -X DELETE /api/index.php/v1/content/artikelen/{artikel_id}

#### Artikel maken

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/content/articles -d

```json
{
    "alias": "mijn-artikel",
    "articletext": "Mijn tekst",
    "catid": 64,
    "language": "*",
    "metadesc": "",
    "metakey": "",
    "title": "Hier is een artikel"
}
```

Momenteel zijn de hier vermelde opties verplichte eigenschappen. Het is echter de bedoeling om TENMINSTE metakey en metadesc optioneel te maken in de API.

#### Artikel bijwerken

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/content/artikelen/{artikel_id} -d

{
    "catid": 64,
    "title": "Bijgewerkt artikel"
}

### Inhoudscategorieën

1. Route-inhoudscategorieën is: "v1/content/categories"
2. Het werken ermee is vergelijkbaar met het werken met banner-categorieën. Opmerking: als workflows zijn ingeschakeld, dan is het specificeren van een workflow vereist (net als bij de UI)

### Veldartikelen

1. Routeveldenartikelen is: "v1/fields/content/articles"
2. Werken ermee is vergelijkbaar met veldencontact.

### Groepen Velden Artikelen

1. Routegroepenveldenartikelen is: "v1/fields/groups/content/articles"
2. Werken ermee is vergelijkbaar met Groepenveldencontact.

### Veldcategorieën

1.  Routen Velden Categorieën is: "v1/fields/groups/content/categories"
2.  Werken ermee is vergelijkbaar met Velden Contact.

### Inhoudscomponent Inhoudsgeschiedenis

1.  Route-inhoudgeschiedenis is:
    "v1/content/articles/{article_id}/contenthistory"
2.  Werken ermee is vergelijkbaar met de geschiedenis van bannersinhoud.

## Talencomponent

### Talen

#### Verkrijg lijst van talen

curl -X GET /api/index.php/v1/talen

#### Taal installeren

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/talen -d

{
    "package": "pkg_fr-FR"
}

### Inhoudstalen

#### Verkrijg lijst van inhoudstalen

curl -X GET /api/index.php/v1/talen/inhoud

#### Verkrijg één inhoudstaal

curl -X GET /api/index.php/v1/v1/languages/content/{taal_id}

#### Taal van de inhoud verwijderen

curl -X DELETE /api/index.php/v1/talen/inhoud/{language_id}

#### Creëer Inhoudstaal

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/talen/inhoud -d

```json
{
    "access": 1,
    "beschrijving": "",
    "image": "fr_FR",
    "lang_code": "fr-FR",
    "metabeschrijving": "",
    "metatrefwoord": "",
    "volgorde": 1,
    "gepubliceerd": 0,
    "sef": "fk",
    "sitenaam": "",
    "titel": "Frans (FR)",
    "titel_native": "Français (France)"
}
```

#### Taal van de inhoud bijwerken

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/talen/inhoud/{taal_id} -d

```json
{
    "description": "",
    "lang_code": "nl-NL",
    "metadesc": "",
    "metakey": "",
    "sitename": "",
    "title": "Nederlands (nl-NL)",
    "title_native": "Nederlands (Nederland)"
}
```

### Overschrijft Talen

#### Verkrijg Lijst van Overrides Taalconstanten

curl -X GET /api/index.php/v1/languages/overrides/{app}/{lang_code}

#### Haal enkele overschrijvings-taalconstante op

curl -X GET
/api/index.php/v1/talen/overschrijvingen/{app}/{taalcode}/{constante_id}

#### Inhoudstaaloverschrijvingen verwijderen

curl -X DELETE
/api/index.php/v1/talen/overschrijvingen/{app}/{taal_code}/{constante_id}

#### Taaloverrides voor content creëren

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/talen/overstijgingen/{app}/{lang_code} -d

{
        "key":"nieuwe_sleutel",
        "override": "tekst"
    }

#### Inhoud van taaloverschrijvingen bijwerken

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/talen/overschrijvingen/{app}/{lang_code}/{constant_id} -d

{
        "key":"new_key",
        "override": "nieuwe tekst"
    }

``` 
var app - enum {"site", "beheerder"}
```

var lang_code - string Voorbeeld: “fr-FR“, “en-GB“ je kunt lang_code krijgen
van v1/languages/content

#### Zoek Override Constante

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/talen/overschrijvingen/zoeken -d

```json
{
    "searchstring": "JLIB_APPLICATION_ERROR_SAVE_FAILED",
    "searchtype": "constante"
}
```

var zoektype - enum {“constant”, “waarde”}. “constant” zoeken op constante naam, “waarde” - zoeken op constante waarde

#### Vernieuw Override Zoeken Cache

```
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/languages/overrides/search/cache/refresh
```

## Menu's Component

### Menu's

#### Lijst van Menu's ophalen

curl -X GET /api/index.php/v1/menus/{app}

#### Enkel Menu Ophalen

curl -X GET /api/index.php/v1/menu's/{app}/{menu_id}

#### Verwijder Menu

curl -X DELETE /api/index.php/v1/menu's/{app}/{menu_id}

#### Menu Maken

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/menus/{app} -d

```json
{
    "client_id": 0,
    "description": "Het menu voor de site",
    "menutype": "menu",
    "title": "Menu"
}
```

#### Menu bijwerken

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/menus/{app}/{menu_id} -d

```json
{
    "menutype": "menu",
    "title": "Nieuw Menu"
}
```

### Menu-items

#### Verkrijg lijst van menutypen

curl -X GET /api/index.php/v1/menus/{app}/items/types

#### Verkrijg Lijst van Menu-items

curl -X GET /api/index.php/v1/menu/{app}/items

#### Haal één menu-item op

curl -X GET /api/index.php/v1/menus/{applicatie}/items/{menu_item_id}

#### Verwijder Menu-item

curl -X DELETE /api/index.php/v1/menus/{app}/items/{menu_item_id}

#### Menu-item maken

```shell
curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/menus/{app}/items -d
```

```json
{
    "toegang": "1",
    "alias": "",
    "associaties": {
        "en-GB": "",
        "fr-FR": ""
    },
    "browserNav": "0",
    "component_id": "20",
    "home": "0",
    "taal": "*",
    "link": "index.php?option=com_content&view=form&layout=edit",
    "menutype": "hoofdmenu",
    "notitie": "",
    "parameters": {
        "annuleer_redirect_menuitem": "",
        "catid": "",
        "custom_annuleer_redirect": "0",
        "enable_categorie": "0",
        "menu-anker_css": "",
        "menu-anker_titel": "",
        "menu-meta_beschrijving": "",
        "menu-meta_trefwoorden": "",
        "menu_afbeelding": "",
        "menu_afbeelding_css": "",
        "menu_tonen": "1",
        "menu_tekst": "1",
        "pagina_kop": "",
        "pagina_titel": "",
        "pagina_klasse_sfx": "",
        "redirect_menuitem": "",
        "robots": "",
        "toon_pagina_kop": ""
    },
    "parent_id": "1",
    "publiceren_omlaag": "",
    "publiceren_omhoog": "",
    "gepubliceerd": "1",
    "sjabloonstijl_id": "0",
    "titel": "titel",
    "toggle_modules_toegewezen": "1",
    "toggle_modules_gepubliceerd": "1",
    "type": "component"
}
```

Voorbeeld voor "Artikelpagina maken"

#### Menu-item bijwerken

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/menu's/{app}/items/{menu_item_id} -d

```json
{
    "component_id": "20",
    "taal": "*",
    "link": "index.php?option=com_content&view=form&layout=edit",
    "menutype": "hoofdmenu",
    "notitie": "",
    "titel": "nieuwe titel",
    "type": "component"
}
```

Voorbeeld voor "Artikelpagina maken"

## Berichtencomponent

### Berichten

#### Verkrijg lijst van berichten

curl -X GET /api/index.php/v1/berichten

#### Haal Eén Bericht Op

curl -X GET /api/index.php/v1/berichten/{bericht_id}

#### Bericht verwijderen

curl -X DELETE /api/index.php/v1/berichten/{bericht_id}

#### Bericht maken

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/berichten -d

{
    "bericht": "tekst",
    "toestand": 0,
    "onderwerp": "tekst",
    "gebruiker_id_van": 773,
    "gebruiker_id_naar": 772
}

#### Bericht bijwerken

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/messages/{bericht_id} -d

{
    "boodschap": "nieuwe tekst",
    "onderwerp": "nieuwe tekst",
    "gebruiker_id_van": 773,
    "gebruiker_id_naar": 772
}

## Modules Component

### Modules

#### Lijst met Moduletypen ophalen

curl -X GET /api/index.php/v1/modules/types/{app}

#### Modulelijst verkrijgen

curl -X GET /api/index.php/v1/modulen/{app}

#### Verkrijg Enkel Module

curl -X GET /api/index.php/v1/modules/{app}/{module_id}

#### Module Verwijderen

curl -X DELETE /api/index.php/v1/modules/{app}/{module_id}

#### Module maken

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/modules/{app} -d

```json
{
    "toegang": "1",
    "toegewezen": [
        "101",
        "105"
    ],
    "toewijzing": "0",
    "klant_id": "0",
    "taal": "*",
    "module": "mod_articles_archive",
    "notitie": "",
    "volgorde": "1",
    "parameters": {
        "bootstrap_grootte": "0",
        "cache": "1",
        "cache_tijd": "900",
        "cachemodus": "statisch",
        "aantal": "10",
        "header_klasse": "",
        "header_tag": "h3",
        "indeling": "_:default",
        "module_tag": "div",
        "moduleklasse_sfx": "",
        "stijl": "0"
    },
    "positie": "",
    "publiceer_omlaag": "",
    "publiceer_omhoog": "",
    "gepubliceerd": "1",
    "toon_titel": "1",
    "titel": "Titel"
}
```

Voorbeeld voor "Artikelen - Gearchiveerd"

#### Module bijwerken

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/modules/{app}/{module_id} -d
```

```json
{
    "access": "1",
    "client_id": "0",
    "language": "*",
    "module": "mod_articles_archive",
    "note": "",
    "ordering": "1",
    "title": "Nieuwe Titel"
}
```

Voorbeeld voor "Artikelen - Gearchiveerd"

## Nieuwsfeeds-component

### Feeds

#### Lijst van feeds ophalen

curl -X GET /api/index.php/v1/newsfeeds/feeds

#### Haal Enkele Feed Op

curl -X GET /api/index.php/v1/newsfeeds/feeds/{feed_id}

#### Feed Verwijderen

```
curl -X DELETE /api/index.php/v1/newsfeeds/feeds/{feed_id}
```

#### Feed maken

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/nieuwsfeeds/feeds -d

```json
{
    "toegang": 1,
    "alias": "alias",
    "catid": 5,
    "beschrijving": "",
    "afbeeldingen": {
        "zweven_eerste": "",
        "zweven_tweede": "",
        "afbeelding_eerste": "",
        "afbeelding_eerste_alt": "",
        "afbeelding_eerste_onderschrift": "",
        "afbeelding_tweede": "",
        "afbeelding_tweede_alt": "",
        "afbeelding_tweede_onderschrift": ""
    },
    "taal": "*",
    "koppeling": "http://samoylov/joomla/gsoc19_webservices/index.php",
    "metadata": {
        "hits": "",
        "rechten": "",
        "robots": "",
        "tags": {
            "tags": "",
            "typeAlias": null
        }
    },
    "metabeschrijving": "",
    "metasleutel": "",
    "naam": "Naam",
    "volgorde": 1,
    "parameters": {
        "feed_teken_telling": "",
        "feed_weergave_volgorde": "",
        "nieuwsfeed_indeling": "",
        "toon_feed_beschrijving": "",
        "toon_feed_afbeelding": "",
        "toon_item_beschrijving": ""
    },
    "gepubliceerd": 1
}
```

#### Update Feed

```
curl -X PATCH -H "Content-Type: application/json" /api/index.php/v1/newsfeeds/feeds/{feed_id} -d
```

{
    "toegang": 1,
    "alias": "test2",
    "catid": 5,
    "beschrijving": "",
    "link": "http://samoylov/joomla/gsoc19_webservices/index.php",
    "metabeschrijving": "",
    "metasleutel": "",
    "naam": "Test"
}

### Nieuwsfeed Categorieën

1. De route Nieuwsfeeds Categorieën is: "v1/newsfeeds/categories"
2. Werken hiermee is vergelijkbaar met Banner Categorieën.

## Privacycomponent

### Verzoek

#### Verkrijg Lijst van Verzoeken

curl -X GET /api/index.php/v1/privacy/request

#### Haal Enkele Verzoek Op

curl -X GET /api/index.php/v1/privacy/request/{verzoek_id}

#### Haal Enkele Verzoek Exporteer Gegevens op

curl -X GET /api/index.php/v1/privacy/verzoek/exporteren/{verzoek_id}

#### Verzoek Maken

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/privacy/request -d

{
    "email":"somenewemail@com.ua",
    "request_type":"exporteren"
}

### Toestemming

#### Verkrijg lijst met toestemmingen

curl -X GET /api/index.php/v1/privacy/toestemming

#### Verkrijg Enkelvoudige Toestemming

curl -X GET /api/index.php/v1/privacy/toestemming/{toestemming_id}

#### Toestemming Verwijderen

curl -X DELETE /api/index.php/v1/privacy/toestemming/{consent_id}

## Redirect Component

### Omleiding

#### Verkrijg Lijst van Redirects

curl -X GET /api/index.php/v1/omleiding

#### Verkrijg Enkele Omleiding

curl -X GET /api/index.php/v1/redirect/{redirect_id}

#### Verwijder Redirect

curl -X DELETE /api/index.php/v1/redirect/{redirect_id}

#### Omleiding Maken

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/doorverwijzen -d

{
        "commentaar": "",
        "koptekst": 301,
        "aantal_hits": 0,
        "nieuwe_url": "/content/art/99",
        "oude_url": "/content/art/12",
        "gepubliceerd": 1,
        "verwijzer": ""
    }

#### Doorsturen bijwerken

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/redirect/{redirect_id} -d
```

{
        "new_url": "/content/art/4",
        "old_url": "/content/art/132"
    }

## Tags-component

### Tags

#### Verkrijg lijst van tags

curl -X GET /api/index.php/v1/tags

#### Haal enkele tag op

curl -X GET /api/index.php/v1/tags/{tag_id}

#### Tag Verwijderen

curl -X DELETE /api/index.php/v1/tags/{tag_id}

#### Tag maken

```markdown
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/tags
-d
```

```json
{
    "access": 1,
    "access_title": "Openbaar",
    "alias": "test",
    "description": "",
    "language": "*",
    "note": "",
    "parent_id": 1,
    "path": "test",
    "published": 1,
    "title": "test"
}
```

#### Label Bijwerken

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/tags/{tag_id} -d

{
        "alias": "test",
        "title": "nieuwe titel"
}

## Sjablonen

### Sjablonenstijlen

#### Verkrijg lijst van sjabloonstijlen

curl -X GET /api/index.php/v1/templates/styles/{app}

#### Haal enkele sjabloonstijl op

curl -X GET /api/index.php/v1/templates/styles/{applicatie}/{template_stijl_id}

#### Stijl van sjabloon verwijderen

curl -X DELETE
/api/index.php/v1/sjablonen/stijlen/{app}/{template_style_id}

#### Sjabloonstijl maken

curl -X POST -H "Content-Type: application/json"
/api/index.php/v1/templates/styles/{app} -d

{
    "home": "0",
    "params": {
        "fluidContainer": "0",
        "logoFile": "",
        "sidebarLeftWidth": "3",
        "sidebarRightWidth": "3"
    },
    "template": "cassiopeia",
    "title": "cassiopeia - Wat Tekst"
}

#### Sjabloonstijl bijwerken

curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/templates/stijlen/{app}/{template_style_id} -d

```json
{
    "template": "cassiopeia",
    "title": "nieuwe cassiopeia - Standaard"
}
```

## Gebruikerscomponent

### Gebruikers

#### Lijst van Gebruikers Ophalen

curl -X GET /api/index.php/v1/gebruikers

#### Verkrijg Enkel Gebruiker

curl -X GET /api/index.php/v1/gebruikers/{gebruiker_id}

#### Gebruiker verwijderen

curl -X DELETE /api/index.php/v1/users/{gebruiker_id}

#### Gebruiker Aanmaken

```
curl -X POST -H "Content-Type: application/json" /api/index.php/v1/gebruikers
-d
```

```json
{
    "blok": "0",
    "e-mail": "test@mail.com",
    "groepen": [
        "2"
    ],
    "id": "0",
    "laatsteResetTijd": "",
    "laatsteBezoekDatum": "",
    "naam": "nnn",
    "parameters": {
        "admin_taal": "",
        "admin_stijl": "",
        "editor": "",
        "helpSite": "",
        "taal": "",
        "tijdzone": ""
    },
    "wachtwoord": "qwertyqwerty123",
    "wachtwoord2": "qwertyqwerty123",
    "registratieDatum": "",
    "vereisReset": "0",
    "resetAantal": "0",
    "stuurEmail": "0",
    "gebruikersnaam": "ad"
}
```

#### Gebruiker bijwerken

```
curl -X PATCH -H "Content-Type: application/json"
/api/index.php/v1/users/{user_id} -d
```

{
    "email": "nieuw@mail.com",
    "groups": [
        "2"
    ],
    "name": "naam",
    "username": "gebruikersnaam"
}

### Gebruikersvelden

1. Routevelden Gebruikers is: "v1/fields/users"  
2. Werken ermee is vergelijkbaar met werken met Veld Contact.

### Groepen Velden Gebruikers

1. Route Groups Fields Users is: "v1/fields/groups/users"
2. Werken ermee is vergelijkbaar met Groups Fields Contact.

*Vertaald door openai.com*

