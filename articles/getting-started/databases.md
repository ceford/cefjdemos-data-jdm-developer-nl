<!-- Filename: J4.x:Developer:_Required_Software / Display title: Database Setup -->

## Over MySQL en MariaDB

Nieuwkomers kunnen de indruk krijgen dat MySQL en MariaDB databases zijn en kunnen in de war raken wanneer instructies zeggen *maak een nieuwe database*. In feite zijn beide Relationele Database Beheer Systemen (RDBMS) die veel individuele databases kunnen beheren. Je lokale testsite kan veel individuele Joomla-installaties hebben, elk met een eigen database. Je wilt bijvoorbeeld je extensie testen in de huidige Joomla-release en apart in een aankomende releasekandidaat.

De onderstaande screenshot toont een deel van een lijst met meer dan 30 databases, gecreëerd voor het testen van verschillende Joomla-installaties en extensieprojecten.

![Phypadmin screenshot of list of databases](../../../en/images/getting-started/phpmyadmin-databases.png)

Een bijkomende opmerking: de sorteringen zijn meestal utf8mb4_0900_ai_ci:

- utf8mb4 betekent dat elk teken wordt opgeslagen als maximaal 4 bytes in het UTF-8 coderingsschema.
- 0900 verwijst naar de versie van het Unicode Collation Algorithm.
- ai verwijst naar accentongevoeligheid, er is geen verschil tussen e, è, é, ê en ë bij het sorteren.
- ci verwijst naar hoofdletterongevoeligheid, er is geen verschil tussen p en P bij het sorteren.

Individuele tabellen en kolommen kunnen een andere collatie hebben. Joomla-tabellen hebben doorgaans utf8mb4_unicode_ci-collatie.

## Database-instelling met phpMyAdmin

Voordat u Joomla installeert, moet u een lege database hebben die klaar is om gevuld te worden. U heeft ook een databasegebruiker nodig.

### Een database maken

- **Start phpMyAdmin** Voer localhost/phpmyadmin in de URL-balk van je browser in.
- **Inloggen** Afhankelijk van hoe het is geïnstalleerd, zal de gebruikersnaam root of admin zijn, en er kan al dan niet een wachtwoord zijn.
- Selecteer **Databases** in het bovenste menu van de phpMyAdmin Startpagina.
- Voer in **Database aanmaken** een korte naam in op de plaats van de **Databasenaam** hint, bijvoorbeeld, jtest.
- Selecteer **utf8mb4_0900_ai_ci** uit de Sorteer volgorde keuzelijst.
- Selecteer **Aanmaken** om de database te maken.

Dat brengt je naar een lege database. Bovenaan is er een bericht: *Geen tabellen gevonden in database.*

### Een gebruiker aanmaken

- Selecteer het **Home**-pictogram linksboven in phpMyAdmin om naar de Home-pagina te gaan.
- Selecteer **Gebruikersaccounts** in het bovenste menu van de Home-pagina.
- Selecteer **Gebruikersaccount toevoegen** in het Nieuwe paneel onder de lijst met huidige gebruikers (indien aanwezig).
- Voer een **Gebruikersnaam** in, die een korte naam kan zijn die je later tijdens de installatie van Joomla nodig hebt. Voorbeeld: jtest. Je kunt dezelfde gebruiker gebruiken voor andere databases die later worden aangemaakt!
- Selecteer **Lokaal** uit de Hostnaam-veld lijst. Het zal localhost instellen in het naastliggende waarderveld.
- Gebruik de **Genereer**-knop om een wachtwoord te genereren. Je moet de gegenereerde waarde kopiëren en plakken in een teksteditor om later te gebruiken. Je hoeft het niet langdurig te onthouden, aangezien het als platte tekst wordt opgeslagen in het Joomla configuratie.php-bestand. Voorbeeld: 8t8mGQq.gw\[\]8lp(
- **Opslaan** sla de Database voor gebruiker en Wereldwijde rechten secties van het formulier over. De **Go** (Opslaan) knop bevindt zich onderaan.

### Toewijzen van database-permissies aan de gebruiker

- Op de **Gebruikersaccounts**-pagina selecteert u de gebruiker (jtest), indien nodig via Home / Gebruikersaccounts / Gebruikersnaam.
- Selecteer **Database** nabij de bovenkant van de pagina. Dit toont een lijst van databases waarvoor deze gebruiker toegangsrechten heeft gekregen en, onderaan, een lijst van databases waarvoor de gebruiker geen toegang heeft gekregen.
- **Selecteer** de database waartoe toegang moet worden verleend en selecteer de **Ga**-knop.
- **Databasespecifieke privileges** Selecteer **Alles aanvinken** en vervolgens **Ga** om op te slaan.

Dat is het! U kunt nu afmelden bij phpMyAdmin. Bent u vergeten het wachtwoord van de databasegebruiker op te schrijven? Ga terug en bewerk het gebruikersaccount dat u hebt aangemaakt om het te wijzigen. Als u dezelfde databasegebruiker voor meerdere Joomla-databases gebruikt, vindt u het wachtwoord in een Joomla configuration.php-bestand.

*Vertaald door openai.com*

