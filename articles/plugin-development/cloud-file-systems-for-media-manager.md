<!-- Filename: J4.x:Cloud_File_Systems_for_Media_Manager / Display title: Cloud-bestandssystemen voor Media Manager -->

<span id="main-portal-heading">GSoC 2017 Cloud File Systemen voor Media Manager Documentatie</span> [<img src="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/75px-Gsoc2016.png" decoding="async" srcset="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/113px-Gsoc2016.png 1.5x, https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/150px-Gsoc2016.png 2x" data-file-width="373" data-file-height="373" width="75" height="75" alt="Gsoc2016.png" />](https://docs.joomla.org/GSOC_2017 "GSOC 2017") Joomla!  4.x

## Introductie

**Joomla! 4.x** wordt standaard geleverd met cloud-bestandssystemen voor de **Media Manager**. Met de vorige API was het maken van aangepaste bestandssystemen een moeilijke taak. Dankzij de nieuwe API is het nu eenvoudig om een aangepast bestandssysteem te maken. Als je een clouddienst wilt gebruiken met de nieuwe Media Manager, wordt geadviseerd om **OAuth2.0** te gebruiken.

Dit document zal je begeleiden door belangrijke stappen om je eigen **Bestandssysteem Plugin** te maken om de **Media Manager** uit te breiden. Voordat je verder gaat, zorg ervoor dat je de basiskennis hebt over hoe je een plugin voor Joomla kunt ontwikkelen. [Deze tutorial](https://docs.joomla.org/J3.x:Creating_a_Plugin_for_Joomla "Special:MyLanguage/J3.x:Creating a Plugin for Joomla") zou moeten helpen.

## Maak uw Filesystem-plug-in aan

### Configuratie

Allereerst moeten we een **filesystem**-plug-in maken om de Media Manager uit te breiden. Deze plug-in moet enkele belangrijke eigenschappen bevatten zodat deze kan werken met de Media Manager.

Zorg ervoor dat uw plugin `group="filesystem"` bevat. Dus in uw `[plugin-name].xml`, zou u moeten hebben:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension version="4.0" type="plugin" group="filesystem" method="upgrade">
    <name>plg_filesystem_myplugin</name>
    <author>Joomla! Project</author>
    <creationDate>April 2017</creationDate>
    <copyright>Copyright (C) 2005 - 2017 Open Source Matters. All rights reserved.</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <authorEmail>admin@joomla.org</authorEmail>
    <authorUrl>www.joomla.org</authorUrl>
    <version>__DEPLOY_VERSION__</version>
    <description>Description</description>
    <files>
        <filename plugin="myplugin">myplugin.php</filename>
        <folder>SomeFolder</folder>
    </files>

    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="display_name"
                    type="text"
                    label="YOUR_LABEL"
                    description="YOUR_DESCRIPTION"
                    default="DEFAULT_VALUE"
                />
            </fieldset>
        </fields>
    </config>
</extension>
```

De parameter **display_name** helpt de Media Manager om de naam van je **Bestandssysteem** als een hoofdnode in de Bestandsbrowser weer te geven. Elke adapter die bij je Bestandssysteem hoort, wordt als kindelement onder de hoofdnode weergegeven.

Neem alle vereiste parameters op die u nodig heeft, zoals `App Secret` met geschikte formuliervelden.

### Plug-in evenementen

- `onFileSystemGetAdapters()`
- `onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)`

#### onFileSystemGetAdapters()

**Elke** Filesystem-plugin moet een evenement bevatten genaamd `onFileSystemGetAdapters()` voor functionaliteit. Dit evenement moet een **array** van `Joomla\Component\Media\Administrator\Adapter\AdapterInterface` retourneren wanneer het wordt aangeroepen. Het evenement wordt geactiveerd wanneer je de Media Manager opent. Elke `AdapterInterface` wordt onder de hoofdmap van je bestandssysteem gekoppeld.

#### onFileSystemOAuthCallback()

Dit evenement wordt geactiveerd wanneer je de **OAuthCallbackHandler** van Media Manager hebt gebruikt voor het OAuth2.0 autorisatie- en authenticatieproces dat hieronder in het document wordt beschreven. Het evenement wordt alleen geactiveerd op de aangevraagde plugin. Het is dus niet nodig om `$context` te controleren in een typisch scenario.

Een voorbeeld van het gebruik van het evenement ziet er als volgt uit:

```php
    public function onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)
    {
        // Your context
        $context = $event->getContext();

        // Get the input
        $data = $event->getInput();

        // Your code goes here

        // Set result to be returned
        $result = [
            "action" => "control-panel"
        ];

        // Pass back the result to event
        $event->setArgument('result', $result);
    }
```

**OAuthCallbackEvent** bevat de invoer die is doorgestuurd naar de Media Manager OAuthCallback URI. `$event->getInput()` retourneert een Input Object.

`$result` definieert hoe Joomla zich gedraagt na een omleiding naar de callback. Er zijn verschillende mogelijke oplossingen beschikbaar:

- `actie`
  - sluit: Sluit een venster dat is geopend door een JavaScript **met
    window.open()**
  - omleiden: Omleidt naar de pagina die is gespecificeerd door de **redirect_uri**
  - controlepaneel: Omleiden naar het controlepaneel
  - media-manager: Omleiden naar de Media Manager
- `redirect_uri`
  - Wordt gebruikt met **omleiden** actie (vereist)
- `boodschap`
  - Boodschap moet worden weergegeven na een actie
- `boodschap_type`
  - waarschuwing
  - kennisgeving
  - fout
  - boodschap(of laat leeg)

Nadat je alles hebt ingesteld, stel je het `$result` argument van de `$event` in om het terug te geven aan de aanroeper.

Een voorbeeld om een bericht aan de gebruiker te tonen zou zijn:

```php
    $result = [
        "action" => "media-manager",
        "message" => "Some message",
        "message-type" => "notice"
    ];
```

Dit zal omleiden naar de Media Manager en een melding geven met de tekst in **bericht**.

Deze methode kan meestal worden gebruikt om autorisatiecodes te verkrijgen voor het **OAuth2.0**-proces. In geval van een **fout** zal Joomla! terugvallen naar het configuratiescherm en een foutmelding weergeven.

## Authenticatie en Autorisatie

Joomla! 4.x adviseert je om OAuth2.0 te gebruiken voor dit proces. Het is een veelvoorkomend scenario dat OAuth2.0 een **redirect url** vereist om authenticatiegegevens door te geven aan de applicatie. Dit wordt meestal gedaan door een callback handler. Voor je plugin hoef je het wiel niet opnieuw uit te vinden; je kunt de **Callback Handler** van **Media Manager** gebruiken om de taak te volbrengen.

Alles wat je hoeft te doen is de **Redirect URI** als volgt instellen in je OAuth2.0-voorziening en de `onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)` in je plugin implementeren.

`[site-naam]/administrator/index.php?option=com_media&task=plugin.oauthcallback&plugin=[uw-plugin-naam]`

In dit voorbeeld:
`[site-name]/administrator/index.php?option=com_media&task=plugin.oauthcallback&plugin=myplugin`

Nu, wanneer je een omleiding doet vanaf je provider zodra deze de opgegeven URL bereikt, zal de **Controller** voor Media Manager ervoor zorgen dat je plugin 
`onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)`
implementeert voordat enige gegevens worden doorgegeven. Zo niet, dan word je doorgestuurd naar het Configuratiescherm met een foutmelding.

Als je alle ingangen hebt geïmplementeerd die worden doorgegeven, zal de Cloud Provider worden opgenomen in de `$event`. Je kunt er toegang toe krijgen met `$event->getInput()` en het behandelen als een gebruikelijke `input` voor Joomla.

Nadat je de `input` hebt ontvangen, kun je deze gebruiken om de details van je clouddienst te verkrijgen, zoals **Access Token**, **Refresh Token** enzovoort.

Als je meerdere accounts wilt onderscheiden, kun je `Session` daarvoor gebruiken.

**Opmerking**: Het wordt aangeraden om te controleren op een **CSRF-token** voordat je je verzoek voortzet. De meeste cloudproviders ondersteunen de `state`-parameter die kan worden gebruikt om de taak te vervullen.

### Veelvoorkomende valkuilen

Het is nodig om je redirect-uri te `urlencode()` wanneer je deze naar de cloudprovider stuurt. Gebruik dit om ongewenst gedrag van je cloud te voorkomen.

## Bestand bedienen vanuit uw adapter

Om uw mediabestanden van de Media Manager naar **Joomla! Site** te serveren
helpt `Joomla\Component\Media\Administrator\Adapter\AdapterInterface` u. Deze Interface bevat een speciale methode genaamd `getUrl($path)`.

`$path : Pad is relatief ten opzichte van je root`

De bedoeling van de methode is om een **Openbare Absolute URL** te bieden voor het bestand dat je wilt invoegen op de site. Stel dat je bestandspad `/pad/naar/mij.png` is op de cloudserver. Een publieke URL zou dan iets kunnen zijn als `mycloud.com/share/u/mijngebruikersnaam/pad/naar/mij.png`. Hoe dit wordt aangeboden, is helemaal aan jou. Wanneer een gebruiker een gegenereerde media-URL wil invoegen, zal deze methode worden gebruikt.

Meer details over `Adapter Interface` zijn te vinden in `administrator/componenents/com_media/Adapter/AdapterInterface.php`.

*Vertaald door openai.com*

