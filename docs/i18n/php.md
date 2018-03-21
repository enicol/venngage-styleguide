# Internationalization with PHP

This section explains how to implement translations within PHP files, and assumes you have read the *[Introduction](../i18n/introduction)* from an earlier section.

Adding internationalization support for PHP files relatively straightforward. First you must add the translated strings to the `messages.po` found in the same directory that matches the locale you are adding support for:
> i.e. Spanish translations are added to the `messages.po` within `/locale/es/LC_MESSAGES/`

## [1.1 PO File Structure](#po-structure)

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

Once you have implemented all your translations and assigned meaningful IDs to them, the next step is to recompile the `.mo` files and restart the `php-fpm` service within your docker. To do this we must run the `compile.sh` file found in the `locale/` directory.

```sh
docker exec docker_workspace_1 bash /deploy/Venngage/locale/compile.sh && \ 
docker exec docker_php-fpm_1 bash service php5.6-fpm restart
```

Once inside the docker container, run the following command to install the necessary locale support

```sh
apt-get update && \
apt-get install locales && \
sed -i 's/# es_ES.UTF-8 UTF-8/es_ES.UTF-8 UTF-8/' /etc/locale.gen && \
locale-gen && \
locale -a
```