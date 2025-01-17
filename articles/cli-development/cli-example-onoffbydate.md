<!-- Filename: J4.x:CLI_example_-_Onoffbydate / Display title: CLI voorbeeld - Onoffbydate -->

## Inleiding

Er zijn gelegenheden waarbij een site een module moet weergeven of verbergen afhankelijk van de datum. Een voorbeeld hiervan kan een aangepaste html-module zijn die een bericht tijdens de winter weergeeft. Een ander voorbeeld kan de afwisseling van aangepaste modules zijn afhankelijk van de dag van de week, bijvoorbeeld één voor weekdagen en een andere voor weekenden. Het hier gepresenteerde voorbeeld gebruikt een plugin, een cli-commando en een cron om het gewenste effect te bereiken.

De code is beschikbaar op [GitHub](https://github.com/ceford/cefjdemos-plg-onoffbydate)

Er zijn meer artikelen over het gebruik van de CLI in de [Gebruikershandleiding](http://localhost/jdm4/jdocmanual?article=user/command-line-interface/using-the-cli) en in de Joomla! Programmeursdocumentatie [lokale kopie](jdocmanual?article=docus/plugins/plugin-examples-console-plugin-sqlfile) of [originele bron](https://manual.joomla.org/docs/building-extensions/plugins/plugin-examples/console-plugin-sqlfile/);

## Joomla-normen

In eerdere versies van Joomla gebruikte het pluginsysteem een implementatie van het Observable/Observer-patroon. Als gevolg hiervan zouden alle publieke methoden van elke geladen plugin onmiddellijk als observators worden geregistreerd. Dit kon problemen veroorzaken.

Joomla 4 en later gebruikt het Joomla Framework Event-pakket om plug-in evenementen te beheren. Dit zorgt voor betere prestaties en veiligheid. In praktische termen betekent dit dat de bestandsstructuur van een Joomla 4/5/6 plug-in verschilt van de legacy plug-in structuur van eerdere versies.

## De Plugin Structuur

De plugin heet **onoffbydate**. Het volgende schematische diagram toont de bestandsstructuur die wordt gebruikt voor lokale ontwikkeling van de plugin:

```
cefjdemos-plg-onoffbydate
|-- .vscode (folder for build settings)
|-- cefjdemos-plg-onoffbydate (folder compressed to create an installable zip file)
    |-- language
        |-- en-GB (language folder, kept with the extension code)
            |-- plg_system_onoffbydate.ini
            |-- plg_system_onoffbydate.sys.ini
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Console (folder for extension specific code)
            |-- OnoffbydateCommand.php (the plugin execution code)
        |-- Extension
            |-- Onoffbydate.php (the plugin register code)
    |-- onoffbydate.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- changelog.xml (a record of changes in each release)
|-- README.md (brief description and instructions on use)
|-- updates.xml (the update server specification)
```

## Het manifesto bestand onoffbydate.xml

Dit is het installatiebestand - een standaarditem voor elke extensie.

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="plugin" group="console" method="upgrade">
    <name>plg_console_onoffbydate</name>
    <author>Clifford E Ford</author>
    <creationDate>Jamuary 2025</creationDate>
    <copyright>(C) Clifford E Ford</copyright>
    <license>GNU General Public License version 3 or later</license>
    <authorEmail>cliff@xxx.yyy.co.uk</authorEmail>
    <version>0.3.0</version>
    <description>PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Plugin\Console\Onoffbydate</namespace>
    <files>
        <folder>language</folder>
        <folder plugin="onoffbydate">services</folder>
        <folder>src</folder>
    </files>
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-plg-onoffbydate/main/changelog.xml</changelogurl>
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" priority="2" name="Cefjdemosonoffbydate Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-plg-onoffbydate/main/updates.xml</server>
    </updateservers>
    <config>
        <fields>

        </fields>
    </config>
</extension>
```

- De **namespace** regel vertelt Joomla waar de code met namespace voor deze plugin te vinden is.
- De **language** map wordt bij de plugin-code gehouden.
- Een **updateserver** is vereist om aan de JED-vereisten te voldoen.
- Er zijn geen **config** opties voor deze plugin, maar dat kan in de toekomst veranderen. Bijvoorbeeld, de gebruiker zou de wintermaanden liever met selectievakjes instellen dan ze hard gecodeerd te hebben.

## Registratie: services/provider.php

Dit is het toegangspunt voor de plugincode.

```php
<?php

/**
 * @package     Cefjdemos.Plugin
 * @subpackage  Console.Onoffbydate
 *
 * @copyright   Copyright (C) 2025 Clifford E Ford.
 * @license     GNU General Public License version 3 or later.
 */

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\Extension\PluginInterface;
use Joomla\CMS\Plugin\PluginHelper;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;
use Joomla\CMS\Factory;
use Joomla\Event\DispatcherInterface;
use Cefjdemos\Plugin\Console\Onoffbydate\Extension\Onoffbydate;

return new class implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.2.0
     */
    public function register(Container $container)
    {
        $container->set(
            PluginInterface::class,
            function (Container $container) {
                $dispatcher = $container->get(DispatcherInterface::class);
                $plugin     = new Onoffbydate(
                    $dispatcher,
                    (array) PluginHelper::getPlugin('console', 'onoffbydate')
                );
                $plugin->setApplication(Factory::getApplication());

                return $plugin;
            }
        );
    }
};
```

## Evenementabonnement: src/Extension/Onoffbydate.php

Dit is de plaats waar de gebeurtenis die de plugin activeert wordt geregistreerd en de locatie van de code die de gebeurtenis zal afhandelen wordt ingesteld.

```php
<?php

/**
 * @package     Cefjdemos.Plugin
 * @subpackage  Console.Onoffbydate
 *
 * @copyright   Copyright (C) 2025 Clifford E Ford.
 * @license     GNU General Public License version 3 or later.
 */

namespace Cefjdemos\Plugin\Console\Onoffbydate\Extension;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\Plugin\CMSPlugin;
use Joomla\Event\SubscriberInterface;
use Cefjdemos\Plugin\Console\Onoffbydate\Console\OnoffbydateCommand;

final class Onoffbydate extends CMSPlugin implements SubscriberInterface
{
    /**
     * Returns the event this subscriber will listen to.
     *
     * @return  array
     */
    public static function getSubscribedEvents(): array
    {
        return [
                \Joomla\Application\ApplicationEvents::BEFORE_EXECUTE => 'registerCommands',
        ];
    }

    /**
     * Returns the command class for the Onoffbydate CLI plugin.
     *
     * @return  void
     */
    public function registerCommands(): void
    {
        $myCommand = new OnoffbydateCommand();
        $myCommand->setParams($this->params);
        $this->getApplication()->addCommand($myCommand);
    }
}
```

De **OnoffbydateCommand** klasse bevindt zich in de *src/Console* map aangezien de ingebouwde Joomla Console opdrachten zich in een Console map bevinden. Het had in een *Extension* map geplaatst kunnen worden.

## Het Commandobestand: src/Console/OnoffbydateCommand.php

Dit bestand bevat de code die al het werk voor deze extensie doet.

```php
<?php

/**
 * @package     Cefjdemos.Plugin
 * @subpackage  Console.Onoffbydate
 *
 * @copyright   Copyright (C) 2025 Clifford E Ford.
 * @license     GNU General Public License version 3 or later.
 */

namespace Cefjdemos\Plugin\Console\Onoffbydate\Console;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

use Joomla\CMS\Factory;
use Joomla\CMS\Language\Text;
use Joomla\Console\Command\AbstractCommand;
use Joomla\Database\DatabaseInterface;
use Symfony\Component\Console\Input\InputArgument;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;
use Symfony\Component\Console\Style\SymfonyStyle;
use Joomla\Database\DatabaseAwareTrait;

class OnoffbydateCommand extends AbstractCommand
{
    use DatabaseAwareTrait;

    /**
     * The default command name
     *
     * @var    string
     *
     * @since  4.0.0
     */
    protected static $defaultName = 'onoffbydate:action';

    /**
     * @var InputInterface
     * @since version
     */
    private $cliInput;

    /**
     * SymfonyStyle Object
     * @var SymfonyStyle
     * @since 4.0.0
     */
    private $ioStyle;

    /**
     * The params associated with the plugin, plus getter and setter
     * These are injected into this class by the plugin instance
     */
    protected $params;

    protected function getParams()
    {
        return $this->params;
    }

    public function setParams($params)
    {
        $this->params = $params;
    }

    /**
     * Initialise the command.
     *
     * @return  void
     *
     * @since   4.0.0
     */
    protected function configure(): void
    {
        $lang = Factory::getApplication()->getLanguage();
        $test = $lang->load(
            'plg_console_onoffbydate',
            JPATH_BASE . '/plugins/console/onoffbydate'
        );

        $this->addArgument(
            'season',
            InputArgument::REQUIRED,
            'winter or weekend'
        );
        $this->addArgument(
            'action',
            InputArgument::REQUIRED,
            'publish or unpublish'
        );

        $this->addArgument(
            'module_id',
            InputArgument::REQUIRED,
            'module id'
        );

        $help = Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_1');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_2');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_3');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_4');
        $help .= Text::_('PLG_CONSOLE_ONOFFBYDATE_HELP_5');

        $this->setDescription(Text::_('PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION'));
        $this->setHelp($help);
    }

    /**
     * Internal function to execute the command.
     *
     * @param   InputInterface   $input   The input to inject into the command.
     * @param   OutputInterface  $output  The output to inject into the command.
     *
     * @return  integer  The command exit code
     *
     * @since   4.0.0
     */
    protected function doExecute(InputInterface $input, OutputInterface $output): int
    {
        $this->configureIO($input, $output);

        $season = $this->cliInput->getArgument('season');
        $action = $this->cliInput->getArgument('action');
        $module_id = $this->cliInput->getArgument('module_id');

        switch ($season) {
            case 'winter':
                $result = $this->winter($module_id, $action);
                break;
            case 'weekend':
                $result = $this->weekend($module_id, $action);
                break;
            default:
                $result = Text::_('PLG_CONSOLE_ONOFFBYDATE_ERROR', $season);
                $this->ioStyle->error($result);
                return 0;
        }

        return 1;
    }

    protected function publish($module_id, $published)
    {
        $db = Factory::getContainer()->get(DatabaseInterface::class);
        //$db    = $this->getDatabase();
        $query = $db->getQuery(true);
        $query->update('#__modules')
            ->set('published = ' . $published)
            ->where('id = ' . $module_id);
        $db->setQuery($query);
        $db->execute();
    }

    protected function weekend($module_id, $action)
    {
        // get the day of the week
        $day = date('w');
        if (in_array($day, array(0,6))) {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_A_WEEKEND');
            $published = $action === 'publish' ? 1 : 0;
        } else {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_A_WEEKEND');
            $published = $action === 'publish' ? 0 : 1;
        }

        $this->publish($module_id, $published);

        $state = empty($published) ? Text::_('JUNPUBLISHED') : Text::_('JPUBLISHED');
        $result = Text::sprintf('PLG_CONSOLE_ONOFFBYDATE_SUCCESS', $msg, $module_id, $state);

        $this->ioStyle->success($result);
    }

    protected function winter($module_id, $action)
    {
        // get the month of the month
        $month = date('n');
        if (in_array($month, array(1,2,11,12))) {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_WINTER');
            $published = $action === 'publish' ? 1 : 0;
        } else {
            $msg = Text::_('PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_WINTER');
            $published = $action === 'publish' ? 0 : 1;
        }

        $this->publish($module_id, $published);

        $state = empty($published) ? Text::_('JUNPUBLISHED') : Text::_('JPUBLISHED');

        $state = empty($published) ? Text::_('JUNPUBLISHED') : Text::_('JPUBLISHED');
        $result = Text::sprintf('PLG_CONSOLE_ONOFFBYDATE_SUCCESS', $msg, $module_id, $state);

        $this->ioStyle->success($result);
    }

    /**
     * Configures the IO
     *
     * @param   InputInterface   $input   Console Input
     * @param   OutputInterface  $output  Console Output
     *
     * @return void
     *
     * @since 4.0.0
     *
     */
    private function configureIO(InputInterface $input, OutputInterface $output)
    {
        $this->cliInput = $input;
        $this->ioStyle = new SymfonyStyle($input, $output);
    }
}
```

- De functie `configure` stelt vast dat drie opdrachtenregelargumenten vereist zijn:
    - `season` moet een van `weekend` of `winter` zijn.
    - `action` moet een van `publish` of `unpublish` zijn.
    - `module_id` moet de gehele ID zijn van de module die gepubliceerd of gedepubliceerd moet worden.
- De functie `doExecute` is waar het werk wordt gedaan. Twee actiemogelijkheden worden erkend:
    - `winter` roept een functie aan om een module te publiceren of te depubliceren als vandaag in de wintermaanden is.
    - `weekend` roept een functie aan om een module te publiceren of te depubliceren als vandaag een weekenddag is.
- De functie `publish` doet een databaseoproep om het veld `publishes` van de module in te stellen op `0` of `1` zoals gepast.
- De functies `winter` en `weekend` doen enkele datumcalculaties om te bepalen of vandaag in de winter (of niet) of in een weekend (of niet) is om de juiste parameters door te geven aan de publicatiefunctie.
- De aanroep van de functie `$this->ioStyle->success()` produceert een consoletekstbericht met zwarte tekst en een groene achtergrond. `$this->ioStyle->error()` produceert een FOUTmelding met witte tekst op een kastanjebruine achtergrond.

## taalbestanden

Er zijn twee taalbestanden die worden gebruikt tijdens de installatie en plug-in configuratie. Het bestand en-GB/plg_console_onoffbydate.sys.ini bevat de teksten die te zien zijn tijdens installatie en beheer:

```ini
PLG_CONSOLE_ONOFFBYDATE="Console - Onoffbydate"
PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION="Set date-dependent enabled state of a module via cli"
```

Het en-GB/plg_console_onoffbydate.ini-bestand bevat de strings die in gebruik worden gezien:

```ini
PLG_CONSOLE_ONOFFBYDATE="Console - Onoffbydate"
PLG_CONSOLE_ONOFFBYDATE_DESCRIPTION="Set date-dependent enabled state of a module via cli"
PLG_CONSOLE_ONOFFBYDATE_ERROR="Unknown season: %s."
PLG_CONSOLE_ONOFFBYDATE_HELP_1="<info>%command.name%</info> Toggles module Enabled/Disabled state\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_2="Usage: <info>php %command.full_name% season action module_id where\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_3="    season is one of winter or weekend,\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_4="    action is one of publish or unpublish and\n"
PLG_CONSOLE_ONOFFBYDATE_HELP_5="    module_id is the id of the module to publish or unpublish.</info>\n"
PLG_CONSOLE_ONOFFBYDATE_SUCCESS="That seemed to work. %s Module %s has been %s."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_A_WEEKEND="Today is in a weekend."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_IN_WINTER="Today is in winter."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_A_WEEKEND="Today is not in a weekend."
PLG_CONSOLE_ONOFFBYDATE_TODAY_IS_NOT_IN_WINTER="Today is not in winter."
```

Merk op dat de uitvoer van de opdracht alleen door jou op je Joomla-installatie wordt gezien.

## De Module

Deze code verandert eenvoudigweg de gepubliceerde status van een module afhankelijk van een functie van de datum. Je hebt dus het module-id nodig zoals geïllustreerd in de rechterkolom van de Modules (Site) lijstpagina.

## De Opdrachtregel

Installeer de plugin in je Joomla-testinstallatie en schakel deze in. Als het je site crasht, gebruik dan phpMyAdmin om de nieuw geïnstalleerde plugin in de #__extensions-tabel te vinden en stel de ingeschakelde parameter in op 0. Los het probleem op en probeer het opnieuw.

In een terminalvenster navigeer je naar de hoofdmap van je site en gebruik je het volgende commando:

```sh
php cli/joomla.php list
```

Als er in deze fase iets misgaat, controleer dan of de aangeroepen PHP-versie de opdrachtregelversie is en niet die welke door de webserver wordt gebruikt. Je kunt afkeuringsberichten onderdrukken met deze versie van de opdrachtregel.

```sh
php -d error_reporting="E_ALL & ~E_DEPRECATED" cli/joomla.php onoffbydate:action garbage publish 133
```

Als de code werkt, zie je `onoffbydate` tussen de lijst van beschikbare commando's en je kunt hulp inschakelen om te zien hoe het gebruikt moet worden:

```sh
php cli/joomla.php onoffbydate:action --help
Usage:
  onoffbydate:action <season> <action> <module_id>

Arguments:
  season                       winter or summer or weekday or weekend
  action                       publish or unpublish
  module_id                    module id

Options:
  -h, --help                   Display the help information
  -q, --quiet                  Flag indicating that all output should be silenced
  -V, --version                Displays the application version
      --ansi                   Force ANSI output
      --no-ansi                Disable ANSI output
  -n, --no-interaction         Flag to disable interacting with the user
      --live-site[=LIVE-SITE]  The URL to your site, e.g. https://www.example.com
  -v|vv|vvv, --verbose         Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Help:
  onoffbydate:action Toggles module Enabled/Disabled state
  Usage: php joomla.php onoffbydate:action season action module_id where
      season is one of winter or summer or weekday or weekend,
      action is one of publish or unpublish and
      module_id is the id of the module to publish or unpublish.
```

En probeer het dan gewoon uit:

```sh
php cli/joomla.php onoffbydate:action weekend publish 133

     [OK] That seemed to work. Today is not a weekend. Module 133 has been Unpublished
```

### In gebruik

Deze gebruikslogica is een beetje onlogisch! Als je een module hebt die alleen in het weekend gepubliceerd moet worden, zijn de parameters om elke dag te gebruiken `weekend publish module_id`. Dat publiceert de module op dagen in een weekend en depubliceert deze op dagen die niet in het weekend zijn. Voor een module die op weekdagen gepubliceerd moet worden, zijn de parameters om elke dag te gebruiken `weekend unpublish module_id`. Dat depubliceert de module op dagen in het weekend en publiceert deze op dagen die niet in het weekend zijn. Dezelfde logica geldt voor de `winter` versie.

## De cron

De opdracht kan in een terminalvenster getest worden, maar je wilt het waarschijnlijk vanuit een cron gebruiken. De optie `winter` zou op de eerste dag van elke maand kunnen worden uitgevoerd. De optie `weekend` zou dagelijks worden uitgevoerd. Het belangrijkste punt is dat je zoveel crons kunt hebben als je nodig hebt om de gepubliceerde status van zoveel modules als je wilt op geschikte intervallen te wijzigen. Dezelfde code werkt voor allemaal.

Op een hostingdienst moet u de volledige paden opgeven naar de php-uitvoerbare bestand en het joomla cli-commando. Voorbeeld:

```sh
/usr/local/bin/php /home/username/public_html/pathtojoomla/cli/joomla.php onoffbydate:action winter publish 130
```

Afhankelijk van hoe je je cron en je systeem hebt ingesteld, kun je een geruststellende e-mail ontvangen die precies dezelfde informatie bevat als je in de commandoregel ziet.

## Controleren

En natuurlijk: ga naar je homepagina en controleer of de module echt is gepubliceerd of niet gepubliceerd.

*Vertaald door openai.com*

