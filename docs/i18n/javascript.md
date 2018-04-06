# Internationalization with Javascript

As mentioned in the *[Introduction](../i18n/introduction)* this part of the guide assumes you have already translated strings from English into the language you are adding support for, and that the destination files for those translations are written in Javascript.

We use <a href="http://airbnb.io/polyglot.js/#interpolation" target="_blank">Airbnb's Polyglot library</a> to handle all frontend translations. Please refer to their documentation for implementation examples.

## [Adding New Locales](#adding-locales)

All translation files for this type of internationalization can be found in one of two locations:

1. `html/app/src/translation/`

2. `html/editor/src/translation/`

Within those folders, you will find `main.js` which contains polyglot config the `locales/` directory which contains all the translations for specific locales. It is in this folder that you will create a new file for the language you are adding support for.

!!! example
    If you are adding language support for Hungarian, create a file called `hu.js` where you will add your translations in the next section.

Once the new language file is created, it needs to be imported in `main.js`

```js
import Polyglot from "node-polyglot";
import { en, es, fr, hu } from "./locales";  // import new locale here

const language = { en, es, fr, hu };  // add new locale to language const
```

## [Adding Translations](#adding-translations)

Below you can see a sample of the file structure from `es.js`. Here we have nested objects of key/value pairs that are labeled according to the parts of the app they relate to. The example below is from the onboarding survey.

```sh
survey: {
    welcome: "¡Bienvenidos a Venngage %{name}!",
    recommend: "Nos gustaría recomendarte algunos diseños, solo para tí",
    personalize: "Vamos a personalizar tu página, %{name}!",
    q1: "¿En qué tipo de organización trabajas?",
    q1_opt: {
        self_employed: "Trabaja por su cuenta<br><br>",
        sm_business: "Negocio Pequeño<br>(<50)",
        md_business: "Negocio Mediano<br>(51-500)",
        enterprise: "Empresa<br>(>500)",
    },
},
```

## [Using Polyglot](#using-polyglot)

Once all of your translations are implemented in the correct language file, you now need to find the `.js` files containing the strings you wish to translate.

Before we begin substituting the translation strings, we first need to import the polyglot library which we've given the class name `VGTranslator`

```js
import React from 'react';
import VGTranslator from 'translation/main';  // our polyglot wrapper
```

Then substitue the translation text with our `VGTranslator` function

```js
<h3>
    { VGTranslator.t('survey.personalize', {name: "Scooby"}) }
</h3>
```

Which outputs to:

```html
=> "Vamos a personalizar tu página, Scooby!"
```

!!! seealso "Using Variables With Translation Strings"
    The example above includes a translation string that specifies a `%{name}` variable. Refer to the Interpolation section of the <a href="http://airbnb.io/polyglot.js/#interpolation" target="_blank">Polyglot Documentation</a> for more information on variables and pluralization.
