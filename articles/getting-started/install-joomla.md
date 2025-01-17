<!-- Filename: J4.x:Developer:_Required_Software / Display title: Installeer Joomla -->

## Samenvatting

Dit is slechts een korte samenvatting van de stappen die betrokken zijn:

- **Download** de nieuwste volledige versie van de [Joomla Downloads](https://downloads.joomla.org/) pagina.
- **Bewaar** het downloadbestand in je Document root of een submap van de root.
- **Pak** het downloadbestand ter plaatse uit. Sommige besturingssystemen maken een map met dezelfde naam als het download-zipbestand, minus de zip. Dat is goed - je kunt de map eenvoudig hernoemen met een korte naam en verplaatsen indien nodig. Andere besturingssystemen pakken de bestanden en mappen ter plaatse uit. Dat is slecht als je de download in je rootmap hebt geplaatst waar zich bestaande mappen met andere sites bevinden. Je zult een korte mapnaam moeten maken en alle nieuw uitgepakte bestanden en mappen erin moeten verplaatsen. Het doel is om te eindigen met een map die je kunt openen met je webbrowser via localhost/j4test.
- **Installeer** wijs je browser naar localhost/j4test en vul de Joomla-installatieformulieren in.

## Configuratie

Enkele suggesties:

- **Globale Configuratie / Site / Cookie / Cookiepad** ingesteld op de map die je Joomla-installatie bevat (/j4test).
- **Globale Configuratie / Systeem / Debug / Debugsysteem** ingesteld op Ja.
- **Globale Configuratie / Systeem / Sessie Levensduur** ingesteld op 60 - anders word je uitgelogd tijdens het nadenken.
- **Globale Configuratie / Server / Server / Foutrapportage** ingesteld op Maximum.
- **Plugin Systeem - Debug / Plugin / Ververs Assets** ingesteld op Nee, tenzij je css of javascript debugt.

Dat is alles. Als Joomla werkt, ben je klaar om het te gebruiken voor de ontwikkeling van extensies.

## Meer Sites

Je kunt zoveel sites installeren als je wilt op een enkele computer. Afhankelijk van hoe je je server hebt ingesteld, kan dat via verschillende submappen die worden benaderd via localhost/test1, localhost/test2 enzovoort of via virtuele hosts die worden benaderd via test1.localhost, test2.localhost enzovoort. Hoe dan ook, je kunt binnen enkele minuten een nieuwe database en een nieuwe schone Joomla-site hebben. Kies veelzeggende korte namen! *test1* en *test2* worden al snel verwarrend.

## Installatie voor Testen

Er is een andere procedure als je wilt helpen met het testen van Joomla. In dit geval moet je **git** installeren en de instructies volgen in de [GitHub Repository](https://github.com/joomla/joomla-cms).

- selecteer in GitHub de Joomla-branch om aan te werken. Dat verandert de verderop op de pagina weergegeven instructies.
- Volg op je laptop of desktop de instructies vanaf *Clone the repository:*.
- Verwijder de Installatiemap niet aan het einde van de Joomla-installatie!
- Hernoem de clone van joomla-cms naar iets anders zodat je het later opnieuw kunt doen.
- Installeer de Joomla Patchtester.

*Vertaald door openai.com*

