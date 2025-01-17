<!-- Filename: API_Guides / Display title: API-gidsen -->

Deze pagina bevat een index naar de set van Joomla API-gidsen. Deze gidsen zijn bedoeld om u te helpen begrijpen hoe u deze Joomla-functies kunt gebruiken in uw eigen Joomla-extensies.

Elke API-gids bevat voorbeeldcode die je kunt kopiëren en installeren in je eigen ontwikkelomgeving. Over het algemeen zijn deze codevoorbeelden geschreven om te worden opgenomen en geïnstalleerd als een Joomla-module, dus als je niet bekend bent met de ontwikkeling van Joomla-modules, kan het nuttig zijn om de korte serie [Een eenvoudige module maken](https://docs.joomla.org/Creating_a_simple_module "Creating a simple module") door te nemen.

Rond Joomla 3.8 begon het Joomla-ontwikkelingsteam de naamgevingsconventie van Joomla-klassen te veranderen om namespaces te gebruiken, zodat bijvoorbeeld JFactory veranderde naar Factory in de Joomla\CMS namespace. Als je bestaande Joomla-code en -documentatie leest, kun je klassen vinden die ofwel de nieuwe of de oude naamgevingsstandaard volgen. Je kunt de mapping tussen de twee naamgevingsconventies vinden in het `libraries/classmap.php` bestand in je Joomla-instantie.

- De basis Applicatieklassen en hun hiërarchie en doeleinden worden beschreven in [Understanding the Application classes](https://docs.joomla.org/J3.x:Understanding_the_Application_classes "J3.x:Understanding the Application classes")
- Ajax-verwerking binnen Joomla Componenten wordt beschreven in [JSON Responses with JResponseJson](https://docs.joomla.org/JSON_Responses_with_JResponseJson "JSON Responses with JResponseJson"). Ajax kan ook worden gebruikt binnen Joomla Modules en Plugins zoals beschreven in [Using Joomla Ajax Interface](https://docs.joomla.org/Using_Joomla_Ajax_Interface "Using Joomla Ajax Interface").
- [Cache](https://docs.joomla.org/Cache_Basic_API_Guide "Cache Basic API Guide") - hoe callbackcache binnen uw code te gebruiken.
- [Categorieën](https://docs.joomla.org/Categories_and_CategoryNodes_API_Guide "Categories and CategoryNodes API Guide") Het gebruik van de `Categories` en `CategoryNode` klassen om toegang te krijgen tot gegevens met betrekking tot Joomla-categorieën
- CSS kan worden toegevoegd zoals beschreven in [Adding JavaScript and CSS to the page](https://docs.joomla.org/Adding_JavaScript_and_CSS_to_the_page)
- Database / JDatabase. Er zijn twee API-gidsen, die [Gegevens selecteren met JDatabase](https://docs.joomla.org/Selecting_data_using_JDatabase "Selecting data using JDatabase") en [Invoegen, bijwerken en verwijderen van gegevens met JDatabase](https://docs.joomla.org/Inserting,_Updating_and_Removing_data_using_JDatabase "Inserting, Updating and Removing data using JDatabase") behandelen
- [Datum/JDate](https://docs.joomla.org/How_to_use_JDate "How to use JDate") is de Datum-klasse van Joomla.
- Bestanden en Mappen. Zie [How to use the filesystem package](https://docs.joomla.org/How_to_use_the_filesystem_package "How to use the filesystem package").
- Formulier / JForm. Er is een [Basisgids](https://docs.joomla.org/Basic_form_guide "Basic form guide") voor het gebruik van de Joomla Form API en het integreren van formulieren binnen een Joomla-component, en ook een meer [Gevorderde gids](https://docs.joomla.org/Advanced_form_guide "Advanced form guide") die meer geavanceerde aspecten van de API's behandelt.
- FormulierVeld / JFormField. Deze klasse en gerelateerde klassen zoals JFormFieldList, die erven erft, zijn voornamelijk nuttig voor het definiëren van aangepaste formuliervelden, zoals beschreven in [Creating a custom form field type](https://docs.joomla.org/Creating_a_custom_form_field_type "Creating a custom form field type").
- [Invoer / JInput](https://docs.joomla.org/Retrieving_request_data_using_JInput "Retrieving request data using JInput") Het gebruik van de `Input` klasse om de waarden van parameters in HTTP GET- en POST-verzoeken te verkrijgen
- JavaScript kan worden toegevoegd zoals beschreven in [Adding JavaScript and CSS to the page](https://docs.joomla.org/Adding_JavaScript_and_CSS_to_the_page)
- Het gebruik van Joomla Layouts wordt beschreven in [Sharing layouts across views or extensions with JLayout](https://docs.joomla.org/J3.x:Sharing_layouts_across_views_or_extensions_with_JLayout "J3.x:Sharing layouts across views or extensions with JLayout"). De flexibiliteit werd vergroot in Joomla 3.2, zoals beschreven in [JLayout Improvements for Joomla!](https://docs.joomla.org/J3.x:JLayout_Improvements_for_Joomla! "J3.x:JLayout Improvements for Joomla!").
- [Log / JLog](https://docs.joomla.org/Using_JLog "Using JLog") Log berichten (bijv. foutmeldingen, debugberichten) naar een logbestand en eventueel naar de debugconsole
- [Menu en Menu-items](https://docs.joomla.org/Menu_and_Menuitems_API_Guide "Menu and Menuitems API Guide")
- [Geneste Sets](https://docs.joomla.org/Using_nested_sets "Using nested sets"), die het mogelijk maken een boomstructuur te implementeren in de databasetabel, worden gebruikt door Joomla-menu's, artikelen, categorieën, etc.
- [Register/JRegistry](https://github.com/joomla-framework/registry) is een hulpprogrammaklasse die zeer nuttig is voor het omgaan met PHP-arrays, conversie naar JSON, etc.
- [JResponseJson](https://docs.joomla.org/JSON_Responses_with_JResponseJson "JSON Responses with JResponseJson") ondersteunt het reageren in JSON-indeling op Ajax-verzoeken.
- Route / JRoute zie [URLs in Joomla](https://docs.joomla.org/URLs_in_Joomla "URLs in Joomla")
- Tabel / JTable biedt functionaliteit voor het uitvoeren van CRUD-bewerkingen (en meer) op databasetabellen. De gids is opgedeeld in een [Basis API Gids](https://docs.joomla.org/Table_Basic_API_Guide "Table Basic API Guide") en een [Gevorderde API Gids](https://docs.joomla.org/Table_Advanced_API_Guide "Table Advanced API Guide")
- De [Controllers](https://docs.joomla.org/Controllers "Controllers") (BaseController, AdminController, FormController, ApiController) zijn verantwoordelijk voor het analyseren van het verzoek van de gebruiker, het controleren of de gebruiker die actie mag uitvoeren en bepalen hoe aan het verzoek kan worden voldaan.
- De [Modellen](https://docs.joomla.org/Models "Models") (BaseModel, BaseDatabaseModel, ItemModel, ListModel, FormModel, AdminModel) omvatten de gegevens die door de component worden gebruikt. Ze zijn ook verantwoordelijk voor het bijwerken van de database indien nodig.
- De [Weergaven](https://docs.joomla.org/Views "Views") (AbstractView, CategoriesView, CategoryFeedView, CategoryView, FormView, HtmlView, JsonApiView, JsonView, ListView) bepalen wat er op de webpagina moet verschijnen, en verzamelen alle gegevens die nodig zijn voor het uitbrengen van de HTTP-reactie.
- [Tags](https://docs.joomla.org/Tags_API_Guide "Tags API Guide").
- Uri / JUri zie [URLs in Joomla](https://docs.joomla.org/URLs_in_Joomla "URLs in Joomla")
- [Gebruiker / JUser](https://docs.joomla.org/Accessing_the_current_user_object "Accessing the current user object") API gerelateerd aan de Joomla Gebruiker.

*Vertaald door openai.com*

