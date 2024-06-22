<!-- Filename: J4.x:CLI_Database_Exporter_Importer / Display title: CLI Database Exporter Importer -->

Joomla!  4.0

## About

Before updating Joomla! or [installing a third party
extension](https://docs.joomla.org/J4.x:CLI_Update "Special:MyLanguage/J4.x:CLI Update"),
it is strongly recommended that you back up your site.  
The Joomla! 4.x Console provides commands for exporting (backing up) and
importing (restoring) your Joomla! database. Note that it doesn't backup
your filesystem which should be done separately

## Requirements

To use these commands, you need a secured shell access (SSH) to your
host on which the PHP CLI (Command Line Interface) is installed.
Consider to have basic knowledge of using shell commands.

## Instructions

Meld u aan bij uw host en ga naar de rootmap van uw site.  
Ik raad aan de Joomla 'tmp'-map in je te gebruiken om lees- /
schrijfrechten te hebben.

- List all available commands of the Joomla Console:  
  `php cli/joomla.php list`
- Export the database to the folder:  
  `php cli/joomla.php database:export --folder `
- Import the database from folder:  
  `php cli/joomla.php database:import --folder `

You can also:

- Exporteer de database als een .zip file:  
  `php cli/joomla.php database:export --all --zip`
- Exporteer een tabel:  
  `php cli/joomla.php database:export --table `
- Exporteer een tabel als een .zip file:  
  `php cli/joomla.php database:export --table --zip`
- Importeer een tabel:  
  `php cli/joomla.php database:import --table `
- Als je hulp nodig hebt:  
  `php cli/joomla.php database:export --help`
  `php cli/joomla.php database:import --help`

## Back up and restore

To make a full backup (with folders, files and database) of your site,
you can execute these commands:

1.  Archive your Joomla root directory:  
    `tar --exclude='./tmp/joomla_bak.*' -zcvf tmp/joomla_bak.tgz . > tmp/joomla_bak.log`
2.  Export all the Joomla database:  
    `php cli/joomla.php database:export --all --folder tmp/db_bak`

And to restore it, execute these commands:

1.  Import all the Joomla database:  
    `php cli/joomla.php database:import --all --folder tmp/db_bak`
2.  Extract the archive:  
    `tar --recursive-unlink -xvf tmp/joomla_bak.tgz .`
