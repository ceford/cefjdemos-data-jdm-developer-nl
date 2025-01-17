<!-- Filename: J4.x:MVC_Anatomy:_Site_Files / Display title: MVC-anatomie: Sitebestanden -->

## Bestandsstructuur

Er zijn minder bestanden in het Site-gedeelte van de component dan in het Administrator-gedeelte, dus dit lijkt een goede plek om te beginnen. Alleen die delen van elk bestand die uitleg behoeven, worden behandeld. Het is het beste als je elk genoemd bestand opent, een kijkje neemt naar de algehele inhoud en vervolgens de uitgelegde delen vindt. De alfabetische volgorde van de bestandsstructuur is als volgt:

```bash
    site
    |- forms
        |- filter_countries.xml
    |- language
        |- en-GB
           |- com_countrybase.ini
    |- src
        |- Controller
           |- DisplayController
        |- Model
           |- CountriesModel.php
        |- Service
           |- Router.php
        |- View
           |- Countries
              |- HtmlView.php
    |- tmpl
        |- countries
           |- default.php
           |- default.xml
```

## DisplayContoller.php

```php
<?php

/**
 * @package     Countrybase.Site
 * @subpackage  com_countrybase
 *
 * @copyright   (C) 2025 Clifford E Ford
 * @license     GNU General Public License version 3 or later; see LICENSE.txt
 */

namespace Cefjdemos\Component\Countrybase\Site\Controller;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\MVC\Controller\BaseController;
use Joomla\CMS\Router\Route;
use Joomla\CMS\Session\Session;

/**
 * Countrybase Component Controller
 *
 * @since  1.0.0
 */
class DisplayController extends BaseController
{
    /**
     * The default view.
     *
     * @var    string
     */
    protected $default_view = 'countries';
}
```

Delen van dit bestand worden als volgt uitgelegd:

### Auteursrechtmelding

Elke PHP-bestand zou moeten beginnen met een copyrightmelding zoals de volgende:

```php
<?php

/**
 * @package     Countrybase.Site
 * @subpackage  com_countrybase
 *
 * @copyright   (C) 2025 Clifford E Ford
 * @license     GNU General Public License version 3 or later; see LICENSE.txt
 */
```

Als je vaak nieuwe bestanden aanmaakt en dit gedeelte kopieert/plakt, vergeet dan niet de componentnaam en het auteursrechtbericht bij te werken. Ook vereist de code sniffer een lege regel voor een doc-blok.

### Naamruimte en gedefinieerde controle

Na de copyright-mededeling moet elk php-bestand een regel bevatten met defined('_JEXEC') or die; behalve dat bestanden met een namespace de namespace moeten declareren voordat er andere php-code is, dus vóór de defined-check. Php-bestanden met een namespace zijn die welke component php-klassen bevatten in de src-map of de submappen daarvan.

```php
namespace Cefjemos\Component\Countrybase\Site\Controller;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects
```

De `defined` controle voorkomt dat een php-bestand wordt uitgevoerd door het direct via de url aan te roepen. De `_JEXEC` constante wordt gedefinieerd wanneer de applicatie start via het hoofdindex.php-bestand of het administrator index.php-bestand. Het is een belangrijk beveiligingshulpmiddel.

De `defined` controle is omhuld door `phpcs:disable` en `phpcs:enable` om een schending van de PSR12-coderingsstandaard toe te staan.

Gecombineerd met de namespace die is gedeclareerd in het countrybase.xml manifestbestand, zal Joomla zoeken naar elke klasse die in het huidige bestand is gedeclareerd in root/components/com_countrybase/src/Controller - in dit geval wordt de naam van dit bestand toegevoegd, DisplayController.php.

### gebruik verklaringen

De use-statements volgen meestal na de gedefinieerde check en worden vaak in alfabetische volgorde gerangschikt. De use-statements definiëren de locaties van de klassen die door dit PHP-bestand worden gebruikt. Soms zijn use-statements per ongeluk aanwezig, ze worden gedeclareerd maar niet gebruikt. Dat schaadt niet, maar zou gecorrigeerd moeten worden. Er zijn hier twee niet-gebruikte statements:

```php
    use Joomla\CMS\Router\Route;
    use Joomla\CMS\Session\Session;
```

### Controller Klasse

De displaycontroller heeft bijna niets te doen, aangezien al het werk in de bovenliggende klasse wordt gedaan. Het enige belangrijke dat het doet, is de standaardweergave instellen, in dit geval **landen**. Dat zorgt ervoor dat de standaard componentweergave de bestanden Countries/HtmlView.php en tmpl/countries/default.php gebruikt om de gegevens van de landen weer te geven.

```php
/**
 * Countrybase Component Controller
 *
 * @since  1.0.0 Created for first release.
 */
class DisplayController extends BaseController
{
    /**
     * The default view.
     *
     * @var    string
     */
    protected $default_view = 'countries';
}
```

Het documentatieblok (DocBlock) voorafgaand aan de `class`-verklaring wordt gebruikt voor automatische documentatie met tools zoals *Doxygen*. De `@since`-verklaring is een *tag* die het versienummer aangeeft van de extensie waarin dit element werd geïntroduceerd. Het kan gevolgd worden door een uitleg in gewone tekst. Elk element kan een DocBlock hebben met een willekeurig aantal standaardtags. Het is een goede gewoonte om je eigen code goed gedocumenteerd te houden!

Vergeet niet de \$default_view voor deze controller in te stellen. Zonder dit zal de DisplayController de standaardweergave gebruiken die is gedefinieerd in het componentconfiguratiebestand of, indien dat niet bestaat, de componentnaam.

## src/View/Landen/HtmlView.php

De controller heeft de standaardweergave ingesteld op **landen**, dus de volgende stap is het laden van de bijbehorende HtmlView.php-code. Delen van dit bestand vereisen enige uitleg.

### Klassevariabelen

```php
class HtmlView extends BaseHtmlView
{
    /**
     * The model state
     *
     * @var  \Joomla\CMS\Object\CMSObject
     */
    protected $state = null;
    ...
    protected $items = null;
    ...
    protected $pagination = null;
    ...
    public $filterForm;
    ...
    public $activeFilters;
```

De klassevariabelen worden gebruikt om informatie op te slaan over de pagina die moet worden weergegeven:

- `$state` - de modelstatus, vaak geprefabriceerd door formulier- of querystringinvoer.
- `$items` - de lijst met landgegevens opgehaald uit de database.
- `$pagination` - een object dat wordt gebruikt om een paginanavigatiemechanisme weer te geven als er meer pagina’s zijn dan de lijstlimiet, normaal gesproken 20 items.
- `$filterForm` - meestal niet te vinden op websitepagina's maar gebruikt in com_countrybase om te filteren op landnaam of gepubliceerde status.
- `$activeFilters` - gebruikt om bij te houden welke filters in gebruik zijn.

### weergavefunctie

Dit is vrij standaard voor een siteweergave, afgezien van de onderdelen filterForm en activeFilters:

```php
    public function display($tpl = null)
    {
        $this->state      = $this->get('State');
        $this->items      = $this->get('Items');
        $this->pagination = $this->get('Pagination');
        $this->filterForm    = $this->get('FilterForm');
        $this->activeFilters = $this->get('ActiveFilters');

        // Flag indicates to not add limitstart=0 to URL
        $this->pagination->hideEmptyLimitstart = true;

        // Check for errors.
        if (count($errors = $this->get('Errors')))
        {
            throw new GenericDataException(implode("\n", $errors), 500);
        }

        parent::display($tpl);
    }
```

De uitspraken in de vorm van `$this->get('Xxxx')` zorgen ervoor dat Joomla in `CountriesModel.php` zoekt naar een functie genaamd `getXxxx()` en eventuele gegevens die door die code worden geproduceerd, retourneert voor opslag en gebruik in de weergave. Vaak bevindt de functie zich in de parent van CountriesModel. Bijvoorbeeld, de getItems-functie is niet aanwezig in CountriesModel.php, maar is wel aanwezig in de ListModel die het uitbreidt.

Samenvattend haalt de viewklasse gegevens op uit het model en roept vervolgens de bovenliggende klasse aan om de gegevens weer te geven.

## Model/LandenModel.php

Er is een klein aantal functies in het Model dat je meestal zelf moet aanvullen. Andere worden geërfd van het bovenliggende ListModel.

### __construct

Het is normaal om in de constructor alle filtervelden op te nemen die je mogelijk wilt gebruiken. Zonder deze lijken filters geen effect te hebben. Ze worden vaak vergeten wanneer je later een filter wilt toevoegen. Elk veld wordt gegeven als zijn veldnaam en met een tabelnaamalias, meestal a, b, c enzovoort, maar het kan alles zijn wat je kiest, zolang het consistent is met de code elders.

```php
       public function __construct($config = array())
        {
            if (empty($config['filter_fields']))
            {
                $config['filter_fields'] = array(
                        'id', 'a.id',
                        'title', 'a.title',
                        'iso_2', 'a.iso_2',
                        'iso_3', 'a.iso_3',
                        'country_code', 'a.country_code',
                        'region_code', 'a.region_code',
                        'state', 'a.state',
                        'subregion_code', 'a.subregion_code',
                        'phone_prefix', 'a.phone_prefix',
                        'currency_code', 'a.currency_code',
                );
            }

            parent::__construct($config);
        }
```

### populateState

Deze functie neemt invoerparameters en bereidt ze voor op gebruik in een databasequery. Sommige parameters worden in de ouder verwerkt, dus er is hier niets te doen. Bijvoorbeeld, als het zoekveld **titel** is, wordt dat door de ouder afgehandeld, net als de **status** en paginatievelden **start** en **limiet**.

```php
       protected function populateState($ordering = 'title', $direction = 'ASC')
        {
            // List state information.
            parent::populateState($ordering, $direction);
        }
```

### getStoreId

Deze functie maakt een hash aan om een query elders te gebruiken.

```php
       protected function getStoreId($id = '')
        {
            // Compile the store id.
            $id .= ':' . $this->getState('filter.search');
            $id .= ':' . $this->getState('filter.published');

            return parent::getStoreId($id);
        }
```

### getListQuery

Dit is waar de query wordt gemaakt om gegevens uit de database te halen. Het vergt enig begrip van hoe queries zijn opgebouwd.

```php
    protected function getListQuery()
    {
        // Create a new query object.
        $db = $this->getDatabase();
        $query = $db->getQuery(true);

        // Select the required fields from the table.
        $query->select(
            $this->getState(
                'list.select',
                [
                    $db->quoteName('a.title'),
                    $db->quoteName('a.iso_2'),
                    $db->quoteName('a.iso_3'),
                    $db->quoteName('a.country_code'),
                    $db->quoteName('a.region_code'),
                    $db->quoteName('a.subregion_code'),
                    $db->quoteName('a.phone_prefix'),
                    $db->quoteName('a.currency_code'),
                    $db->quoteName('a.state'),
                    $db->quoteName('b.title') . ' AS currency_title',
                    $db->quoteName('b.symbol'),
                    $db->quoteName('b.dollar_exchange_rate'),
                ]
            )
        )
            ->from($db->quoteName('#__countrybase_countries', 'a'))
            ->leftjoin($db->quoteName('#__countrybase_currencies', 'b') . 'ON a.currency_code = b.currency_code');

        // Filter by search in title.
        $search = $this->getState('filter.search');

        if (!empty($search)) {
            $search = '%' . str_replace(' ', '%', trim($search) . '%');
            $query->where($db->quoteName('a.title') . ' LIKE :search')
            ->bind(':search', $search, ParameterType::STRING);
        }

        // Filter by published state
        $published = (string) $this->getState('filter.published');

        if ($published !== '*') {
            if (is_numeric($published)) {
                $state = (int) $published;
                $query->where($db->quoteName('a.state') . ' = :state')
                    ->bind(':state', $state, ParameterType::INTEGER);
            }
        }

        // Add the list ordering clause.
        $orderCol = $this->state->get('list.ordering', 'a.title');
        $orderDirn = $this->state->get('list.direction', 'ASC');

        $query->order($db->escape($orderCol) . ' ' . $db->escape($orderDirn));
        return $query;
    }
```

Uitleg

- **getQuery(true)** haalt een nieuw leeg query-object op.
- **\$query-\>select()** voegt een SELECT-statement toe. Er kunnen meerdere select-statements zijn - Joomla voegt ze samen.
- **tabelaliassen a en b** let op dat sommige kolommen uit verschillende tabellen komen.
- **-\>from()** definieert welke tabel tabel a is. Dit kan in een aparte verklaring: \$query-\>from();
- **-\>leftjoin()** definieert tabel b en hoe deze moet worden verbonden met tabel a.
- **\$query-\>where()** maakt gebruik van eventuele filters die zijn gedefinieerd, één voor **zoeken** en een andere voor **staat**.
- **return \$query** er is geen bovenliggende aanroep, alles in de query moet hier worden ingesteld.

## tmpl/landen/default.php

Dit is het deel van de code waar de HTML-inhoud wordt gemaakt. Voor de lijst van landen is een tabel nodig met een kopregel en een rij voor gegevens over elk land. Aangezien er 250 landen zijn, is een paginering mechanisme nodig om een subset van landen weer te geven, een paar tegelijk. Daarvoor is een formulier nodig. En in dit geval is een standaard Joomla **searchtools** balk handig. Dit is het:

```php
<?php

/**
 * @package     Countrybase.Site
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

$listOrder = $this->escape($this->state->get('list.ordering'));
$listDirn  = $this->escape($this->state->get('list.direction'));

?>
<h1><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES'); ?></h1>

<form action="<?php echo Route::_('index.php?option=com_countrybase'); ?>"
    method="post" name="adminForm" id="adminForm">

<?php echo LayoutHelper::render('joomla.searchtools.default', array('view' => $this)); ?>

<div class="table-responsive">
    <table class="table table-striped">
    <caption><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_TABLE_CAPTION'); ?></caption>
    <thead>
    <tr>
        <th scope="col">
            <?php echo HTMLHelper::_(
                'searchtools.sort',
                'COM_COUNTRYBASE_COUNTRIES_COUNTRY',
                'a.title',
                $listDirn,
                $listOrder
            ); ?>
        </th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_ISO_2'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_ISO_3'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_CURRENCY_TITLE'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_CURRENCY_SYMBOL'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_CURRENCY_CODE'); ?></th>
        <th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_XRATE'); ?></th>
    </tr>
    </thead>
    <tbody>
    <?php foreach ($this->items as $id => $item) : ?>
    <tr>
        <td><?php echo $item->title; ?></td>
        <td><?php echo $item->iso_2; ?></td>
        <td><?php echo $item->iso_3; ?></td>
        <td><?php echo $item->currency_title; ?></td>
        <td><?php echo $item->symbol; ?></td>
        <td><?php echo $item->currency_code; ?></td>
        <td><?php echo $item->dollar_exchange_rate; ?></td>
    </tr>
    <?php endforeach; ?>
    </tbody>
    </table>
</div>

<?php echo $this->pagination->getListFooter(); ?>

<input type="hidden" name="task" value="">
<input type="hidden" name="boxchecked" value="0">
<?php echo HTMLHelper::_('form.token'); ?>

</form>
```

Punten om op te letten:

- **\$listOrder** en **\$listDirection** worden gebruikt om te sorteren op kolomkop. Alleen de titel is ingesteld om dat te doen.
- **form action** is meestal zo ingesteld dat het naar zichzelf verwijst.
- **LayoutHelper::render('joomla.searchtools.default',...)** maakt een zoekbalk van het type dat te zien is op beheerderslijstpagina's. **Er is een filterformulier nodig!**
- **\$this-\>pagination-\>getListFooter()** haalt de HTML-code op voor de paginering-widget.
- **task** dit verborgen veld wordt door javascript ingevuld wanneer het formulier wordt verzonden.
- **boxchecked** dit verborgen veld wordt gebruikt wanneer een of meer rijselectievakjes zijn geselecteerd voor batchverwerking. Hier echt niet nodig!
- **HTMLHelper::\_('form.token');** haalt de code op voor een formulier-token dat wordt gebruikt als beveiligingsmiddel voor formulierinzendingen waarbij gegevens worden verzonden.

## tmpl/landen/standaard.xml

Deze file wordt gebruikt om een menu-item aan te maken. Het heeft dezelfde naam als het php-bestand, dus default.xml in dit geval.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<metadata>
    <layout title="COM_COUNTRYBASE_VIEW_DEFAULT_MENU_LABEL"
        option="COM_COUNTRYBASE_VIEW_DEFAULT_OPTION">
        <help
            url="components/com_countrybase/help/en-GB/countrybase.html"
        />
        <message>
            <![CDATA[COM_COUNTRYBASE_VIEW_DEFAULT_MENU_DESC]]>
        </message>
    </layout>

    <!-- Add fields to the parameters object for the layout. -->
    <fields name="params">

        <!-- Options -->
        <fieldset name="options">
        </fieldset>

    </fields>
</metadata>
```

Notities:

- **helplink** verwijst naar een helpbestand in de beheerdermap. Hiermee kunt u uw eigen lokale helpbestanden maken, die worden opgeroepen vanuit de menubewerkingsvorm Help-knop na het selecteren van het menu type Countrybase Standaardweergave.
- **params** stelt u in staat om parameters te gebruiken, bijvoorbeeld of een bepaalde kolom in de landenlijst al dan niet moet worden weergegeven. Er zijn nog geen parameters gespecificeerd.
- **sleutel** vertalingen moeten in het bestand administrator/language/en-GB/countrybase.sys.ini staan. De sleutel *COM_COUNTRYBASE_VIEW_DEFAULT_MENU_DESC*, vertaald naar *Toont een Landenpagina*, verschijnt als een beschrijving in het Menu-item Type selectievorm.

## forms/filter_countries.xml

Dit bestand is nodig voor de zoekbalk. Zonder dit bestand zal Joomla een fatale fout geven. De bestandsnaam moet precies zijn zoals getoond: de naam van de weergave voorafgegaan door filter\_. De inhoud is eenvoudig, gewoon definities voor het zoekveld en eventuele andere filters die u wilt gebruiken.

```xml
<?xml version="1.0" encoding="utf-8"?>
<form>
    <fields name="filter">
        <field
            name="search"
            type="text"
            label="COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_LABEL"
            description="COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_DESC"
            hint="JSEARCH_FILTER"
        />

        <field
            name="published"
            type="status"
            label="JOPTION_SELECT_PUBLISHED"
            onchange="this.form.submit();"
            >
            <option value="">JOPTION_SELECT_PUBLISHED</option>
        </field>

    </fields>

    <fields name="list">
        <field
            name="fullordering"
            type="list"
            label="JGLOBAL_SORT_BY"
            default="a.title ASC"
            onchange="this.form.submit();"
            >
            <option value="">JGLOBAL_SORT_BY</option>
            <option value="a.title ASC">COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_ASC</option>
            <option value="a.title DESC">COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_DESC</option>
            <option value="a.iso_2 ASC">ISO2 ASC</option>
            <option value="a.iso_2 DESC">ISO2 DESC</option>
            <option value="a.iso_3 ASC">ISO3 ASC</option>
            <option value="a.iso_3 DESC">ISO3 DESC</option>
            <option value="a.currency_code ASC">COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_ASC</option>
            <option value="a.currency_code DESC">COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_DESC</option>
            <option value="a.id ASC">JGRID_HEADING_ID_ASC</option>
            <option value="a.id DESC">JGRID_HEADING_ID_DESC</option>
        </field>

        <field
            name="limit"
            type="limitbox"
            label="JGLOBAL_LIST_LIMIT"
            default="25"
            onchange="this.form.submit();"
        />
    </fields>
</form>
```

Merk op dat stringsleutels die beginnen met J Joomla-gedefinieerd zijn en dat je ze niet in je taalbestanden moet opnemen.

## language/nl-NL/com_countrybase.ini

Joomla laadt altijd eerst Engelse sleutelvertalingen voordat een andere taal wordt geladen. Dit zorgt ervoor dat sleutels niet in de uitvoer verschijnen als een taal onvolledig is vertaald. Omdat de taalsleutels lange woorden in pseudo-Engels zijn, wordt het als beter beschouwd om een mengeling van Engels en een andere taal te hebben dan een mengeling van sleutels en een andere taal. Als een andere taal in gebruik is, overschrijft Joomla de Engelse strings met die van de andere taal.

Merk op dat het gebruikelijk is om de tekenreeksen in alfabetische volgorde van de sleutel te plaatsen:

```ini
    ; Joomla! Project
    ; (C) 2005 Open Source Matters, Inc. https://www.joomla.org
    ; License GNU General Public License version 2 or later; see LICENSE.txt
    ; Note : All ini files need to be saved as UTF-8

    COM_COUNTRYBASE_COUNTRIES_COUNTRY="Country"
    COM_COUNTRYBASE_COUNTRIES_CURRENCY_CODE="Code"
    COM_COUNTRYBASE_COUNTRIES_CURRENCY_SYMBOL="Symbol"
    COM_COUNTRYBASE_COUNTRIES_CURRENCY_TITLE="Currency"
    COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_ASC="Country ASC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_DESC="Country DESC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_ASC="Currency code ASC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_DESC="Currency code DESC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_DESC="Search in Country Name"
    COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_LABEL="Search"
    COM_COUNTRYBASE_COUNTRIES_ISO_2="ISO2"
    COM_COUNTRYBASE_COUNTRIES_ISO_3="ISO3"
    COM_COUNTRYBASE_COUNTRIES_TABLE_CAPTION="Table of Country Currencies"
    COM_COUNTRYBASE_COUNTRIES_XRATE="Exchange Rate"
    COM_COUNTRYBASE_COUNTRIES="Countries"
```

## src/Service/Router.php

De Router is nodig voor SEO-url's. Zonder deze kan een menu-link verschijnen als option=com_countrybase&view=countries. Met de Router zal een link verschijnen als country-base.html of welke naam je ook kiest voor het koppelingsalias.

```php
    public function __construct(
        SiteApplication $app,
        AbstractMenu $menu
    ) {

        $countries = new RouterViewConfiguration('countries');
        $countries->setKey('id');
        $this->registerView($countries);

        parent::__construct($app, $menu);

        $this->attachRule(new MenuRules($this));
        $this->attachRule(new StandardRules($this));
        $this->attachRule(new NomenuRules($this));
    }
```

Als er meer weergaven zijn, bijvoorbeeld een tabel met valuta's, zou je elke weergave hier definiëren vóór de parent::\_\_construct() verklaring.

*Vertaald door openai.com*

