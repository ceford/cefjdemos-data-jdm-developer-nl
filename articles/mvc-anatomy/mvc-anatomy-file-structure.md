<!-- Filename: J4.x:MVC_Anatomy:_File_Structure / Display title: MVC Anatomie: Bestandsstructuur -->

## Ontwikkelaarsinstelling

Er zijn verschillende manieren om bestanden te organiseren voor ontwikkeldoeleinden. Een methode is het gebruik van een kloon van een GitHub-repository zoals in de volgende schematische structuur:

```
cefjdemos-com-countrybase
|-- .vscode (folder for build settings)
|-- com_countrybase (folder compressed to create an installable zip file)
    |-- admin (the administrator files)
        |-- forms
        |-- help
            |-- countrybase.html (this is a Help screen used in the backend module edit form)
        |-- language
            |-- en-GB (language folder, kept with the extension code)
                |-- mod_downmsg.ini
                |-- mod_downmsg.sys.ini
        |-- services (folder for dependency injection)
            |-- provider.php (the DI code)
        |-- sql
            |-- updates
                |-- mysql
                    |-- index.html (an empty file required to avoid an installation error if there are no sql updates)
        |-- src (folder for namespaced classes)
            |-- more folders: Controller, Extension, Helper, Model, Table and View
        |-- tmpl (folder for layouts)
            |-- more folders: countries, country, currencies and currency
        |-- access.xml (standard list of access permissions)
        |-- config.xml (options parameters)
    |-- media
        |-- css (placeholder for CSS files - contains an empty index.html file)
        |-- js (placeholder for JavaScript files - contains an empty index.html file)
    |-- site (the site files, abbreviated here)
        |-- forms
        |-- language
        |-- src
        |-- tmpl
    |-- countrybase.xml (the manifest file)
    |-- script.php (a script to run on install, update or uninstall)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- changelog.xml (record of changes)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
|-- updates.xml (update server specification)
```

Dit is de structuur in de VSCodium IDE:

![Vscodium file structure view](../../../en/images/mvc-anatomy/com-countrybase-vscodium.png)

Bij installatie worden de onderdelen van de `com_countrybase` component verdeeld over verschillende locaties in de Joomla-bestandenstructuur:
- Administratorbestanden gaan naar `root/administrator/components/com_countrybase`.
- Sitebestanden gaan naar `root/components/com_countrybase`.
- Mediabestanden, javascript en css-bestanden (indien aanwezig) gaan naar `root/media/com_countrybase`.
- Taalbestanden gaan in de administrator- en sitecomponentstructuren. Een eerdere conventie plaatste ze in de kern taal mappen.

Waar de items precies naartoe gaan, wordt gecontroleerd door het componentmanifestbestand, in dit geval countrybase.xml.

Merk op dat het zipbestand alles binnen de com_countrybase map bevat. Je kunt een zip maken door eenvoudig die map te comprimeren. Buiten die map bevinden zich build.xml, een bestand dat wordt gebruikt om de component te bouwen wanneer er een wijziging is aangebracht, en README.md, een standaard Markdown-bestand in Github-formaat dat de component beschrijft.

## Aantekeningen

- Er zijn meer dan 40 bestanden in deze eenvoudige component!
- Bij de installatie wordt het countrybase.xml-bestand gekopieerd naar administrator/components/com_countrybase, waar het nodig is voor update- of deïnstellingsdoeleinden.

*Vertaald door openai.com*

