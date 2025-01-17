<!-- Filename: J4.x:MVC_Anatomy:_Administrator_Edit_Files / Display title: MVC Anatomie: Beheerdersbestanden Bewerken -->

## Landgegevensbestanden

De beheerdersbestanden omvatten vier weergaven: twee lijstweergaven voor de gegevens van landen en valuta's en twee bewerkingsweergaven voor het beheren van gegevensinvoer in elk van de twee betrokken tabellen. De lijstweergaven zijn bijna identiek aan de landenlijstweergave beschreven voor de site-interface, dus ze worden hier niet opnieuw behandeld. Evenzo zijn de bewerkingsweergaven bijna identiek, dus er zal slechts één worden behandeld, de gegevensinvoerview voor een enkel land.

## src/Controller/CountryController.php

Dit is enorm eenvoudig! Afgezien van de class-declaratie is het zonder inhoud. Alles wordt verzorgd door de ouderklasse.

```php
    class CountryController extends FormController
    {
    }
```

## src/View/Country/HtmlView.php

Dit is waar gegevens van het model worden opgehaald voor gebruik in het gegevensinvoerformulier. Er is een significant verschil met de eerder beschreven lijstweergave: het is gebruikelijk om een werkbalk met knoppen voor Opslaan, Annuleren, Help, et cetera te gebruiken.

Wanneer je het bewerkingsformulier oproept door een lijstitemtitel te selecteren, bevat de URL een id, bijvoorbeeld option=com_countrybase&view=country&layout=edit&id=1. De id is het tabelrecordnummer. Voor een nieuw record opgeroepen via de Nieuwe knop in de lijst, ontbreekt de id. Als deze aanwezig is, vult het model het gegevensinvoerscherm in met de bestaande recordgegevens.

```php
    public function display($tpl = null)
    {
        $this->form  = $this->get('Form');
        $this->item  = $this->get('Item');
        $this->state = $this->get('State');

        if (count($errors = $this->get('Errors')))
        {
            throw new GenericDataException(implode("\n", $errors), 500);
        }

        $this->addToolbar();

        return parent::display($tpl);
    }
```

### De Werkbalk

De functie addToolbar() wordt meestal gebruikt om de paginatitel in te stellen en geschikte knoppen toe te voegen voor de toegangsrechten van de persoon die het formulier gebruikt. Let op het gebruik van de \$isNew-variabele op basis van het record-id om de uitvoer iets aan te passen.

```php
    protected function addToolbar()
    {
        Factory::getApplication()->input->set('hidemainmenu', true);
        $isNew      = ($this->item->id == 0);

        $canDo = ContentHelper::getActions('com_countrybase');

        $toolbar = Toolbar::getInstance();

        ToolbarHelper::title(
            Text::_('COM_COUNTRYBASE_COUNTRY_PAGE_TITLE_' . ($isNew ? 'ADD' : 'EDIT'))
        );

        if ($canDo->get('core.create')) {
            if ($isNew) {
                $toolbar->apply('country.save');
            } else {
                $toolbar->apply('country.apply');
            }
            $toolbar->save('country.save');
        }
        $toolbar->cancel('country.cancel', 'JTOOLBAR_CLOSE');

        ToolbarHelper::inlinehelp();
    }
```

**Notities:**

- **hidemainmenu** is ingesteld op true voor alle bewerkformulieren om te voorkomen dat je het formulier verlaat via het Administrator-menu zonder op te slaan.
- **ToolbarHelper::title()** stelt de titel in de paginatitelbalk in, in dit geval op Bewerk Land of Voeg Land Toe.
- **toolbar-\>action** knoppen, waarbij action kan zijn opslaan, toepassen, annuleren of een van vele anderen, nemen (controller.function) als argumenten. Dus in dit geval, wanneer een knop is geselecteerd, wordt de actie doorgegeven aan een opslaan of toepassen functie in de CountryController klasse.
- **cancel** neemt een extra argument, de tekenreeks sleutel die wordt gebruikt om de knop te labelen.
- **inlinehelp()** toont een knop die de veldbeschrijvingen onder elk gegevensinvoerveld aan of uit zet.

## forms/land.xml

Dit bestand bevat de specificatie van de formuliervelden, meestal in de volgorde waarin ze in het bewerkingsformulier worden gepresenteerd. De meeste velden spreken voor zich, dus er worden slechts twee hieronder weergegeven. De naamwaarden zijn de kolomnamen die worden gebruikt in de databasetabellen.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<form>
        <field
            name="title"
            type="text"
            label="COM_COUNTRYBASE_COUNTRY_TITLE"
            required="true"
            maxlength="128"
        />

        <field
            name="iso_2"
            type="text"
            label="COM_COUNTRYBASE_COUNTRY_ISO_2"
            description="COM_COUNTRYBASE_COUNTRY_ISO_2_DESC"
            required="true"
            maxlength="3"
        />
```

## src/Model/CountryModel.php

De functies in dit bestand zijn vrij standaard. De functie getTable() vereist echter enige uitleg.

```php
    public function getTable($name = '', $prefix = '', $options = array())
    {
        $name = 'Countries';
        $prefix = 'Table';

        if ($table = $this->_createTable($name, $prefix, $options)) {
            return $table;
        }

        throw new \Exception(Text::sprintf('JLIB_APPLICATION_ERROR_TABLE_NAME_NOT_SUPPORTED', $name), 0);
    }
```

Deze functie verwijst niet naar de tabel zelf! Het verwijst naar de constructor van src/Table/CountriesTable.php. Dit kan een beetje verwarrend zijn omdat het wordt aangeroepen vanuit de CountryModel klasse.

## src/Tabel/LandenTabel.php

Dus dit is waar de tabelnaam voor de landenlijst wordt gedefinieerd.

```php
    class CountriesTable extends Table
    {
        /**
         * Constructor
         *
         * @param   DatabaseDriver  $db  Database connector object
         *
         * @since   1.6
         */
        public function __construct(DatabaseDriver $db)
        {
            parent::__construct('#__countrybase_countries', 'id', $db);

            $this->setColumnAlias('published', 'state');
        }
    }
```

Merk op dat de kolomalias wordt gedeclareerd. Dit wordt gebruikt omdat het formulier een invoerveld met de naam **published** gebruikt, maar het veld dat voor opslag wordt gebruikt, **status** heet.

## tmpl/land/bewerk.php

Dit is het bestand dat de HTML-uitvoer creëert. Het begint met twee helpers: HTMLHelper::_('behavior.formvalidator'); laadt de javascript die nodig is voor client-side formuliercontrole. Hoewel moderne browsers formuliercontrole toepassen, voorkomt dit niet het indienen van formulieren. HTMLHelper::_('behavior.keepalive'); laadt javascript om de server te pingen om sessieverloop te voorkomen tijdens het invullen van complexe formulieren.

**Notities:**

- LayoutHelper::render('joomla.edit.title_alias', \$this); render een standaard Joomla-titelveld.
- \$this-\>form-\>renderField('iso_2'); render het gespecificeerde veld, in dit geval iso_2.
- LayoutHelper::render('joomla.edit.global', \$this); render de velden aan de rechterkant van het Detailstabblad.

```php
<?php

/**
 * @package     Countrybase.Administrator
 * @subpackage  com_countrybase
 *
 * @copyright   (C) 2025 Clifford E Ford
 * @license     GNU General Public License version 3 or later; see LICENSE.txt
 */

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\HTML\HTMLHelper;
use Joomla\CMS\Language\Text;
use Joomla\CMS\Layout\LayoutHelper;
use Joomla\CMS\Router\Route;

HTMLHelper::_('behavior.formvalidator');
HTMLHelper::_('behavior.keepalive');

?>

<form action="<?php echo Route::_('index.php?option=com_countrybase&view=country&layout=edit&id=' .
        (int) $this->item->id); ?>"
    method="post" name="adminForm" id="country-form" class="form-validate">

    <?php echo LayoutHelper::render('joomla.edit.title_alias', $this); ?>

    <div>
        <?php echo HTMLHelper::_('uitab.startTabSet', 'myTab', array('active' => 'details')); ?>

        <?php echo HTMLHelper::_('uitab.addTab', 'myTab', 'details', Text::_('COM_COUNTRYBASE_COUNTRY_TAB_DETAILS')); ?>
        <div class="row">
            <div class="col-md-9">
                <div class="row">
                    <div class="col-md-6">
                        <?php echo $this->form->renderField('iso_2'); ?>
                        <?php echo $this->form->renderField('iso_3'); ?>
                        <?php echo $this->form->renderField('country_code'); ?>
                        <?php echo $this->form->renderField('region_code'); ?>
                        <?php echo $this->form->renderField('subregion_code'); ?>
                        <?php echo $this->form->renderField('phone_prefix'); ?>
                        <?php echo $this->form->renderField('currency_code'); ?>
                        <?php echo $this->form->renderField('id'); ?>
                    </div>
                </div>
            </div>
            <div class="col-md-3">
                <div class="card card-light">
                    <div class="card-body">
                        <?php echo LayoutHelper::render('joomla.edit.global', $this); ?>
                    </div>
                </div>
            </div>
        </div>
        <?php echo HTMLHelper::_('uitab.endTab'); ?>

        <?php echo HTMLHelper::_('uitab.endTabSet'); ?>
    </div>
    <input type="hidden" name="task" value="">
    <?php echo HTMLHelper::_('form.token'); ?>
</form>
```

Je kunt door veldsets van formulieren en velden binnen elke veldset heen fietsen. Dat kan de uitvoer van complexe formulieren met veel tabbladen vereenvoudigen.

![country edit form](../../../en/images/mvc-anatomy/com-countrybase-edit-country.png)

*Vertaald door openai.com*

