<!-- Filename: J4.x:Namespace_Conventions_In_Joomla / Display title: Naamruimten -->

## Introductie

Het gebruik van **PHP-namespaces** is een manier om gerelateerde klassen, interfaces, functies en constanten te groeperen om naamsconflicten in grotere projecten te vermijden. Ze stellen ontwikkelaars in staat om code effectiever te organiseren, vooral bij het werken met derde partij-bibliotheken of grote codebases die mogelijk dubbele klassenamen bevatten.

Er is meer informatie in het gedeelte [Namespace](jdocmanual?article=docus/namespaces/index) van de Joomla! Programmersdocumentatie.

### Belangrijkste Kenmerken van PHP Namespaces

1. **Vermijd Naamconflicten**:
   - Zonder namespaces, als twee klassen dezelfde naam hebben, zal PHP een foutmelding geven. Namespaces bieden een context om klassen of andere elementen uniek te identificeren.
2. **Organiseer Code**:
   - Namespaces maken het mogelijk om verwante klassen of functies logisch te groeperen, waardoor de code makkelijker te lezen en te onderhouden is.
3. **Toegang via Volledig Gekwalificeerde Namen**:
   - Klassen en functies binnen een namespace kunnen worden benaderd met behulp van hun volledig gekwalificeerde naam, die het namespace-pad bevat.

### Gemeenschappelijke Praktijken:
- Gebruik namespaces om de structuur van je projectmap te spiegelen voor consistentie.
- Importeer namespaces met behulp van het `use` keyword voor schonere code.

## Het Manifestbestand

De eerste verklaring van een extensie-naamruimte bevindt zich in een extensie-manifestbestand, zoals in dit voorbeeld:

```xml
    <namespace path="src">Acme\Component\Wonderful</namespace>
```

De items in deze verklaring zijn als volgt:

- **path="src"** vertelt de class loader dat de genamespaceerde klassen zich in de **src** map van de extensie bevinden, bijvoorbeeld com_wonderful/src.
- **Acme** is het vendorvoorvoegsel. Voor Joomla-core-extensies zou dat *Joomla* zijn. Je moet je eigen unieke vendorvoorvoegsel bedenken.
- **Component** geeft het type extensie aan (component, module, plugin, sjabloon, bibliotheek, ...).
- **Wonderful** is de naam van de extensie. Kies een extensienaam die kort, betekenisvol en waarschijnlijk uniek is.

## Het Klassenbestand

De declaratie van de naamruimte moet de eerste instructie in het bestand zijn waarin het wordt gebruikt. Bijvoorbeeld:

```php
<?php

/**
 * @package     Wonderful
 * @subpackage  Site
 *
 * @copyright   (C) 2024 Merlin. All rights reserved.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace Acme\Component\Wonderful\Controller;

use Joomla\CMS\MVC\Controller\BaseController;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

/**
 * Controller to display the default page - display of wonderful items
 *
 * @since  1.0.0
 */
class MagicController extends BaseController
{
    public function dosomething($wonderful)
    {
        // ... output something wonderful!
    }
}
```

## Het gebruiken van het klasbestand

Wanneer je de klasse moet gebruiken, bijvoorbeeld in een sjabloon, zou je een **use**-verklaring aan de bovenkant van het sjabloon invoegen:

```
use Acme\Component\Wonderful\Controller\MagicController;
```

En instantieer vervolgens de klasse en roep de functie aan:

```
$magic = new MagicController;
$result = $magic->dosomething('wonderful');
```

## Het autoload_psr4.php-bestand

Dit bestand bevindt zich in de administrator/cache-map van de Joomla-site. Het bevat een complete kaart van de namespaces die uit manifestbestanden zijn gehaald. Het wordt gegenereerd bij installatie en opnieuw gegenereerd telkens wanneer een extensie wordt geïnstalleerd of opnieuw geïnstalleerd. Je kunt dit bestand verwijderen en het zal opnieuw worden gegenereerd bij de volgende pagina-lading. Dit is soms noodzakelijk als de installatie van een extensie is geprobeerd met een ongebruikelijke methode. Het bestand bevat 275 of meer paden naar locaties van geklasseerde klassen.

**PSR** is een acroniem voor [**PHP Standards Recommendation**](https://www.php-fig.org/psr/) en [**PSR-4: Autoloader**](https://www.php-fig.org/psr/psr-4/) is een geaccepteerde standaard voor het laden van PHP-klassen.

*Vertaald door openai.com*

