<!-- Filename: https://docs.joomla.org/Package / Display title: Pakketten -->

## Inleiding tot Pakketten

Soms gebruikt een module (of plugin of andere extensie) de modellen of helpers van een component. In deze gevallen is het het beste om een afhankelijke module te kunnen installeren of deïnstalleren wanneer het component zelf wordt geïnstalleerd of gedeïnstalleerd. In Joomla wordt deze functionaliteit bereikt met een pakket, hoewel het gebruik van pakketten niet onfeilbaar is.

Een pakket is een uitbreiding die wordt gebruikt om meerdere uitbreidingen tegelijkertijd te installeren. Door ze in een pakket te combineren, kan de gebruiker twee of meer uitbreidingen in één keer installeren en verwijderen.

## Pakketcreatie

Een pakket wordt gecreëerd door alle afzonderlijke extensie `.zip` bestanden samen te voegen met een `.xml` manifestbestand. Bijvoorbeeld, voor een pakket bestaande uit:

* **component** hellowereld
* **module** hellowereld
* **bibliotheek** hellowereld
* **systeem plugin** hellowereld
* **sjabloon** hellowereld

Het pakket zou de volgende boomstructuur in het zipbestand hebben:

```sh
  -- pkg_helloworld.xml
  -- packages <dir>
      |-- com_helloworld.zip
      |-- mod_helloworld.zip
      |-- lib_helloworld.zip
      |-- plg_sys_helloworld.zip
      |-- tpl_helloworld.zip
  -- pkg_script.php
```

De `pkg_helloworld.xml` zou de volgende inhoud kunnen hebben:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<extension type="package" method="upgrade">
<name>Hello World Package</name>
<author>Hello World Package Team</author>
<creationDate>May 2022</creationDate>
<packagename>helloworld</packagename>
<version>1.0.0</version>
<url>http://www.yoururl.com/</url>
<packager>Hello World Package Team</packager>
<packagerurl>http://www.yoururl.com/</packagerurl>
<description>Example package to combine multiple extensions</description>
<update>http://www.updateurl.com/update</update>
<scriptfile>pkg_script.php</scriptfile>
<blockChildUninstall>true</blockChildUninstall>
<files folder="packages">
    <file type="component" id="com_helloworld">com_helloworld.zip</file>
    <file type="module" id="helloworld" client="site">mod_helloworld.zip</file>
    <file type="library" id="helloworld">lib_helloworld.zip</file>
    <file type="plugin" id="helloworld" group="system">plg_sys_helloworld.zip</file>
    <file type="template" id="helloworld" client="site">tpl_helloworld.zip</file>
</files>
</extension>
```

Wanneer gezipt en geïnstalleerd, zal het pakket zichtbaar zijn in de extensielijst zodat een gebruiker alle extensies in het pakket kan verwijderen.

Vergeet niet om de pakketverwijderaar te gebruiken in plaats van individuele subpakketverwijderaars om te voorkomen dat er verweesde extensie-items in de extensiebeheerder achterblijven. Dit is het deel dat niet waterdicht is!

### Bestands-id

Het id-element in de `<file ..>` tag is **niet** willekeurig! De `id=` moet worden ingesteld op de waarde van de `element` kolom in de `#__extensions` tabel. Als ze niet correct zijn ingesteld, zal het `file`-kind bij deïnstallatie van het pakket **niet** worden gevonden en gedeïnstalleerd.

### Manifest Bestandsnaam en Pakketnaam

De naamgeving van het manifestbestand en de mogelijkheid om het pakketbestand zelf te deïnstalleren zijn nauw met elkaar verbonden. Het manifestbestand moet een **pkg_** voorvoegsel hebben. Het resterende deel van de naam van het manifest (zonder de **.xml** extensie) moet worden gebruikt als `<packagename>`. Of, andersom, een pakket dat u wilt identificeren als **blurpblurp_J3** krijgt dat als zijn `<packagename>` en moet in een manifestbestand genaamd **pkg_blurpblurp_J3.xml** staan. Als u dit niet doet, wordt het onmogelijk om het pakket zelf te deïnstalleren.

Een optionele `<pkg_script.php>` die een klasse `pkg_<packagename>InstallerScript` bevat met de publieke functie **postflight** kan worden gebruikt.

### Extensie-tag

De `<extension>`-tag moet `method="upgrade"` bevatten om een pakketupdate te laten slagen bij volgende installaties.

### Uitbreiding Deïnstallatie Afhankelijkheid

Een pakketextensie kan verklaren dat de kindelementen niet onafhankelijk kunnen worden verwijderd met een `<blockChildUninstall>true</blockChildUninstall>` element in het pakketmanifest. Als jouw pakket al zijn extensies nodig heeft om effectief te functioneren, stel dit dan in op true of 1.

*Vertaald door openai.com*

