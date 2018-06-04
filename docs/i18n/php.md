# Internationalization with PHP

This section explains how to implement translations within PHP files, and assumes you have read the *[Introduction](../i18n/introduction)* from an earlier section.

We use <a href="https://www.codeigniter.com/userguide3/libraries/language.html?highlight=language" target="_blank">Codeigniter's Language Class</a> to handle all backend translations. Please refer to their documentation for implementation examples.

## [1. Adding translations](#adding-translations)

Adding internationalization support for PHP files relatively straightforward. First you must add the translated strings to the `messages.po` file. Currently, the two repositories that contain a `messages.po` file are the Homepage repo and the Infograph repo. Within both of these repos the `.po` files can be found in a subdirectory of `locale/` that matches the language you are adding support for:
> i.e. Spanish translations are added to the `messages.po` within `/locale/es/LC_MESSAGES/`

## [2. PO File Structure](#po-structure)

Below is an example of basic PO file structure. The `msgid` is the id used to represent the text being translated and should always follow this format rather than using the literal English string you are translating. The `msgstr` line where you would insert your translated strings. Both the `msgid` and `msgstr` should be wrapped in double quotations.

```po
# top_menu
msgid "top_menu.home"
msgstr "Inicio"

msgid "top_menu.features"
msgstr "Características"

msgid "top_menu.pricing"
msgstr "Precios"

msgid "top_menu.about_us"
msgstr "Sobre Nosotros"

msgid "top_menu.templates"
msgstr "Plantillas"

msgid "top_menu.my_designs"
msgstr "Mis Diseños"
```

Once you have implemented all your translations and assigned a meaningful `ID` to each, the next step is to recompile the `.mo` files and restart the `php-fpm` service within your docker. To do this we must run the `compile.sh` file found in the `locale/` directory.

```sh
docker exec docker_workspace_1 bash /deploy/Venngage/locale/compile.sh && \
docker exec docker_php-fpm_1 bash service php5.6-fpm restart
```

## [3. Adding Locale Support](#locale-support)

If the translation changes you made aren't showing up on your local, first check that your browser is set to the language you are adding support for. If you have followed all of the preceding steps and your changes are still not rendering, check to make sure you have the locale support installed for the specific locale you're working with.

You can check which locales you have installed by going into the `php-fpm` container

```sh
cd docker && \
docker-compose exec php-fpm bash
```

Once inside the docker container, run `locale -a` to get a list of the locales installed on your local container.

If the locale you are working with is missing, run the following command to install the necessary locale support. The example below is for installing locale support for Spanish locales.

```sh
apt-get update && \
apt-get install locales && \
sed -i 's/# es_ES.UTF-8 UTF-8/es_ES.UTF-8 UTF-8/' /etc/locale.gen && \
locale-gen && \
locale -a
```

Once you've ensured you have the correct locales installed, exit the container and recompile & restart `php-fpm` to see your changes.

```sh
docker exec docker_workspace_1 bash /deploy/Venngage/locale/compile.sh && \
docker exec docker_php-fpm_1 bash service php5.6-fpm restart
```

## [4. Adding Project Specific Locale Support](#locale-support)

### Infograph:
**Filename:** ci/system/core/CodeIgniter.php

```php
<!-- Add locale to locale lookup and $lang_map -->

$matching_lang = locale_lookup(['es', 'fr', 'it'], $header_lang, false, 'en');

$lang_map = array(
  'es' => 'es_ES.UTF-8',
  'fr' => 'fr_FR.UTF-8',
  'en' => 'en_US.UTF-8',
  'it' => 'it_IT.UTF-8'
);
```

**Filename:** docker/php-fpm/5.6/Dockerfile

```sh
# Add locale to list of installed locales in Dockerfile
RUN apt-get -y update && \
    apt-get install -y locales && \
    sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    sed -i -e 's/# fr_FR.UTF-8 UTF-8/fr_FR.UTF-8 UTF-8/' /etc/locale.gen && \
    sed -i -e 's/# es_ES.UTF-8 UTF-8/es_ES.UTF-8 UTF-8/' /etc/locale.gen && \
    sed -i -e 's/# it_IT.UTF-8 UTF-8/it_IT.UTF-8 UTF-8/' /etc/locale.gen && \
```