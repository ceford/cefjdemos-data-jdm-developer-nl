<!-- Filename: Working_with_git_and_github / Display title: Werken met git en github -->

# Introductie

Dit document geeft informatie over het bijdragen aan het Joomla! CMS met
behulp van Git en Github. Als u graag op een eenvoudige wijziging (op
een enkel bestand) wilt doen is het beter om deze documentatie te
raadplegen: <a
href="https://docs.joomla.org/Special:Nl/Using_the_Github_UI_to_Make_Pull_Requests"
class="new"
title="Special:Nl/Using the Github UI to Make Pull Requests (page does not exist)">De
Github UI gebruiken om een Pull Request in te dienen</a> Als u meer
complexe wijzigingen wilt doorvoeren of geïnteresseerd bent, lees dan
vooral door!

## Wat zijn Git en GitHub

Git is een gedistribueerd versiebeheersysteem. Het is een systeem dat
wijzigingen in bestanden registreert en deze wijzigingen in een
historisch bestand opslaat. U kunt altijd terug kijken in een eerdere
versie van uw code en indien gewenst wijzigingen herstellen. Door het
historisch archief is Git erg handig als je met veel mensen samenwerkt
in hetzelfde project.

Dit is hoe GitHub gebruikt kan worden.
<a href="https://www.github.com" class="external text" target="_blank"
rel="nofollow noreferrer noopener">GitHub</a> is een website die bij het
beheren van Gitprojecten op een visuele manier helpt. Als een eigenaar
van een project kan je de code wijzigen en vergelijken met verschillende
versies. Als een gebruiker van een project kunt u door het maken van een
Pull Request een bijdrage leveren. Een Pull Request is een verzoek aan
de eigenaar om wat code in het project te plaatsen. U biedt een stuk
code aan (misschien de oplossing voor een bug) en hiermee vraagt u de
projecteigenaar of hij deze code wilt gebruiken. Als de eigenaar het wil
kan hij deze code samenvoegen (merge) in zijn project.

Joomla! gebruikt GitHub en Git om zijn code te beheren. Iedereen kan
bijdragen aan de Joomla! software. De URL naar het Joomla! CMS Project
op GitHub is:
<a href="https://github.com/joomla/joomla-cms" class="external free"
target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla/joomla-cms</a>

## Aan de slag

## Aanmelden op GitHub en Git installeren

Als eerste moet u zich op
<a href="http://www.github.com" class="external text" target="_blank"
rel="nofollow noreferrer noopener">GitHub</a> registreren. Het is gratis
en erg makkelijk. Volg de opgegeven stappen.

Nu u aangemeld bent kunt u Git installeren. De laatste versie van Git
kan gevonden worden op
<a href="http://git-scm.com" class="external free" target="_blank"
rel="nofollow noreferrer noopener">http://git-scm.com</a>. Download en
start het installatiebestand. Git is een CLI (command line interface)
programma. In het begin kan dit verwarrend en angstig zijn maar dit
document zal u door het proces leiden.

Als u niet een geavanceerde gebruiker bent kunt u gewoon het
installatiebestand starten en op de "Volgende" knop drukken totdat het
programma geïnstalleerd is. Git zal uw systeem niet schaden. U kunt het
later, indien gewenst, verwijderen zoals u dat met andere programma's
ook doet.

Zodra we Git hebben geïnstalleerd kunnen we het programma genaamd "Git
Bash" starten. Er zal een commandoregel geopend worden. We gaan Git onze
naam en e-mailadres geven. Git zal deze gegevens gebruiken wanneer we
een bijdrage leveren aan een project. Met de volgende commandos kunnen
we Git deze gegevens geven±

    git config --global user.name "Your name"
    git config --global user.email youremail@mail.com

In de bovenstaande commandos, en bij alle overige commandos die in deze
documentatie staat, zal een nieuwe regel een nieuw commando zijn. Dus u
typt de eerste regel, drukt op enter en begint met de tweede regel en
drukt weer op enter.

We zijn nu klaar om Git te gebruiken en gaan verder met het instellen
van onze testinstallatie.

# Instellen van de testinstallatie

Voor onze testinstallatie hebben we een webserver programma nodig zodat
we Joomla op onze computer kunnen installeren. Er zijn verschillende
programma's die dit kunnen verzorgen zoals
<a href="https://www.mamp.info/MAMP" class="external autonumber"
target="_blank" rel="nofollow noreferrer noopener">[1]</a> en
<a href="https://www.apachefriends.org/XAMPP"
class="external autonumber" target="_blank"
rel="nofollow noreferrer noopener">[2]</a>. Download en installeer één
van deze programma's.

Na de installatie van een programma als dat (in dit voorbeeld wordt MAMP
gebruikt) kunnen we de laatste versie van Joomla! installeren. In ons
geval is de laatste versie van Joomla! niet de stabiele versie. De
laatste versie van Joomla! is de "staging branche" op GitHub. We gaan
eerst GitHub wat beter uitleggen.

## "Fork" en kloon Joomla!

Op GitHub kunt u verschillende projecten vinden, de zogenaamde
"Repositories". In een project kunt u verschillende versies vinden. Zo'n
versie heet een "branch". Joomla! heeft de volgende "branches":

- "'Staging:"' Deze "branch" bevat de laatste bugfixes en nieuwe
  features van Joomla!
- "'Master:"' Deze branche is de huidige stabiele versie van Joomla!
- "'3.5-dev"' Deze branche bevat de bestanden voor Joomla! 3.5 welke, op
  moment van schrijven, nog niet de stabiele versie is.

Op onze testlocatie gaan we de **Staging** "branche" gebruiken maar als
we deze "branche" direct gaan gebruiken dan hebben we een probleem. We
kunnen deze "branche" niet aanpassen omdat we niet de eigenaar zijn. We
maken er een kopie van. Op GitHub heet dit een "Fork". Hier zijn we dan
eigenaar van en kunnen het dus wijzigen. Na het wijzigen kunnen we onze
"Fork" vergelijken met het originele project. Nu kunnen we een "Pull
Request" aanmaken voor de wijzigingen die we hebben gemaakt. Daarover
later meer. U kunt van een "branche" een "Fork" maken door op de "Fork"
knop te drukken op de
<a href="https://github.com/joomla/joomla-cms" class="external text"
target="_blank" rel="nofollow noreferrer noopener">Joomla! CMS Github
Repository</a>. Deze knop bevindt zich aan de rechter bovenkant van de
pagina.

<img src="https://docs.joomla.org/images/6/6c/Github-fork-button.png"
decoding="async" data-file-width="1002" data-file-height="222"
width="1002" height="222" alt="Github-fork-button.png" />

Nadat we de "Fork" hebben aangemaakt kunnen we Joomla! op onze lokale
server installeren. Ga naar de map waar u de bestanden van de webserver
kan uitvoeren. De meeste programma's gebruiken een map als `htdocs`.
Zodra we in deze map zitten, klik op de rechtermuis toets en klik op:
"Git Bash Here". De command line zal openen op deze locatie.

Typ de volgende opdracht om de bestanden van de "Fork" van het Joomla!
CMS naar uw computer te downloaden. Vervang *Gebruikersnaam* met de
gebruikersnaam die u gebruikt op GitHub.

    git clone https://github.com/username/joomla-cms.git

U zal Git moeten openen via de bovenstaande beschreven stappen om alle
commando's uit te voeren die in deze documentatie worden gegeven.
Onthoudt dat voor alle andere commando's in deze documentatie.

Als Git klaar is kunt u de browser starten en naar de installatie op uw
localhost gaan. Meestal is de URL iets als:
<a href="http://localhost/joomla-cms" class="external free"
target="_blank"
rel="nofollow noreferrer noopener"><code>http://localhost/joomla-cms</code></a>.
U zult nu een standaard Joomla! installatie zien.

## Installeer Joomla!

De installatie van Joomla! op onze lokale testinstallatie is bijna
hetzelfde als een reguliere installatie. Er zijn twee kleine
verschillen.

Bij de database-instellingen zijn de gebruikersnaam en wachtwoord
standaard. Meestal is de gebruikersnaam `root` en het wachtwoord is ook
`root` of er is geen wachtwoord. Als u nog steeds de database niet kan
bereiken dan kunt u in de handleiding van uw lokale webserver kijken
voor de gebruikersnaam en het wachtwoord.

Het laatste verschil is de laatste stap van de installatie. Normaliter
moeten we de installation-map verwijderen om verder te gaan naar de
backend of frontend van onze website. Voor de testinstallatie kunnen we
dit overslaan en kunnen we direct naar de backend of front-end gaan.
Verwijder de installation-map niet. Dit kan erg nuttig zijn bij het
opnieuw installeren van Joomla!

# Maak uw wijzigingen

Nu is het tijd om ons Joomla! bestand te wijzigen. Alle wijzigingen die
gemaakt worden zullen worden geregistreerd door Git. Op elk moment kunt
u via de commando `git status` zien welke bestanden gewijzigd zijn en
welke niet worden "Untracked" (gemonitord). "Untracked" betekent dat het
bestand op de locatie nieuw is voor Git.

Als u een vergissing heeft gemaakt of een bestand wilt herstellen, kunt
u dit commando gebruiken:

    git checkout path/to/file

Als u de wijzigingen die u hebt gemaakt wilt verwijderen, gebruik dan
het volgende commando:

    git checkout .
    git clean -f -d

Het eerste commando reset alle bestanden. Het tweede verwijdert alle
"untracked" bestanden en mappen.

## Onze wijzigingen publiceren op GitHub

# "Push" de wijziging naar de "fork"

Nadat alle wijzigingen gemaakt zijn moeten we onze wijzigingen uploaden
naar onze "repository" op GitHub. Hierna kunnen we een "pull request"
maken met onze wijzigingen.

Het uploaden van de wijzigingen heet `"push"` in Git terminologie.
Voordat we dat kunnen doen moeten we eerst iets anders belangrijks doen.
We moeten een nieuwe "branch" aanmaken voor onze wijzigingen. (Een
"branche" is een versie van ons project, weet u nog?) als we dit niet
doen en we maken onze wijziging direct in de "staging branch" dan zal de
eerste keer daar geen probleem mee zijn. Maar wanneer we wijzigingen
voor de tweede keer maken en eerste wijzigingen zijn nog niet "gemerged"
zullen al die wijzigingen ook als nieuwe wijzigingen beschouwd worden.

Dus onze eerste commando dat we uitvoeren is het creëren van een nieuwe
"branche". Het voorkomt het hier boven beschreven probleem. Vervang
"name-new-branche" met de naam van uw nieuwe "branche". Deze naam moet
kort zijn en kan alleen kleine letters en getallen bevatten. Gebruik
**GEEN** spaties. In plaats van spaties kan de - (minus) gebruikt
worden.

    git checkout -b name-new-branch

De volgende commando verteld git dat alle wijzigingen goed zijn en klaar
staan om te worden "commit".

    git add --all

Het volgende commando voegt onze wijziging toe aan de "branche". Vervang
het bericht door een korte beschrijving van de wijzigingen die gemaakt
zijn. Deze beschrijving zal de titel zijn van de "pull request" die we
gaan maken.

    git commit -m "description"

Het laatste commando zal de wijzigingen uploaden ("push") naar onze
"fork" Vervang alstublieft name-new-branche- door de naam van de
"branche" die u een paar stappen eerder gekozen heeft.

    git push origin name-new-branch

## Vergelijk "forks" en doe een "pull request"

Nadat onze wijziging ge"pusht" is naar GitHub kan u naar uw Joomla! CMS
"fork" gaan.

# Meer informatie

Omdat de "staging' versie van Joomla! op elk moment kan wijzigen is het
erg nuttig om de mogelijkheid te hebben om onze "fork" up-to-date te
kunnen houden. We kunnen dit doen door een "remote" toe te voegen aan
onze project "fork".

    git remote add upstream https://github.com/joomla/joomla-cms.git

We hebben nu een "remote" genaamd "upstream" toegevoegd. Met het
volgende commando kunnen we Git laten zoeken naar nieuwe "commits" in de
"staging branch" die we niet in onze "fork" hebben.

    git pull upstream staging

De wijzigingen zijn nu alleen in onze lokale "fork" gemaakt. We kunnen
ze met het volgende commando naar GitHub uploaden:

    git push

  
<a
href="https://docs.joomla.org/index.php?title=Categorie:_Bug_Squad/nl_Bug_Squad&amp;action=edit&amp;redlink=1"
class="new"
title="Categorie: Bug Squad/nl Bug Squad (page does not exist)">Categorie:
Bug Squad/nl Bug Squad</a> <a
href="https://docs.joomla.org/index.php?title=Categorie:_Ontwikkeling/nl_Ontwikkeling&amp;action=edit&amp;redlink=1"
class="new"
title="Categorie: Ontwikkeling/nl Ontwikkeling (page does not exist)">Categorie:
Ontwikkeling/nl Ontwikkeling</a> <a
href="https://docs.joomla.org/index.php?title=Categorie:GitHub/nl_GitHub&amp;action=edit&amp;redlink=1"
class="new"
title="Categorie:GitHub/nl GitHub (page does not exist)">Categorie:GitHub/nl
GitHub</a>
