<!-- Filename: J4.x:Web_Assets / Display title: Web assets -->

## Begrip

In de frontend wereld zijn een heleboel assets (middelen) verwant. Ons
keepalive script hangt bijvoorbeeld af van het core.js bestand voor het
beheer van de instellingen. In Joomla was nooit een makkelijke manier om
dit te specificeren, je moest gewoon meerdere bestanden opnemen. Joomla
4 verandert dit met het concept van web assets.

## Definitie

Verwante assets worden gedefinieerd in een JSON bestand zoals <a
href="https://github.com/joomla/joomla-cms/blob/7b72c565b610e02c1b01f8958d622879631fa6a2/build/media_source/system/joomla.asset.json#L14-L21"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">system/joomla.asset.json#L14-L21</a>

Dit heeft een structuur met een schema definitie (voor validatie), naam,
versie, licentie en dan één of meer asset definities. Assets bevatten
een lijst met js - en css bestanden die gerelateerd zijn aan de assets
en afhankelijkheden. De afhankelijkheden sectie is alleen maar een lijst
met assetnamen die noodzakelijk zijn om de assets te laten functioneren.
Bijvoorbeeld:

    {
      "$schema": "https://developer.joomla.org/schemas/json-schema/web_assets.json",
      "name": "com_example",
      "version": "4.0.0",
      "description": "Joomla CMS",
      "license": "GPL-2.0+",
      "assets": [
        {
          "name": "bar",
          "type": "style",
          "uri": "com_example/bar.css"
        },
        {
          "name": "bar",
          "type": "script",
          "uri": "com_example/bar.js"
        },
        {
          "name": "beer",
          "type": "style",
          "uri": "com_example/beer.css",
          "dependencies": [
            "bar"
          ],
        },
        {
          "name": "beer",
          "type": "script",
          "dependencies": [
            "core",
            "bar"
          ],
          "uri": "com_example/beer.js",
          "attributes": {
            "defer": true,
            "data-foo": "bar"
          }
        }
      ]
    }

Het `$schema` attribuut is een schema definitie bestand dat je in staat
stelt je bestand te valideren met behulp van JSON schema. Lees op de
<a href="https://json-schema.org/understanding-json-schema/index.html"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">de officiële website</a> voor meer
informatie hoe json schema validatie werkt.

**Let op:** Het hebben van joomla.asset.json voor je extensie of
template wordt aanbevolen, maar is niet verplicht om WebAssset te laten
werken (kijk bij de volgende sectie).

**Let op:** Het wordt niet aangeraden om een inline asset toe te voegen
aan een json bestand, gebruik liever een bestand.

## Uitleg van asset stadia

Elke asset heeft 2 stadia: geregistreerd en gebruikt.

**Registered** is where an asset is loaded into **WebAssetRegistry**.
That means **WebAssetManager** knows about the existence of these
assets, but will not attach them to a document while rendering. All
assets loaded from joomla.asset.json is at **registered** stage.

**Used** is where an asset is enabled via "\$wa-\>useAsset()"
(-\>useScript(), -\>useStyle(), -\>registerAndUseX() etc). That means
**WebAssetManager** will attach these assets and their dependencies to a
document while rendering.

An asset cannot be used if it was not registered before, this will cause
an unknown asset exception.

## Register an asset

All known assets loaded and then stored in **WebAssetRegistry** (to
enable/disable an asset item you have to use **WebAssetManager**, see
next section).

Joomla! will look for next assets definition automatically at runtime
(in following order):

    media/vendor/joomla.asset.json (on first access to WebAssetRegistry)
    media/system/joomla.asset.json
    media/legacy/joomla.asset.json
    media/{com_active_component}/joomla.asset.json (on dispatch the application)
    templates/{active_template}/joomla.asset.json

And load them to registry of known assets.

**Note:** Each following assets definition will override asset items
from previous assets definition, by item name.

  
You can register your own assets definition via **WebAssetRegistry**:

    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();
    $wr = $wa->getRegistry();
    $wr->addRegistryFile('relative/path/to/your/joomla.asset.json');

To add a custom asset item at runtime:

    $wr->add('script', new Joomla\CMS\WebAsset\WebAssetItem('foobar', 'com_foobar/file.js', ['type' => 'script']));

Or more simply, using **WebAssetManager**:

    $wa->registerScript('foobar', 'com_foobar/file.js');

The new asset item **foobar** will be added to the registry of know
assets, but will not be attached to a document until your code (a
layout, template etc) will request it.

To check whether an asset exists:

    if ($wa->assetExists('script', 'foobar'))
    {
        var_dump('Script "foobar" exists!');
    }

## Enabling an asset

All asset management in the current Document handled by
**WebAssetManager**, which is accessible with
**\$doc-\>getWebAssetManager();** By using AssetManager you can enable
or disable needed asset easily in Joomla! through a standard methods.

To enable an asset in the page use the useAsset function, for example:

    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();
    $wa->useScript('keepalive');

    // Or multiple
    $wa->useScript('keepalive')
        ->useScript('fields.validate')
        ->useStyle('foobar')
        ->useScript('foobar');

    // Add new asset item with dependency and use it
    $wa->registerAndUseScript('bar', 'com_foobar/bar.js', [], [], ['core', 'foobar']);

**WebAssetManager** will look to **WebAssetRegistry** whether the
requested asset exists, and will enable it for current Document
instance. Otherwise it will throw an UnknownAssetException.

To disable an asset in the page use the disableAsset function. The
example below will disable the jquery-noconflict asset from being
loaded.

    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();
    $wa->disableScript('jquery-noconflict');

**Note:** If there are any dependencies to the disabled asset, then this
asset will be re-enabled automatically, no matter what.

To check whether asset enabled, and the asset state:

    // Checking whether an asset are active (enabled manually or automatically as dependency)
    if ($wa->isAssetActive('script', 'foobar'))
    {
        var_dump('Script "foobar" is active!');
    }

    // Checking state
    switch($wa->getAssetState('script', 'foobar')){
        case Joomla\CMS\WebAsset\WebAssetManager::ASSET_STATE_ACTIVE:
            var_dump('Active! Was enabled manually');
            break;
        case Joomla\CMS\WebAsset\WebAssetManager::ASSET_STATE_DEPENDENCY:
            var_dump('Active! Was enabled automatically while resolving dependencies');
            break;
        default:
            var_dump('not active!');
    }

## Overriding an asset

Overriding may be useful when you need to redefine the URI of asset item
or its dependencies. As already was noted, each of the following assets
definition from joomla.asset.json will override asset items from
previous assets definitions, by item name. That means if you provide
joomla.asset.json which contain already loaded asset items, they will be
replaced with your items. Another way to override in the code is to
register an item with the same name. Example, we have "foobar" script,
that load com_example/foobar.js library, and we want to use CDN for this
exact library:

How it defined in the system initially:

    ...
    {
      "name": "foobar",
      "type": "script",
      "uri": "com_example/foobar.js",
      "dependencies": ["core"]
    }
    ...

To override the URI we define the asset item with "foobar" name in our
joomla.asset.json:

    ...
    {
      "name": "foobar",
      "type": "script",
      "uri": "http://foobar.cdn.blabla/foobar.js",
      "dependencies": ["core"]
    }
    ...

Or, register new asset item with AssetManager:

    $wa->registerScript('foobar', 'http://fobar.cdn.blabla/foobar.js', [], [], ['core']);

## Working with styles

AssetManager allow to manage Stylesheet files. Stylesheet asset item
have a type "style".

Example json definition of item in joomla.asset.json:

    ...
    {
      "name": "foobar",
      "type": "style",
      "uri": "com_example/foobar.css"
    }
    ...

### Methods to work with styles

AssetManager offers the following methods to work with style files:

    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Attach foobar to the document
    $wa->useStyle('foobar');

    // Disable foobar from being attached
    $wa->disableStyle('foobar');

    // Register custom item without json definition
    $wa->registerStyle('bar', 'com_example/bar.css', [], ['data-foo' => 'some attribute'], ['some.dependency']);
    // And use it later
    $wa->useStyle('bar');

    // Register and attach a custom item in one run
    $wa->registerAndUseStyle('bar', 'com_example/bar.css', [], ['data-foo' => 'some attribute'], ['some.dependency']);

### Add inline style

Additionaly to style files, WebAssetManager allows you to add an inline
style, and maintain their relation to the file asset. Inline styles may
be placed directly before the dependency, after the dependency, or as
usual after all styles.

Inline asset may have a name as well as other assets (but not required),
the name can be used to retrieve the asset item from a registry, or as a
dependency to another inline asset. If the name is not specified then a
generated name based on a content hash will be used.

    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Add an inline content as usual, will be rendered in flow after all assets
    $wa->addInlineStyle('content of inline1');

    // Add an inline content that will be placed after "foobar" asset
    $wa->addInlineStyle('content of inline2', ['position' => 'after'], ['data-foo' => 'bar'], ['foobar']);

    // Add an inline content that will be placed before "foobar" asset
    $wa->addInlineStyle('content of inline3', ['position' => 'before'], [], ['foobar']);

    // Named inline asset
    $wa->addInlineStyle('content of inline4', ['name' => 'my.inline.asset']);

**Note:** "foobar" asset should exist in the asset registry, otherwise
you will get an unsatisfied dependency exception.

Example above will produce:

    ...
    content of inline3

    content of inline2
    ...
    ...
    content of inline1
    content of inline4
    ...

If inline asset has multiple dependencies, then will be used last one
for positioning. Example:

    $wa->addInlineStyle('content of inline1', ['position' => 'before'], [], ['foo', 'bar']);
    $wa->addInlineStyle('content of inline2', ['position' => 'after'], [], ['foo', 'bar']);

Will produce:

    ...

    content of inline1

    content of inline2
    ...

**Note:** Named inline assets may be a dependency to another inline
asset, however it is not recommended to use an inline asset as
dependency to non-inline asset. This will work, but this behavior may
change in the future. Prefer to use "position" instead.

## Working with scripts

AssetManager allow to manage Script files. Script asset item have a type
"script". Example json definition of item in joomla.asset.json:

    ...
    {
      "name": "foobar",
      "type": "script",
      "uri": "com_example/foobar.js",
      "dependencies": ["core"]
    }
    ...

Example json definition of ES6 module script, with fallback to legacy:

    ...
    {
      "name": "foobar-legacy",
      "type": "script",
      "uri": "com_example/foobar-as5.js",
      "attributes": {
        "nomodule": true,
        "defer": true
      },
      "dependencies": ["core"]
    }
    {
      "name": "foobar",
      "type": "script",
      "uri": "com_example/foobar.js",
      "attributes": {
        "type": "module"
      },
      "dependencies": [
        "core", 
        "foobar-legacy"
      ]
    }
    ...

### Methods to work with scripts

AssetManager offer next methods to work with script files:

    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Attach foobar to the document
    $wa->useScript('foobar');

    // Disable foobar from being attached
    $wa->disableScript('foobar');

    // Register custom item without json definition
    $wa->registerScript('bar', 'com_example/bar.js', [], ['defer' => true], ['core']);
    // And use it later
    $wa->useScript('bar');

    // Register and attach a custom item in one run
    $wa->registerAndUseScript('bar','com_example/bar.js', [], ['defer' => true], ['core']);

### Add inline script

Addittionaly to script files WebAssetManager allow to add an inline
script, and maintain their relation to the file asset. Inline script may
be placed directly before the dependency, after the dependency, or as
usual after all scripts.

Inline asset may have a name as well as other assets (but not required),
the name can be used to retrive the asset item form a registry, or as
dependecy to another inline asset. If name not specified then will be
used generated name based on a content hash.

    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Add an inline content as usual, will be rendered in flow after all assets
    $wa->addInlineScript('content of inline1');

    // Add an inline content that will be placed after "foobar" asset
    $wa->addInlineScript('content of inline2', ['position' => 'after'], ['data-foo' => 'bar'], ['foobar']);

    // Add an inline content that will be placed before "foobar" asset
    $wa->addInlineScript('content of inline3', ['position' => 'before'], [], ['foobar']);

    // Named inline asset
    $wa->addInlineScript('content of inline4', ['name' => 'my.inline.asset']);

    // Specify script type
    $wa->addInlineScript('content of inline5', [], ['type' => 'module']);

**Note:** "foobar" asset should exist in the asset registry, otherwise
you will get an unsatisfied dependency exception.

Example above will produce:

    ...
    content of inline3

    content of inline2
    ...
    ...
    content of inline1
    content of inline4
    content of inline5
    ...

If inline asset have a multiple dependencies, then will be used last one
for positioning. Example:

    $wa->addInlineScript('content of inline1', ['position' => 'before'], [], ['foo', 'bar']);
    $wa->addInlineScript('content of inline2', ['position' => 'after'], [], ['foo', 'bar']);

Will produce:

    ...

    content of inline1

    content of inline2
    ...

**Note:** Named inline asset may be as dependency to another inline
asset, hovewer it is not recomended to use an inline asset as dependency
to non-inline asset. This will work, but this behavior may changes in
future. Prefer to use "position" instead.

## Working with a web component

Joomla! allows you to use
<a href="https://developer.mozilla.org/en-US/docs/Web/Web_Components"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">Web Components</a> for your needs. In
Joomla! web components are not loaded as regular script, but loaded via
Web Component loader so that they are loaded asynchronously. Therefore,
a web component asset item must have a flag "webcomponent" set to the
boolean "true". In all other aspects, working with web components in
AssetManager is the same as working with a "script" asset item.

Example json definition of some web components in joomla.asset.json (as
ES6 module):

    ...
    {
      "name": "webcomponent.foobar",
      "type": "style",
      "uri": "com_example/foobar-custom-element.css",
    },
    {
      "name": "webcomponent.foobar",
      "type": "script",
      "uri": "com_example/foobar-custom-element.js",
      "attributes": {
         "type": "module"
      },
    }
    ...

Example with fallback, for browsers that does not support ES6 "module"
feature. Note that the legacy script should have "wcpolyfill"
dependency, and module script should have dependency from legacy script:

    ...
    {
      "name": "webcomponent.foobar",
      "type": "style",
      "uri": "com_example/foobar-custom-element.css",
    },
    {
      "name": "webcomponent.foobar-legacy",
      "type": "script",
      "uri": "com_example/foobar-custom-element-es5.js",
      "attributes": {
        "nomodule": true,
        "defer": true
      },
      "dependencies": [
        "wcpolyfill"
      ]
    },
    {
      "name": "webcomponent.foobar",
      "type": "script",
      "uri": "com_example/foobar-custom-element.js",
      "attributes": {
        "type": "module"
      },
      "dependencies": [
        "webcomponent.foobar-legacy"
      ]
    }
    ...

Alternatively you can register them in PHP (as ES6 module):

    $wa->registerStyle('webcomponent.foobar', 'com_example/foobar-custom-element.css')
        ->registerScript('webcomponent.foobar', 'com_example/foobar-custom-element.js', ['type' => 'module']);

Attach to document:

    $wa->useStyle('webcomponent.foobar')
        ->useScript('webcomponent.foobar');

**Note:** It is preferred to prefix the asset name with "webcomponent."
to make it easily to spot, and distinct it from regular scripts in a
layout.

### Methods to work with web component

All methods to work with a web component are the same as methods to work
with script asset item.

## Working with a presets

"Preset" is a special kind of asset item that hold a list of items that
has to be enabled, in same way as direct call of useAsset() to each of
item in the list. Preset can hold mixed types of assets (script, style,
another preset, etc), the type should be provided after \# symbol and
follows after an asset name, example: foo#style, bar#script.

Example json definition of item in joomla.asset.json:

    ...
    {
      "name": "foobar",
      "type": "preset",
      "uri": "",
      "dependencies": [
        "core#script",
        "foobar#style",
        "foobar#script",
      ]
    }
    ...

### Methods to work with preset

AssetManager offer next methods to work with preset items:

    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Attach all items from foobar preset to the document
    $wa->usePreset('foobar');

    // Disable all items from foobar preset from being attached
    $wa->disablePreset('foobar');

    // Register custom item without json definition
    $wa->registerPreset('bar', '', [], [], ['core#script', 'bar#script']);

    // And use it later
    $wa->usePreset('bar');

    // Register and attach a custom item in one run
    $wa->registerAndUsePreset('bar','', [], [], ['core#script', 'bar#script']);

## Advanced: Custom WebAssetItem class

The default class for all WebAsset items is
**Joomla\CMS\WebAsset\WebAssetItem**.

You are also allowed to use a custom class, which must implement
**Joomla\CMS\WebAsset\WebAssetItemInterface** or extend
**Joomla\CMS\WebAsset\WebAssetItem**.

A custom class can allow you to do advanced actions, for example,
including a script file depending on an active language:

    class MyComExampleAssetItem extends WebAssetItem
    {
        public function getUri($resolvePath = true): string
        {
            $langTag = Factory::getApplication()->getLanguage()->getTag();
            // For script asset use ".js", for style we would use ".css"
            $path    = 'com_example/bar-' . $langTag . '.js';

            if ($resolvePath)
            {
                // For script asset use "script", for style we would use "stylesheet"
                $path = $this->resolvePath($path, 'script');
            }

            return $path;
        }
    }

Additionally, implementing
**Joomla\CMS\WebAsset\WebAssetAttachBehaviorInterface** allows you to
add a script options (which may depend on the environment) when your
asset is enabled and attached to the Document.

    class MyFancyFoobarAssetItem extends WebAssetItem implements WebAssetAttachBehaviorInterface
    {
        public function onAttachCallback(Document $doc): void
        {
            $user = Factory::getApplication()->getIdentity();
            $doc->addScriptOptions('com_example.fancyfoobar', ['userName' => $user->username]);
        }
    }

**Important note:** An asset item that implements
**WebAssetAttachBehaviorInterface** should be enabled before <a
href="https://docs.joomla.org/Plugin/Events/System#onBeforeCompileHead"
class="external text" target="_blank"
rel="noreferrer noopener">onBeforeCompileHead</a> event, otherwise
'onAttachCallback' will be ignored.

### Defining a custom WebAssetItem class in joomla.asset.json

In joomla.asset.json you can define which Class should be used with
specific AssetItem. For this you can use 2 properties **namespace** and
**class**. **namespace** can be defined at Root level (then it will be
used as default namespace for all Asset items in joomla.asset.json) or
in the Item level. For example:

    {
      "$schema": "https://developer.joomla.org/schemas/json-schema/web_assets.json",
      "name": "com_example",
      "version": "4.0.0",
      "namespace": "Joomla\Component\Example\WebAsset",
      "assets": [
        {
          "name": "foo",
          "type": "script",
          "class": "FooAssetItem",
          "uri": "com_example/foo.js"
        },
        {
          "name": "bar",
          "type": "script",
          "namespace": "MyFooBar\Library\Example\WebAsset",
          "class": "BarAssetItem",
          "uri": "com_example/bar.js"
        }
      ]
    }

Here the asset **foo** will be associated with class
**Joomla\Component\Example\WebAsset\FooAssetItem**, and **bar** with
class **MyFooBar\Library\Example\WebAsset\BarAssetItem**.

**Note:** If **namespace** are not defined then by default will be used
**Joomla\CMS\WebAsset**. When **namespace** is defined but empty, then
no namespace will be used, only **class**. Example:

    {
      "$schema": "https://developer.joomla.org/schemas/json-schema/web_assets.json",
      "name": "com_example",
      "assets": [
        {
          "name": "foo",
          "type": "script",
          "class": "FooAssetItem",
          "uri": "com_example/foo.js"
        },
        {
          "name": "bar",
          "type": "script",
          "namespace": "",
          "class": "BarAssetItem",
          "uri": "com_example/bar.js"
        }
      ]
    }

Here the asset **foo** will be associated with class
**Joomla\CMS\WebAsset\FooAssetItem**, and **bar** with class
**BarAssetItem** (without namespace).
