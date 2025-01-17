<!-- Filename: Working_with_git_and_github / Display title: Werken met git en github -->

## Introductie

Dit document zal informatie verschaffen over het bijdragen aan de Joomla! CMS met behulp van Git en GitHub. Als je een eenvoudige wijziging wilt maken (slechts één bestand), is het gemakkelijker om deze documentatie te gebruiken: [Using the Github UI to Make Pull Requests](https://docs.joomla.org/Using_the_Github_UI_to_Make_Pull_Requests). Als je meer complexe wijzigingen wilt toevoegen of je bent hier gewoon in geïnteresseerd, lees dan verder!

## Wat zijn Git en GitHub?

Git is een gedistribueerd versiebeheersysteem. Het is een systeem dat wijzigingen in bestanden bijhoudt en deze wijzigingen opslaat in een geschiedenisbestand. Je kunt altijd terugkijken naar een eerdere versie van je code en wijzigingen herstellen als je dat wilt. Vanwege het geschiedenisarchief is Git zeer nuttig wanneer je met veel mensen samenwerkt aan hetzelfde project.

Hier is hoe GitHub kan worden gebruikt. [GitHub](https://www.github.com) is een website die helpt bij het beheren van Git-projecten op een visuele manier. Als eigenaar van een project kun je de code wijzigen en verschillende versies vergelijken. Als gebruiker van het project kun je bijdragen door een Pull Request te maken. Een pull request is een verzoek aan de eigenaar om wat code in het project op te nemen. Je biedt een stuk code aan (misschien een oplossing voor een bug) en vraagt of de projecteigenaar het wil gebruiken. Als de eigenaar het leuk vindt, kan hij het samenvoegen (toevoegen) aan zijn project.

Joomla! gebruikt GitHub en Git om zijn code te onderhouden. Iedereen kan bijdragen aan Joomla! software via zijn [Github-repository](https://github.com/joomla/joomla-cms).

## Meld je aan bij GitHub en installeer Git

Eerst moet je je [registreren bij Github](http://www.github.com). Het is gratis en eenvoudig te doen. Volg gewoon de aangegeven stappen.

Zodra je bent aangemeld, moet je Git installeren op de computer die je gebruikt voor ontwikkeling (werkstation of laptop). Volg de installatie-instructies op de [git](http://git-scm.com) website. Git is een CLI (Command Line Interface) programma. In het begin kan dit verwarrend en een beetje eng zijn, maar dit document zal je door het proces leiden.

Als je geen gevorderde gebruiker bent, start dan de installer en druk op de "volgende" knoppen totdat het programma is geïnstalleerd. Git zal je systeem niet beschadigen. Later kun je het verwijderen zoals elk ander programma.

Met Git geïnstalleerd, open je een Terminal-applicatie. Begin met het instellen van je Git-naam en e-mailadres. Git zal deze informatie gebruiken wanneer je bijdraagt aan een project:

```sh
    git config --global user.name "Your name"
    git config --global user.email youremail@mail.com
```

In de bovenstaande commando's, en alle andere commando's die in deze documentatie worden gegeven, is elke regel een nieuw commando. Dus je typt de eerste regel, drukt op enter en typt vervolgens de tweede regel en drukt op enter.

Je bent nu klaar om Git te gebruiken en verder te gaan met het instellen van je testinstallatie.

## Opzetten van een testinstallatie

Voor uw testinstallatie heeft u een softwarestack nodig zodat u Joomla! op uw computer kunt installeren en uitvoeren. Stacks zoals MAMP, XAMP of WAMP worden elders in deze documentatie behandeld.

Na de installatie van je softwarestack moet je de nieuwste versie van Joomla! installeren. Dit is niet de laatste stabiele release. De laatste versie van Joomla! is de ontwikkelingsversie op GitHub.

## Fork en clone Joomla!

Op GitHub kun je projecten vinden in zogenaamde Repositories. Binnen een project kun je verschillende versies vinden. Zo'n versie wordt een Branch genoemd. Joomla! heeft de volgende branches:

- **4.2-dev** Bestanden voor de ontwikkeling van de huidige versie.
- **4.3-dev** Bestanden voor de ontwikkeling van de volgende kleine versie.
- **4.4-dev** Bestanden voor de ontwikkeling van de een-na-volgende kleine versie.
- **5.0-dev** Bestanden voor de ontwikkeling van de volgende grote versie.

Op je testcomputer ga je de **4.2-dev** branch gebruiken. Je kunt deze branch echter niet wijzigen, omdat je niet de eigenaar ervan bent. Je moet er een kopie van maken. Op GitHub wordt dit een Fork genoemd. Je bent de eigenaar van die kopie, dus je kunt deze wijzigen. Na het wijzigen van je fork kun je een Pull Request indienen voor de veranderingen die je hebt aangebracht. Daarover later meer. Je kunt een branch forken door op de Fork-knop in de [Joomla! CMS Github Repository](https://github.com/joomla/joomla-cms) te drukken. Deze knop bevindt zich rechtsboven op de pagina.

![Fork joomla in github](../../../en/images/getting-started/core-git-fork-joomla.png)

Na het forken moet je Joomla! installeren op je lokale computer. Ga naar de map waar je bestanden kunt plaatsen die door je webserver worden gebruikt. Veel programma's gebruiken een map genaamd `htdocs`. Sommigen gebruiken `www` en sommigen gebruiken geheel andere mappen. Het hangt allemaal af van of je Windows, Mac of Linux gebruikt. Uiteindelijk zal je webroot verschillende mappen bevatten voor verschillende websites. Zodra je in je webrootmap bent, gebruik je in een open Terminal-venster het cd-commando om de huidige map te wijzigen naar de webroot. Of, in je bestandsverkenner GUI, zoek de webrootmap, druk op de rechtermuisknop en klik op: "Git Bash Here" of "Open Terminal" of iets dergelijks.

In de Terminal, met de site-rootmap ingesteld als de huidige map, typ je de volgende opdracht om de bestanden van je Fork van de Joomla! CMS naar je computer te downloaden. Vervang *username* door de gebruikersnaam die je gebruikt op GitHub.

```sh
    git clone https://github.com/username/joomla-cms.git
```

Het kloonproces kan enige tijd duren. Wanneer het voltooid is, zal je webroot een map bevatten genaamd joomla-cms. Je moet die map de huidige map maken om git-commando's voor die map uit te voeren:

```sh
    cd joomla-cms
```

Denk eraan dat voor andere commando's in deze documentatie.

## Joomla! installeren

Je gedownloade clone van je geforkte repository heeft verdere actie nodig voordat het klaar is voor gebruik. Een van de gedownloade bestanden is genaamd README.md. Open het met een teksteditor en volg de instructies in de sectie **Hoe een werkende installatie vanaf de bron te krijgen**.

Wanneer u klaar bent, opent u uw browser en gaat u naar de installatie op uw localhost. Meestal is de URL iets zoals:
`http://localhost/joomla-cms`. U ziet nu het standaard Joomla!-installatieproces.

## Wijzigingen in de code aanbrengen

Alle wijzigingen die je aanbrengt in de Joomla-code op je lokale site worden geregistreerd en gemonitord door Git. Op elk moment kun je het commando `git status` typen om te zien welke bestanden zijn gewijzigd of niet gevolgd. Niet gevolgd betekent dat het bestand op die locatie nieuw is voor Git.

In dit stadium is het waarschijnlijk het beste om een Integrated Development Environment (IDE) te gebruiken om aan Joomla Code te werken. Visual Studio Code (VSCode) wordt sterk aanbevolen. Het is gratis en werkt op alle platformen. Met VSCode kun je wijzigingen aanbrengen, **Commit** naar je lokale Git-kloon en **Push** naar je externe GitHub-fork.

## Wijzigingen naar de Fork pushen

Het uploaden van wijzigingen wordt in Git-termen een `push` genoemd. Voordat je dat kunt doen, moet je iets heel belangrijks doen. Je moet een nieuwe branch aanmaken voor je wijzigingen. (Een branch is een versie van je project, weet je nog?) Als je dat niet doet en je wijziging direct in de huidige branch maakt, zal er de eerste keer geen probleem zijn. Maar als je een tweede keer wijzigingen aanbrengt en de wijzigingen die je eerste keer hebt gemaakt zijn nog niet samengevoegd, zullen al deze wijzigingen ook worden geregistreerd als nieuwe wijzigingen.

U kunt VSCode instellen om al de volgende opdrachten met een paar klikken uit te voeren. Maar als u git-opdrachten vanaf de terminal-opdrachtregel wilt gebruiken:

Dus de eerste opdracht die je gaat uitvoeren zal een nieuwe branch aanmaken. Vervang name-new-branch met de naam van de nieuwe branch. Deze naam moet kort zijn en mag alleen kleine letters en cijfers bevatten. Gebruik **GEEN** spaties. Gebruik in plaats van spaties een - (minus).

```sh
    git checkout -b name-new-branch
```

De volgende opdracht vertelt git dat alle wijzigingen klaar zijn om te committen.

```sh
    git add --all
```

De volgende opdracht voegt je wijziging toe aan de branch. Vervang het bericht a.u.b. door een korte beschrijving van je wijzigingen. Deze beschrijving zal de titel zijn van de pull-aanvraag die je gaat maken.

```sh
    git commit -m "description"
```

De laatste opdracht zal de wijzigingen naar je fork pushen (uploaden). Vervang name-new-branch met de naam van de branch die je een paar stappen hierboven hebt gemaakt.

```sh
    git push origin name-new-branch
```

## Vergelijk Forks en Maak een Pull Request

Nadat je je wijziging naar GitHub hebt gepusht, ga je naar je fork van de Joomla! CMS op de GitHub-site. Selecteer je branch en maak een pull request.

Wanneer je klaar bent, schakel in je lokale kloon terug naar de oorspronkelijke branch:

```sh
git checkout 4.2-dev
```

Je kunt nu nieuwe wijzigingen aanbrengen zonder de wijzigingen in je vorige branch te beïnvloeden.

## Op de hoogte blijven

Omdat de huidige versie van Joomla! elke dag kan veranderen, is het belangrijk om je fork up-to-date te houden. Je kunt dat doen door de oorspronkelijke Joomla repository toe te voegen aan je geforkte project:

```sh
    git remote add upstream https://github.com/joomla/joomla-cms.git
```

Werk vervolgens je lokale kloon bij met het volgende commando:

```sh
    git pull upstream 4.2-dev
```

En update je externe fork:

```sh
    git push
```

*Vertaald door openai.com*

