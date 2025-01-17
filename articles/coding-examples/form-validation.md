<!-- Filename: J4.x:Joomla_4_Tips_and_Tricks:_Form_Validation_Basics / Display title: Formulier Validatie -->

## Introductie

Joomla heeft een client-side validatiescript dat door elke component kan worden gebruikt en uitgebreid. Dit artikel gaat over hoe het werkt en hoe je het kunt gebruiken. Er zijn vier standaardvalidators voor gebruikersnaam-, wachtwoord-, numerieke en e-mailveldtypen. Er is ook een meer algemene patroonvalidator en een verplichte veldvalidator.

Merk op dat moderne browsers standaard ook formuliercontrole bieden. Browsercontrole kan worden uitgeschakeld door novalidate in het formulierelement op te nemen.

## Hoe validatie aan te roepen

Aan het begin van het bestand edit.php dat de code voor het genereren van het formulier bevat, voeg de oproep toe om het validator-javascriptbestand te laden:

```php
/** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
$wa = $this->document->getWebAssetManager();
$wa->useScript('keepalive')
    ->useScript('form.validate');
```

in de form-tag zorg ervoor dat je class="form-validate" opneemt. Dit voorbeeld komt uit het gebruikersbewerkingsformulier:

```html
<form action="<?php echo Route::_('index.php?option=com_users&layout=edit&id=' . (int) $this->item->id); ?>"
    method="post" name="adminForm" id="user-form"
    enctype="multipart/form-data"
    aria-label="<?php echo Text::_('COM_USERS_USER_FORM_' . ( (int) $this->item->id === 0 ? 'NEW' : 'EDIT'), true); ?>"
    class="form-validate">
```

In het formulier XML-bestand de uitspraken opnemen die individuele veldvalidatie aanroepen. Dit voorbeeld is het e-mailveld van de gebruiker:

```xml
        <field
            name="email"
            type="email"
            label="JGLOBAL_EMAIL"
            required="true"
            size="30"
            validate="email"
            validDomains="com_users.domains"
        />
```

## De validatie-expressies

Dit gedeelte is bedoeld om een klein begrip van de validatie-expressies te geven om het gebruik toe te lichten. Allemaal zijn opgenomen in de validator.js-code.

### Gebruikersnaam

```php
this.setHandler('username', value => {
      const regex = new RegExp('[<|>|"|\'|%|;|(|)|&]', 'i');
      return !regex.test(value);
    });
```

Hier zoekt de expressie naar het voorkomen van een van de alternatieve tekens binnen de tekenklasse [] en retourneert false (niet geldig) als er een wordt gevonden.

### Wachtwoord

```pnp
this.setHandler('password', value => {
      const regex = /^\S[\S ]{2,98}\S$/;
      return regex.test(value);
    });
```

Hier zoekt de expressie naar een eerste teken dat geen witruimte is, gevolgd door 2 tot 98 tekens die ofwel geen witruimte zijn of een spatie, en eindigend met een teken dat geen witruimte is. Joomla heeft een meer verfijnde wachtwoordcontrole die zorgt voor een mix van verschillende tekentypen en een minimale lengte.

### E-mail

```php
this.setHandler('email', value => {
      const newValue = punycode.toASCII(value);
      const regex = /^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/;
      return regex.test(newValue);
    }); // Attach all forms with a class 'form-validate'
```

Hier accepteert de reguliere expressie elke string die begint met een of meer van de karakters in de [] karakterklasse, gevolgd door @, gevolgd door een of meer karakters in de tweede [] karakterklasse, gevolgd door nul of meer punten gevolgd door een of meer karakters in de derde karakterklassegroep, en eindigend met een van de groepen na de @.

Hoewel technisch correct, laat deze validator enkele nogal gekke e-mailadressen toe, zoals !@me - je zou een meer restrictief patroon kunnen maken.

### Numeriek

```php
this.setHandler('numeric', value => {
      const regex = /^(\d|-)?(\d|,)*\.?\d*$/;
      return regex.test(value);
    });
```

Hier moet de waarde beginnen met een cijfer of een minteken nul of één keer, gevolgd door een cijfer of een komma nul of meer keer, gevolgd door een punt nul of één keer, eindigend met een cijfer nul of meer keer. Dus dit zal overeenkomen met 3.142 of -10 of 100,000.0 enzovoort. Het daadwerkelijke nummer moet overeenkomen met het veldtype in de database. De expressie komt niet overeen met getallen met exponenten, dus 10e2 zal worden afgewezen.

### Patroon

Stel dat je wilt dat een invoerveld een positief geheel getal is. Je zou je eigen patroon kunnen gebruiken dat in het XML-bestand van het formulier is geplaatst:

```xml
        <field
            name="number"
            type="text"
            label="COM_MYCOMPONENT_CAMP_NUMBER_LABEL"
            description="COM_MYCOMPONENT_CAMP_NUMBER_DESC"
            class="w-auto"
            required="true"
            pattern="\d+"
        />
```

Hier staat het patroon één of meer cijfers toe. Dus -1 zou ongeldig zijn, net als 3.142.

### Vereist

Zorg ervoor dat required="true" in de veldspecificatie is opgenomen, anders zal het formulierlabel de veelgebruikte * markering weglaten die wordt gebruikt om verplichte velden aan te geven. Het opnemen van required in de klasse is niet voldoende voor validatie.

*Vertaald door openai.com*

