<!-- Filename: Deploying_an_Update_Server / Display title: Update-servers -->

## Achtergrond

Er is een goede beschrijving van Update Servers in de Joomla! Programmeursdocumentatie gekopieerd naar [deze site](jdocmanual?article=docus/install-update/update-server) of beschikbaar in de [oorspronkelijke bron](https://manual.joomla.org/docs/building-extensions/install-update/update-server/).

## Problemen oplossen

- **SQL-update script wordt niet uitgevoerd tijdens update.**

Als het SQL-update script (bijvoorbeeld in de map `sql/updates/mysql`) niet wordt uitgevoerd tijdens het updateproces, kan het zijn dat er geen versienummer in de `#_schemas` tabel staat voor deze extensie *vóór de update*. Deze waarde wordt bepaald door de naam van het laatste script in de SQL-updatemap. Als deze waarde leeg is, zullen er tijdens die updatecyclus geen SQL-scripts worden uitgevoerd. Om ervoor te zorgen dat deze waarde correct is ingesteld, zorg ervoor dat je een SQL-script in deze map hebt met de naam als het versienummer (bijvoorbeeld 1.2.3.sql als de versie 1.2.3 is). Het bestand kan leeg zijn of alleen een SQL-opmerkingsregel bevatten. Dit moet worden gedaan in de oude versie — degene vóór de update. Als alternatief kun je deze waarde toevoegen aan de `#_schemas` met behulp van een SQL-query.

*Vertaald door openai.com*

