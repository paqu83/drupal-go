# Repozytorium zawiera przykładową aplikację startową dla Drupal

## Wymagania - podstawowe narzędzia
- system operacyjny Linux lub MacOs
- zainstalowany Docker Desktop https://www.docker.com/products/docker-desktop/ lub Colima https://github.com/abiosoft/colima
- zainstalowany composer https://getcomposer.org/download/
- zainstalowane dockerowe środowisko developerskie https://ddev.com/

## Uruchomienie projektu lokalnie
Dzięki odpowiedniej konfiguracji tego repozytorium, uruchomienie projektu to kilka komend
```
git clone git@github.com:paqu83/drupal-go.git
cd drupal-go
composer install
ddev start
ddev import-db --file=drupal-go-db.sql.gz 
```
W tym momencie środowisko jest gotowe pod adresem https://drupal-go.ddev.site/

### Jak wygląda przygotowanie lokalnego środowiska Drupal 10
- Wszystkie komendy wykonujemy z linii poleceń
- `composer create-project drupal/recommended-project drupal-go`
- `cd drupal-go`
- `git init`
- stworzenie pliku `.gitignore` wprowadzenie odpowiednich ścieżek aby nie były wersjonowane
- konfiguracja kontenerów docker `ddev config`
```
 ~/projects/drupal-go │ main +1 !1 ?6 ▓▒░ ddev config                                                                                                                                                                                  ░▒▓ ✔
Creating a new DDEV project config in the current directory (/Users/ppakulski/projects/drupal-go)
Once completed, your configuration will be written to /Users/ppakulski/projects/drupal-go/.ddev/config.yaml

Project name (drupal-go):

The docroot is the directory from which your site is served.
This is a relative path from your project root at /Users/ppakulski/projects/drupal-go
You may leave this value blank if your site files are in the project root
Docroot Location (web):
Found a drupal10 codebase at /Users/ppakulski/projects/drupal-go/web.
Project Type [backdrop, craftcms, django4, drupal10, drupal6, drupal7, drupal8, drupal9, laravel, magento, magento2, php, python, shopware6, silverstripe, typo3, wordpress] (drupal10): drupal10
No settings.php file exists, creating one
Configuration complete. You may now run 'ddev start'.
```
- `ddev start` celem uruchomienia kontenerów
```
Building project images...
Project images built in 1s.
 Network ddev-drupal-go_default  Created
 Container ddev-drupal-go-web  Created
 Container ddev-drupal-go-db  Created
 Container ddev-drupal-go-web  Started
 Container ddev-drupal-go-db  Started
Starting Mutagen sync process...
.....Mutagen sync flush completed in 8s.
For details on sync status 'ddev mutagen st drupal-go -l'
Waiting for web/db containers to become ready: [web db]
Starting ddev-router if necessary...
 Container ddev-router  Created
 Container ddev-router  Started
Waiting for additional project containers to become ready...
All project containers are now ready.
Successfully started drupal-go
Project can be reached at https://drupal-go.ddev.site https://127.0.0.1:32774
```
- na tym etapie możemy uruchomic instalację drupal odwiedzając adres `https://drupal-go.ddev.site/` w przeglądarce

![Screenshot 2023-12-03 at 09 53 48](https://github.com/paqu83/drupal-go/assets/20280759/489ded0b-f893-435b-8f9a-49a7ba8bb1cb)

- podczas instalacji wybieramy opcję `Demo: Umami Food Magazine (Experimental)`

![Screenshot 2023-12-03 at 09 55 19](https://github.com/paqu83/drupal-go/assets/20280759/d5d52245-45d7-424e-898e-8b91fa052e22)


- przechodzimy przez kolejne kroki instalacji
- na tym etapie mamy w pełni działającą stronę na Drupal 10 na naszym lokalnym środowisku `https://drupal-go.ddev.site/`

![Screenshot 2023-12-03 at 10 00 54](https://github.com/paqu83/drupal-go/assets/20280759/56b6a132-e286-469f-b6cd-c89dd5ff53c4)


### Dostosowanie repozytorium do szybkiego uruchamiania
na kolejnych środowiskach developerskich
- instalujemy narzędzie drush `composer require --dev drush/drush`
- sprawdzamy czy drush działa prawidłowo
```
   ~/projects/drupal-go │ main +1 !1 ?7 ▓▒░ ddev drush status                                                                                                                                                                          ░▒▓ 1 х
Drupal version   : 10.1.6
Site URI         : https://drupal-go.ddev.site
DB driver        : mysql
DB hostname      : db
DB port          : 3306
DB username      : db
DB name          : db
Database         : Connected
Drupal bootstrap : Successful
Default theme    : umami
Admin theme      : claro
PHP binary       : /usr/bin/php8.1
PHP config       : /etc/php/8.1/cli/php.ini
PHP OS           : Linux
PHP version      : 8.1.23
Drush script     : /var/www/html/vendor/bin/drush
Drush version    : 12.4.3.0
Drush temp       : /tmp
Drush configs    : /var/www/html/vendor/drush/drush/drush.yml
Install profile  : demo_umami
Drupal root      : /var/www/html/web
Site path        : sites/default
Files, Public    : sites/default/files
Files, Temp      : /tmp
```
- zapisujemy backup bazy danych `ddev drush sql:dump --gzip >> drupal-go-db.sql.gz `
```
 ~/projects/drupal-go │ main +1 !1 ?7 ▓▒░ ddev drush sql:dump --gzip >> drupal-go-db.sql.gz                                                                                                                                        ░▒▓ INT х
 ~/projects/drupal-go │ main +1 !1 ?8 ▓▒░ ls -l | grep drupal-go-db                                                                                                                                                                    ░▒▓ ✔
-rw-r--r--   1 ppakulski  staff  1012053 Dec  3 10:15 drupal-go-db.sql.gz
```
- sprawadzamy czy import bazy danych przchodzi prawidłowo
- `ddev drush sql-drop` usuwamy istniejąca bazę
- importujemy bazę z pliku
```
 ~/projects/drupal-go │ main +1 !1 ?9 ▓▒░ ddev import-db --file=drupal-go-db.sql.gz                                                                                                                                                    ░▒▓ ✔
6.61MiB 0:00:02 [2.40MiB/s] [==============================================================================================================================================================================================>] 100%
Successfully imported database 'db' for drupal-go
```
- ponownie odwiedzmay https://drupal-go.ddev.site/ aby sprawdzić czy strona działa prawidłowo
- na tym etapie jesteśmy gotowi aby zakomitować projekt i wypchnąć go do zdalnego repozytorium
