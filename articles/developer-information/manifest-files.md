<!-- Filename: Manifest_files / Display title: Manifestbestanden -->

## Inleiding

Elke Joomla-extensie vereist een manifestbestand. Het wordt door Joomla gebruikt om bestanden op de juiste locaties te installeren en andere taken uit te voeren bij installatie of verwijdering.

Raadpleeg de sectie [Manifest Files](jdocmanual?article=docus/install-update/installation-manifest) van de Joomla! Programmersdocumentatie voor meer informatie.

De volgende code toont het manifestbestand voor de **Contacts**-component (com_contact) als een voorbeeld:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<extension type="component" method="upgrade">
    <name>com_contact</name>
    <author>Joomla! Project</author>
    <creationDate>2006-04</creationDate>
    <copyright>(C) 2006 Open Source Matters, Inc.</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <authorEmail>admin@joomla.org</authorEmail>
    <authorUrl>www.joomla.org</authorUrl>
    <version>4.0.0</version>
    <description>COM_CONTACT_XML_DESCRIPTION</description>
    <namespace path="src">Joomla\Component\Contact</namespace>
    <install> <!-- Runs on install -->
        <sql>
            <file driver="mysql" charset="utf8">sql/install.mysql.utf8.sql</file>
        </sql>
    </install>
    <uninstall> <!-- Runs on uninstall -->
        <sql>
            <file driver="mysql" charset="utf8">sql/uninstall.mysql.utf8.sql</file>
        </sql>
    </uninstall>
    <files folder="site">
        <folder>forms</folder>
        <folder>helpers</folder>
        <folder>layouts</folder>
        <folder>src</folder>
        <folder>tmpl</folder>
    </files>
    <languages folder="site">
        <language tag="en-GB">language/en-GB/com_contact.ini</language>
    </languages>
    <media destination="com_contact" folder="media">
        <folder>js</folder>
    </media>
    <administration>
        <menu img="class:address-book">COM_CONTACT</menu>
        <submenu>
            <!--
                Note that all & must be escaped to &amp; for the file to be valid
                XML and be parsed by the installer
            -->
            <menu link="option=com_contact" img="class:contact"
                alt="Contact/Contacts">COM_CONTACT_CONTACTS</menu>
            <menu link="option=com_categories&amp;extension=com_contact"
                view="categories" img="class:contact-cat" alt="Contacts/Categories">COM_CONTACT_CATEGORIES</menu>
        </submenu>
        <files folder="admin">
            <filename>access.xml</filename>
            <filename>config.xml</filename>
            <filename>contact.xml</filename>
            <folder>forms</folder>
            <folder>helpers</folder>
            <folder>layouts</folder>
            <folder>services</folder>
            <folder>sql</folder>
            <folder>src</folder>
            <folder>tmpl</folder>
        </files>
        <languages folder="admin">
            <language tag="en-GB">language/en-GB/com_contact.ini</language>
            <language tag="en-GB">language/en-GB/com_contact.sys.ini</language>
        </languages>
    </administration>
</extension>
```

*Vertaald door openai.com*

