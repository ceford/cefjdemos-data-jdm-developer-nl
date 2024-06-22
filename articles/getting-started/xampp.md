<!-- Filename: XAMPP / Display title: XAMPP -->

## Introductie

XAMPP is een eenvoudig te installeren pakket dat onder andere een Apache
webserver, PHP, XDBUG en MySQL database bundelt. Zo kunt u makkelijk een
omgeving creëren om Joomla! op uw lokale machine te installeren. De
laatste versie van XAMPP is beschikbaar op
<a href="https://www.apachefriends.org/index.html" class="external text"
target="_blank" rel="nofollow noreferrer noopener">de XAMPP website</a>
De downloads zijn beschikbaar voor Linux, Windows, Mac OS X en Solaris.
Download het bestand die geschikt is voor uw platform.

*Belangrijke opmerking met betrekking tot XAMPP en Skype:* Apache en
Skype gebruiken allebei poort 80 als een alternatief voor inkomende
verbindingen. Als u Skype gebruikt kunt u deze in Extra - Opties -
Geavanceerd - Verbindingspaneel gaan en daar de "Gebruik poort 80 en 443
als alternatief voor ingaande verbindingen" uitvinken. Als Apache als
service start zal deze poort 80 voor Skype gebruiken en zal er geen
probleem zijn. Maar schakel deze optie in Skype uit om problemen te
voorkomen.

### Installatie op Windows

De installatie onder Windows is erg makkelijk. Je kan het gedownloade
XAMPP installatiebestand (bijvoorbeeld
*xampp-windows-x64-7.4.4-0-VC15-installer.exe*) starten. Gedetailleerde
instructies voor Windows zijn hier
<a href="https://www.apachefriends.org/download.html"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">beschikbaar</a>.

If you are on Windows XP or 2003 they are not supported by the main
package but there are compatible versions of XAMPP for these platforms
listed on the download page (but you will only be able to run PHP 5.4 or
lower - and therefore will only be able to test Joomla 3.x and lower).

Bij Windows is het aanbevolen om XAMPP in de hoofdmap te installeren
zoals *c:\xampp* en niet in bijvoorbeeld *c:\program files*. Wanneer dit
gedaan wordt kan Joomla! (en eventueel andere lokale webpagina's) in
*c:\xampp\htdocs* geplaatst worden. (Volgens afspraak worden alle
webcontent in de *htdocs* map geplaatst.)

Als u meerdere http servers heeft (zoals bijvoorbeeld ook IIS) kunt u de
XAMPP poort wijzigen. In \apache\conf\httpd.conf kunt u de regel Listen
80 aanpassen naar Listen \[poortnummer\] (bijvoorbeeld "Listen 8080")

Joomla Community Magazine Tutorial

You can find a detailed tutorial on installing XAMPP on Windows, along
with the Joomla 4 Beta, the Joomla Patch Tester and Git in this <a
href="https://magazine.joomla.org/all-issues/june-2020/github-installing-git"
class="external text" target="_blank" rel="noreferrer noopener">Joomla
Community Magazine article</a>.

### Installatie op Linux

#### Installeer XAMPP

Open een Terminal en voer in:

    sudo tar xvfz xampp-linux-1.7.7.tar.gz -C /opt

(vervang *xampp-linux-1.7.7.tar.gz* door de XAMPP-versie die u heeft
gedownload.)

Dit installeert ... Apache, mysql en php als ook een ftp-server

    sudo /opt/lampp/lampp start

en

    sudo /opt/lampp/lampp stop

start/stopt alle services

#### Test uw XAMPP localhost server

Open uw browser en ga naar

    http://localhost

De index.php zal doorverwezen worden naar

    http://localhost/xampp

Daar zult u instructies vinden over hoe u de standaard
gebruikersnaam/wachtwoord kan aanpassen. Op een PC die deze bestanden op
het internet of een LAN beschikbaar stelt is het aanpassen van deze
standaardwaarde een persoonlijke keuze.

#### Verkrijg Joomla

Download de laatste Joomla installatiebestand
<a href="https://downloads.joomla.org/" class="external autonumber"
target="_blank" rel="noreferrer noopener">[1]</a>

Pak deze uit op uw harde schijf

Verbind met localhost met een standaard FTP-programma

    nobody
    lampp

Creëer een map voor Joomla op de localhost server

Verplaats de uitgepakte Joomla installatiebestanden via FTP naar de
nieuw aangemaakte Joomla-map.

**Belangrijk:**

- De XAMPP installatie stelt de correcte bestandeneigenaar en permissies
  in.
- Using the **CHOWN command** will **cause Ownership problems with
  xampp**.
- **Using nautilus** to manipulate folders/files on localhost will
  **cause Ownership problems with xampp**.

**Database info**

Host

    localhost

Default Database name

    test

Default Database user

    root

There is **no** default Password.

Administrator password is your choice.

Installing Sample Data is recommended for the novice user.

After installation delete the installation directory and point your
Browser to:

    http://localhost/yournewjoomlafolder

or

    http://localhost/yournewjoomlafolder/administrator

#### Creating a link in the Ubuntu menu

**To create a GUI for xammp connected to your Ubuntu menu**

Open up the Terminal and type

    sudo gedit /usr/share/applications/xampp-control-panel.desktop

Then copy the following into the gedit and save.

    [Desktop Entry]
    Encoding=UTF-8
    Name=XAMPP Control Panel
    Comment=Start and Stop XAMPP
    Exec=gksudo "python /opt/lampp/share/xampp-control-panel/xampp-control-panel.py"
    Icon=/usr/share/icons/Tango/scalable/devices/network-wired.svg
    Terminal=false
    Type=Application
    Categories=GNOME;Application;Network;
    StartupNotify=true

If the control panel fails to launch, try running the Exec command
directly in the terminal:

    gksudo "python /opt/lampp/share/xampp-control-panel/xampp-control-panel.py"

If you receive the error:

    Error importing pygtk2 and pygtk2-libglade

Install the missing libraries:

    sudo apt-get install python-glade2

#### XDebug PHP debugger

The XAMPP package for Linux does not includes the XDebug PHP debugger.
To install XDebug on Debian or Ubuntu:

\- Install the *build-essential* package:

    sudo apt-get update
    sudo apt-get install build-essential
    sudo apt-get install autoconf

\- Download the
<a href="http://www.apachefriends.org/en/xampp-linux.html"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">development package</a> for your
version of XAMPP and extract it over your existing installation:

    sudo tar xvfz xampp-linux-devel-1.7.7.tar.gz -C /opt 

\- Build XDebug:

    wget http://xdebug.org/files/xdebug-2.1.3.tgz
    tar xzf xdebug-2.1.3.tgz
    cd xdebug-2.1.3/
    /opt/lampp/bin/phpize

After this you will have following output on your console…

    Configuring for:
    PHP Api Version:         20090626
    Zend Module Api No:      20090626
    Zend Extension Api No:   20090626 

    ./configure --with-php-config=/opt/lampp/bin/php-config
    make
    sudo make install 

Then the output will be this.. please monitor the directory specified.

    Installing shared extensions:     /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/ 

Create a folder in your temp folder that will holds the data file
generated by XDebug:

    sudo mkdir /opt/lampp/tmp/xdebug
    sudo chmod a+rwx -R /opt/lampp/tmp/xdebug 

Alternative installations:

Install using PHP extensions community library (PECL) bundled with
xampp:

    sudo /opt/lampp/bin/pecl install xdebug

On Ubuntu/Debian you can install using:

    apt-get install php5-xdebug 

(warning: this will also install Apache and PHP from apt repositories).

**Warning for 64bit users**

When compiling XDebug or installing via apt-get, you will receive an
error when (re)starting xampp:

    /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/xdebug.so: wrong ELF class: ELFCLASS64

This is because xampp runs 32bit but XDebug is 64bit. To overcome this
problem, either make xdebug.so on a 32bit machine or download it from:

    http://code.activestate.com/komodo/remotedebugging/

Download the file: "PHP Remote Debugging Client" for "Linux (x86)"
Extract the content of the file on your computer, this compressed file
contains several folders with version numbers ex: 4.4, 5.0, 5.1 ... 5.3
and so forth, get in the folder with the higher version number or the
one that works for you, then manually copy the file "xdebug.so" to the
following location, overwrite if needed

    /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/

Remember this location could be different on your computer

### Installation on Mac OS X

Mac OS X actually includes an Apache server out-of-the-box, but most
developers will prefer to use the integrated tools and configurability
provided by XAMPP.

As with most programs on Mac, installation is a breeze. Visit
<a href="https://www.apachefriends.org/en/download.html"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">Apache Friends - Mac OS X</a> for the
universal binary download.

Once the file has finished downloading, just open the disk image, and
drag the XAMPP folder to the "Applications" folder alias.

To start the server, open "XAMPP Control.app" and press the start button
next to Apache.

##### A Little Troubleshooting

Many Mac users have a little difficulty at this stage when trying to set
up another instance of Apache on their machine. If you cannot start
XAMPP's Apache, you have two options:  
**You can change the listening port of XAMPP.** In
\Applications\XAMPP\xamppfiles\etc\httpd.conf, modify the line that
says, "Listen 80" to Listen \[portNumber\]. E.g.:

    Listen 8080

**You can change the listening port of the pre-installed Apache
server.** In finder, go to "/etc" (CMD+SHIFT+G); from here you will be
able to navigate through the normally hidden Apache files. Find the
folder labeled Apache2, and edit the "http.conf" file. Modify the line
that says, "Listen 80" to Listen \[portNumber\]. E.g.:

    Listen 8080

*Note: If you choose to change the port of the pre-installed Apache
server, you may need to restart your computer for changes to take
effect. You will also have to authenticate as an administrator to change
these files.*

### Test XAMPP Installation

Once XAMPP is installed and you have started the Apache service with the
XAMPP Control Panel tool, you can test it by opening your browser and
navigating to
"<a href="http://localhost" class="external free" target="_blank"
rel="nofollow noreferrer noopener">http://localhost</a>". You should see
the XAMPP welcome screen similar to the one below.

<img
src="https://docs.joomla.org/images/thumb/f/fc/Phpinfo_on_xampp.png/800px-Phpinfo_on_xampp.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/f/fc/Phpinfo_on_xampp.png/1200px-Phpinfo_on_xampp.png 1.5x, https://docs.joomla.org/images/f/fc/Phpinfo_on_xampp.png 2x"
data-file-width="1498" data-file-height="883" width="800" height="472"
alt="Phpinfo on xampp.png" />

Select the link called "phpinfo()" in the top menu. This will display a
long screen of information about the PHP configuration, as shown below.

<img
src="https://docs.joomla.org/images/thumb/d/db/Phpinfo.png/800px-Phpinfo.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/d/db/Phpinfo.png/1200px-Phpinfo.png 1.5x, https://docs.joomla.org/images/d/db/Phpinfo.png 2x"
data-file-width="1432" data-file-height="1282" width="800" height="716"
alt="Phpinfo.png" />

At this point, XAMPP is installed successfully. Notice the "Loaded
Configuration File". We will be editing this file in the next section to
configure XDebug.
