<!-- Filename: How_to_use_JDate / Display title: Hoe de Date-klasse te gebruiken -->

## Introductie
De Date-klasse van Joomla is een hulpklasse, uitgebreid van de DateTime-klasse van PHP, die ontwikkelaars in staat stelt om datumopmaak efficiënter te beheren. De klasse stelt ontwikkelaars in staat om datums te formatteren voor leesbare strings, MySQL-interactie, UNIX-tijdstampaantallen en biedt ook hulpmethoden voor het werken in verschillende tijdzones.

## Een Datuminstantie Maken

Alle datum-helpermethoden vereisen een instantie van de Date-klasse. Om te beginnen moet je er een maken. Een Date-object kan op twee manieren worden gemaakt. Eén manier is de typische native methode van het simpelweg creëren van een nieuwe instantie:

```php
use Joomla\CMS\Date\Date;

$date = new Date(); // Creates a new Date object equal to the current time.
```

Je kunt ook een instantie maken met behulp van de statische methode gedefinieerd in Date:

```php
use Joomla\CMS\Date\Date;

$date = Date::getInstance(); // Alias of 'new Date();'
```

Er is geen verschil tussen deze methoden, aangezien Date::getInstance simpelweg een nieuwe instantie van Date creëert, precies zoals de eerste getoonde methode.

Als alternatief kun je de huidige datum (als een Date-object) ophalen van het Application-object, door gebruik te maken van:
```php
use Joomla\CMS\Factory;

$date = Factory::getDate();
```

## Argumenten

De Date constructor (en de getInstance statische methode) accepteert twee optionele parameters: een datumstring om te formatteren en een tijdzone. Als je geen datumstring doorgeeft, wordt er een Date-object gemaakt met de huidige datum en tijd. Als je geen tijdzone doorgeeft, zal het Date-object de standaard ingestelde tijdzone gebruiken.

Het eerste argument, indien gebruikt, moet een tekenreeks zijn die kan worden geparseerd met behulp van PHP's native DateTime constructor. Bijvoorbeeld:
```php
use Joomla\CMS\Date\Date;

$currentTime = new Date('now'); // Current date and time
$tomorrowTime = new Date('now +1 day'); // Current date and time, + 1 day.
$plus1MonthTime = new Date('now +1 month'); // Current date and time, + 1 month.
$plus1YearTime = new Date('now +1 year'); // Current date and time, + 1 year.
$plus1YearAnd1MonthTime = new Date('now +1 year +1 month'); // Current date and time, + 1 year and 1 month.
$plusTimeToTime = new Date('now +1 hour +30 minutes +3 seconds'); // Current date and time, + 1 hour, 30 minutes and 3 seconds
$plusTimeToTime = new Date('now -1 hour +30 minutes +3 seconds'); // Current date and time, + 1 hour, 30 minutes and 3 seconds
$combinedTimeToTime = new Date('now -1 hour -30 minutes 23 seconds'); // Current date and time, - 1 hour, +30 minutes and +23 seconds

$date = new Date('2012-12-1 15:20:00'); // 3:20 PM, December 1st, 2012
```

Een Unix-timestamp (in seconden) kan ook als het eerste argument worden doorgegeven, in welk geval het intern wordt omgezet in een datum. Als een tijdzone is opgegeven als het tweede argument van de constructor, wordt het omgezet naar die tijdzone.

## Datums weergeven

Een punt van voorzichtigheid bij het weergeven van Datum-objecten in een gebruikerscontext: druk ze niet simpelweg af op het scherm. De toString()-methode van het Datum-object roept simpelweg de format()-methode van zijn ouder aan, zonder rekening te houden met tijdzones of gelokaliseerde datumopmaak. Dit zal niet leiden tot een goede gebruikerservaring, en zal leiden tot inconsistenties tussen de opmaak in uw extensie en elders in Joomla. In plaats daarvan moet u altijd datums weergeven met behulp van de hieronder getoonde methoden.

### Veelvoorkomende Datumnotaties

Een aantal datumformaten zijn vooraf gedefinieerd in Joomla als onderdeel van de basis taalpakketten. Dit is voordelig omdat het betekent dat datumformaten eenvoudig geïnternationaliseerd kunnen worden. Een voorbeeld van de beschikbare formaatstrings is hieronder te zien, uit het en-GB taalpakket. Het wordt sterk aanbevolen om deze opmaakstrings te gebruiken bij het weergeven van datums, zodat je datums automatisch opnieuw worden opgemaakt volgens de locatie van een gebruiker. Ze kunnen op dezelfde manier worden opgehaald als elke andere taalstring (zie hieronder voor voorbeelden).

```ini
DATE_FORMAT_LC="l, d F Y"
DATE_FORMAT_LC1="l, d F Y"
DATE_FORMAT_LC2="l, d F Y H:i"
DATE_FORMAT_LC3="d F Y"
DATE_FORMAT_LC4="Y-m-d"
DATE_FORMAT_LC5="Y-m-d H:i"
DATE_FORMAT_LC6="Y-m-d H:i:s"
DATE_FORMAT_JS1="y-m-d"
DATE_FORMAT_CALENDAR_DATE="%Y-%m-%d"
DATE_FORMAT_CALENDAR_DATETIME="%Y-%m-%d %H:%M:%S"
DATE_FORMAT_FILTER_DATE="Y-m-d"
DATE_FORMAT_FILTER_DATETIME="Y-m-d H:i:s"
```

### De HtmlHelper-methode (Aanbevolen)

Zoals met veel voorkomende uitvoeritems, is de HtmlHelper-klasse hier om te...helpen! De date()-methode van HtmlHelper accepteert elke datum-tijdreeks die de Date-constructor zou accepteren, samen met een opmaakreeks, en geeft de datum op de juiste manier weer voor de tijdzone-instellingen van de huidige gebruiker. Dit is dan ook de aanbevolen methode om datums voor de gebruiker weer te geven.

```php
use Joomla\CMS\HTML\HTMLHelper;
use Joomla\CMS\Language\Text;

$myDateString = '2012-12-1 15:20:00';
echo HtmlHelper::date($myDateString, Text::_('DATE_FORMAT_FILTER_DATETIME'));
```

### De format()-methode van het Date-object

Een andere optie is om de datum handmatig te formatteren. Als deze methode wordt gebruikt, moet je ook handmatig de tijdzone van de gebruiker ophalen en instellen. Deze methode is nuttiger voor het formatteren van data buiten de gebruikersinterface, zoals in systeemlogs of API-aanroepen.

```php
use Joomla\CMS\Language\Text;
use Joomla\CMS\Date\Date;
use Joomla\CMS\Factory;

$myDateString = '2012-12-1 15:20:00';
$timezone = Factory::getUser()->getTimezone();

$date = new Date($myDateString);
$date->setTimezone($timezone);
echo $date->format(Text::_('DATE_FORMAT_FILTER_DATETIME'));
```

## Andere Nuttige Codevoorbeelden

### De Huidige Tijd Snel Weergeven

Er zijn twee eenvoudige manieren om dit te doen.
- De date() methode van HtmlHelper, als er geen datumwaarde wordt opgegeven, zal standaard de huidige tijd gebruiken.
- Factory::getDate() haalt de huidige datum op als een Date-object, dat we vervolgens kunnen formatteren.

```php
use Joomla\CMS\Factory;
use Joomla\CMS\HTML\HTMLHelper;
use Joomla\CMS\Language\Text;

// These two are functionally equivalent
echo HtmlHelper::date('now', Text::_('DATE_FORMAT_FILTER_DATETIME'));

$timezone = Factory::getUser()->getTimezone();
echo Factory::getDate()->setTimezone($timezone)->format(Text::_('DATE_FORMAT_FILTER_DATETIME'));
```

### Toevoegen en Aftrekken van Datums

Omdat het Joomla Date-object het PHP DateTime-object uitbreidt, biedt het methoden voor het toevoegen en aftrekken van datums. De eenvoudigste van deze methoden is de modify() methode, die elke relatieve wijzigingsstring accepteert die de PHP [strtotime](https://www.php.net/manual/en/function.strtotime.php) methode zou accepteren. Bijvoorbeeld:

```php
use Joomla\CMS\Date\Date;

$date = new Date('2012-12-1 15:20:00');
$date->modify('+1 year');
echo $date->toSQL(); // 2013-12-01 15:20:00
```

Er zijn ook afzonderlijke add() en sub() methoden, voor het respectievelijk toevoegen of aftrekken van tijd van een datumobject. Deze accepteren PHP-standaard [DateInterval](https://www.php.net/manual/en/class.dateinterval.php) objecten.

```php
use Joomla\CMS\Date\Date;

$interval = new \DateInterval('P1Y1D'); // Interval represents 1 year and 1 day

$date1 = new Date('2012-12-1 15:20:00');
$date1->add($interval);
echo $date1->toSQL(); // 2013-12-02 15:20:00

$date2 = new Date('2012-12-1 15:20:00');
$date2->sub($interval);
echo $date2->toSQL(); // 2011-11-30 15:20:00
```

### Datums weergeven in ISO 8601-formaat

```php
$date = new Date('2012-12-1 15:20:00');
$date->toISO8601(); // 20121201T152000Z
```

### Datums in RFC 822-formaat weergeven

```php
$date = new Date('2012-12-1 15:20:00');
$date->toRFC822(); // Sat, 01 Dec 2012 15:20:00 +0000
```

### Datums uitgeven in SQL datum-tijd formaat

```php
$date = new Date('20121201T152000Z');
$date->toSQL(); // 2012-12-01 15:20:00
```

### Datums weergeven als Unix-timestamps

Een Unix-tijdstempel wordt weergegeven als het aantal seconden dat is verstreken sinds de Unix Epoch (de eerste seconde van 1 januari 1970).

*Vertaald door openai.com*

