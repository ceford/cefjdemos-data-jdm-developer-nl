<!-- Filename: J4.x:Cloud_File_Systems_for_Media_Manager / Display title: Cloud Bestands Systems voor Media Beheer -->

<span id="main-portal-heading">GSoC 2017
Cloud Bestands Systems voor Media Beheer
Documentatie</span> [<img
src="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/75px-Gsoc2016.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/113px-Gsoc2016.png 1.5x, https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/150px-Gsoc2016.png 2x"
data-file-width="373" data-file-height="373" width="75" height="75"
alt="Gsoc2016.png" />](https://docs.joomla.org/GSOC_2017 "GSOC 2017")
Joomla!  4.x

## Introductie

**Joomla! 4.x** wordt standaard geleverd met cloud-bestandssystemen voor
de **Media Manager**. Met de vorige API was het maken van aangepaste
bestandssystemen een moeilijke taak. Dankzij de nieuwe API is het nu
eenvoudig om een ​​aangepast bestandssysteem te maken. Als u een
cloudservice wilt gebruiken met de nieuwe Media Manager, is het raadzaam
om *'OAuth2.0'* te gebruiken.

Dit document leidt u door belangrijke stappen om uw eigen **File System
Plugin** te maken om de **Media Manager** uit te breiden. Voordat u
verdergaat, moet u ervoor zorgen dat u de basiskennis hebt over het
ontwikkelen van een plugin voor Joomla. [This
tutorial](https://docs.joomla.org/J3.x:Creating_a_Plugin_for_Joomla "Special:MyLanguage/J3.x:Creating a Plugin for Joomla")
kan hierbij helpen.

## Maak je Filesystem plugin

### Configuratie

Allereerst moeten we een **filesystem** plugin die Media beheer
uitbreid. Deze plug-in moet enkele belangrijke kenmerken bevatten, zodat
deze met Media Manager kan werken.

Zorg ervoor dat je plugin bevat`group="filesystem"`. Dus in
je`[plugin-name].xml`, moet staan:

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

De **display_name** parameter helpt Media Beheer de naam te tonen van je
**File System** als basis in je bestands browser. Elke adapter die bij
uw bestandssysteem hoort, wordt als "child" onder de root weergegeven.

Neem all vereiste parameters op, zoals `App Secret` met geschikte
formuliervelden.

### Plugin Events

- `onFileSystemGetAdapters()`
- `onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)`

#### onFileSystemGetAdapters()

**Any** De plug-n van het bestandssysteem moet de gebeurtenis bevatten
met de naam`onFileSystemGetAdapters()` voor functionaliteit. Deze
gebeurtenis moet een '**array** retouneren van
`Joomla\Component\Media\Administrator\Adapter\AdapterInterface` als het
is aangeroepen. Het event wordt georganiseerd wanneer u Media Manager
opent. Elke `AdapterInterface` zal worden gemount aan de basis van je
bestandssysteem.

#### onFileSystemOAuthCallback()

Deze gebeurtenis wordt geactiveerd wanneer u Media Manager gebruikt
**OAuthCallbackHandler** voor OAuth2.0 Autorisatie- en
authenticatieproces hieronder beschreven in het document. De gebeurtenis
wordt alleen geactiveerd op de gevraagde plug-in. Het is dus niet nodig
om te controleren op `$context` in een typisch scenario.

Een voorbeeld van het gebruik van deze gebeurtenis ziet eruit als:

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

**OAuthCallbackEvent** bevat de invoer doorgestuurd naar de Media
Manager OAuthCallback URI. `$event->getInput()` geeft een invoerobject
terug.

`$result` definieert hoe Joomla zich gedraagt ​​na een omleiding naar de
callback. Er zijn verschillende mogelijke oplossingen beschikbaar:

- `action`
  - sluit: Sluit een scherm dat is geopend door Javascript **using
    window.open()**
  - doorverwijzing: Doorverwijzing naar de pagina **redirect_uri**
  - controle-paneel: Verwijs door naar het controle paneel
  - media beheer: Verwijs door naar Media Beheer
- `redirect_uri`
  - Gebruikt met **redirect** actie (verplicht)
- `message`
  - Bericht moet getoond worden na een actie
- `message_type`
  - waarschuwing
  - let op
  - fout
  - bericht(of zet leeg)

Nadat u alles hebt ingesteld, stelt u de`$result` argument van de
`$event` terug te koppelen.

Een voorbeeld om een ​​bericht aan de gebruiker weer te geven zou zijn:

```php
    $result = [
        "action" => "media-manager",
        "message" => "Some message",
        "message-type" => "notice"
    ];
```

Hiermee wordt u omgeleid naar Media Manager en wordt een melding
weergegeven met de tekst **message**.

Deze methode zal meestal worden gebruikt om autorisatiecodes te
verkrijgen voor het '**OAuth2.0** proces. In het geval van een
**error**, Joomla! zal terugvallen op het bedieningspaneel en zal een
foutmelding weergeven.

## Authenticatie en authorisatie

Joomla! 4.x adviseerd je gebruik OAuth2.0 voor dit proces. Het is
gebruikelijk OAuth2.0 heeft een**redirect url** nodig om
authenticatiegegevens door te geven aan de applicatie. Dit wordt meestal
gedaan door een callback handler. Voor uw plugin hoeft u het wiel niet
opnieuw uit te vinden, u kunt de **Callback Handler** of **Media
Manager** gebruiken om de taak uit te voeren.

Het enige wat u moet doen is stel de *Redirect URI'* in om te volgen in
OAuth2.0 Verstrek en implementeer
de`onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)`
in je plugin.

`[site-name]/administrator/index.php?option=com_media&task=plugin.oauthcallback&plugin=[your-plugin-name]`

In dit voorbeeld:
`[site-name]/administrator/index.php?option=com_media&task=plugin.oauthcallback&plugin=myplugin`

Wanneer u nu een omleiding van uw provider uitvoert zodra deze de
opgegeven URL bereikt, de **Controller** voor de Media Manager zorgt
ervoor dat uw plug-in wordt geïmplementeerd
`onFileSystemOAuthCallback(\Joomla\Component\Media\Administrator\Event\OAuthCallbackEvent $event)`
voordat u er gegevens aan doorgeeft. Als dit niet het geval is, wordt u
doorgestuurd naar het Configuratiescherm met een foutmelding.

Als u alle ingevoerde gegevens hebt geïmplementeerd, wordt de
cloudprovider opgenomen in de`$event`. You can access them using
`$event->getInput()` en behandel het zoals gewoonlijk`input` inJoomla!.

Nadat je de`input`hebt ontvangen, kan je het gebruiken om de details van
uw cloudservice ophalen, zoals **Access Token**, **Refresh Token** enz.

Als u meerdere accounts wilt onderscheiden, kunt u daarvoor ` Session `
gebruiken.

**Note**: Het wordt aangeraden om te controleren tegen **CSRF token**
voordat je doorgaat met je request. De meeste cloudproviders
ondersteunen de parameter ` status ` die kan worden gebruikt om de taak
te vervullen.

### Gemeenschappelijke valkuilen

Het is verplicht om `urlencode()` je redirect uri wanneer je het naar de
cloudprovider verzendt. Gebruik het om wangedrag van je cloud te
voorkomen.

## Bestand Serveren vanaf uw adapter

Om uw mediabestanden van Media Manager aan te biede **Joomla! Site**
`Joomla\Component\Media\Administrator\Adapter\AdapterInterface` helpt
je. Deze interface bevat een speciale methode genaamd`getUrl($path)`.

`$path : Pad is relatief ten opzichte van je root`

De bedoeling van de methode is om een ​​ *'Public Absolute URL'* te geven
voor het bestand dat u in de site wilt invoegen. Veronderstel
bijvoorbeeld dat uw bestandspad ` /path/to/me.png ` is in de
cloud-server. Nu kan een openbare URL zoiets zijn als
` mycloud.com/share/u/myusername/path/to/me.png `. Hoe het wordt
geserveerd, is helemaal aan jou. Wanneer een gebruiker een door media
gegenereerde URL wil invoegen, wordt deze methode gebruikt.

Meer details over `Adapter Interface` kunnen worden gevonden
in`administrator/componenents/com_media/Adapter/AdapterInterface.php`
