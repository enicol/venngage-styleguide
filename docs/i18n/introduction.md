# Internationalization

## Introduction

---
This walkthrough is is intended for anyone taking part in the process of adding support for a new language across Venngage's Web App. The subsequent documentation is meant to provide you with an outline for the general process of adding a new language, the steps needed to release it to our production site, and clarification on the roles and responsibilities for those involved in the process.


## [Outline](#outline)
1. [Key Terms](#key-terms)
1. [Release Schedule](#release-schedule)
1. [Roles and Responsibilities](#roles-and-responsibilities)
1. [Critical Pages](#critical-pages)
1. [Release Checklist](#release-checklist)


## Key Terms
Before outlining the release process for new languages, it's important to read up on the *Master Docs* as they are central to the translation process and the term is used frequently in this guide. 

### Master Docs: 
This is the term used to refer to the group of files that contain all of the translations for the venngage. These files come in 2 different types, PO files that end with `.po` and Javascript files, which end in `.js`. Below is a sample of the general structure for each file type.
```po
# messages.po 

msgid "top_menu.email_us"
msgstr "Email Us"

msgid "top_menu.my_account"
msgstr "My Account"

msgid "top_menu.sign_out"
msgstr "Sign Out"
```

```js
// en.js

login: {
    name: "First name",
    surname: "Last name",
    password: "Password",
}
```

As you can see in both examples, the master docs are organized as a list of key value pairs. Where the `msgid` is the *key* in `.po` files, and `msgstr` is the *value* to be translated by the *Translator*. In the `.js` files the word to the left of the colon is the *key*, and everything to the right of the colon in quotes `""` is the *value* 


### locale name:
This term is used in some parts of the documentation and refers to the language or locale you are adding support for. Specifically it is a shorthand reference for that language. If we were to add support for Italian, the locale name for it would be `it`, and for Hungarian is would be `hu`.

## Release Schedule

> ![zoomify](images/i18n-release-schedule.png){.center .small}

## Stage 1: Initial File Translation

**Translator, Developer** ~ 2 weeks

- Developer
    - Before beginning any other steps in this process, the Developer should ensure that the most recent version of master docs (with all the latest translations) is uploaded to Trello card
- Translator
    - Translator downloads the up-to-date version of master docs from Trello card and begins translating them
    - Once the translator is done translating the files, they should ensure that 
        - All translations are consistent across files ([see example here](#what-not-to-do))
        - All files are UTF-8 encoded 
- Translator
    - Once both of these checks are done, **translators** should re-upload them to Trello card [INSERT TRELLO CARD NAME] and notify the members of the card that the files have been updated
- Developer
    - The developer should then download and begin implementing and testing the the new language files. Please refer to the Developer documentation for [i18n with JS](https://enicol.github.io/venngage-styleguide/i18n/javascript/) and [i18n with PHP](https://enicol.github.io/venngage-styleguide/i18n/php/) for further implementation instructions. 
    - Once the new language files have been implemented and tested to make sure there are no issues or syntax errors, the developer should create a pull request to merge the new language files in time for Monday's internal release



## Stage 2: Internal Release

**QA Translator, Developer** ~ 2 weeks

- Developer
    - If new translation keys were added to the translation files by other developers during the time between receiving and uploading the files to staging, the developer **MUST** update the Trello Card containing the Master Docs so that the **QA Translator** has the most up-to-date version of the files in time for them to begin QA'ing the new translations
- QA Translator
    - Once the new language is live on staging,  the QA Translator can begin auditing the site and updating the translation files with their changes
	- Ensure any changes to specific translations are consistent and updated across all other files containing the same translation
- Developer
    - Make sure that the sitemap has been added to the subdomain for SEO
    - You can test this by going to  https://pt.venngage.com/sitemap.xml
- QA Translator
    - After the 2 weeks QA translator uploads their edited translation files to the Trello card and notifies the card that the new files are ready to be updated on staging

### Stage 3: Product Sprint Part 1

#### Infograph ~ 1 week

**QA Translator, Developer 1, Engineering QA**

- **NOTE:** Stage 3 is often split into 2 separate tasks within the Product Sprint, one for the Homepage translations and one for Infograph translations. We do this because it can be difficult to anticipate how many issues and bug fixes will be required for each new language. with one developer focusing on the fixes for Infograph and the other focusing on the fixes for Homepage and SEO
- Developer
    - Once the QA has uploaded the new files to Trello, the developer should upload the new files to staging as soon as possible so that the Engineering QA’s can have ample time to carry out testing
- Developer
    - **Pt 1:**
        - The first part of this stage focuses on fixes to these critical pages within to tool:
            - My Designs
            - Templates page
            - Brand
    - **Pt 2:**
        - After fixing those pages, move on to the secondary areas of the app:
            - Editor
            - Account pages


## Stage 3: Product Sprint Part 2

#### Homepage & SEO ~ 1 week

**QA Translator, Developer 2, Engineering QA**

- Developer
    - Once the QA has uploaded the new files to Trello, another developer should upload the new files to staging as soon as possible so that the Engineering QA’s can have ample time to carry out testing
- A second developer is needed to complete this stage of translations. Their primary focus will be on fixes for the Homepage translations and then eventually on SEO requirements and wordpress
- Developer
    - **Pt 1:**
        - Landing Page
        - Top navigation
        - Pricing page
        - Templates Page
    - **Pt 2:**
        - SEO Requirements
        - Landing Page Admin
        - Wordpress


## Roles and Responsibilities

Below is a list of roles and responsibilities for the different people involved in the translations process

**Product/Card Owner:**

- Establish contact with the Translator and ensure you have some way of contacting them throughout the release process
- Ensure Master Docs files are updated prior to translation handoff to Translator
- *need marketer input @John*

**Translator**

- **IMPORTANT** all new translations must:
    - Be equal length or **NO MORE** than 20% longer than original english translation
    - Consistent across all files
    - Make sure the files are saved with `utf-8` encoding
- Downloads the master docs from Trello card
- Complete translations within 2 week period & re-upload to Trello card


**QA Translator**

- Must make a list of all translation issues & inconsistencies throughout site
- Update the Master Docs files with changes from their list

**Product**

- New translations should not drastically affect the appearance elements within the app
    - i.e Causing a button's text to wrap to a new line or go beyond its container
- It's important to note that when planning project roadmaps, **careful consideration** should be taken when features include the ability to search
    - New features involving search **MUST** support the ability to search in different languages
- Prior to release, ensure the following critical areas function properly with the new translations
- This sheet will help determine which are the critical pages to focus on first if there are any issues with translations. Fix top priority first and if there is more time focus on the remaining priorities. If not, move to a new card.  [Critical Page Doc](https://docs.google.com/spreadsheets/d/1x1SmQ9ZA4nHUsPoNV0gUdypKs6qTD9oKoxjy4Ozt6zM/edit?usp=sharing)
    - upgrade paths
    - export
    - business features


**Developer**

- Upload the most recent version of the translation files to Master Docs Trello card
    - **IMPORTANT:** It is the developer's responsibility to ensure that the translators receive the most up-to-date files for translation
        - Failing to do this will not be fun for anyone and make everyone's life harder :(
            - **ESPECIALLY** your life developer
- Any new features need to be internationalization friendly (no inline HTML/CSS etc)
- It's also important to note that special character searching is enabled in all search boxes across the site
- Moving and refactoring code needs to also make sure i18n is considered
    - When translations are migrated over to a new repo, the translation entries should be removed from the old location's files
- All new translation keys should follow the same conventions as
```js
    // en.js
    infographic_templates: "Infographic Templates",
    presentations: "Presentation",
    posters: "Poster",
    reports: "Report",
    resumes: "Resume",
    "social media": "Social", // Bad - Inconsistent key syntax
    flyers: "Flyer",
```
- **Blog**: See documentation for [Creating Wordpress for other languages](https://github.com/Venngage/wordpress/blob/master/db_migration/Create%20the%20new%20wordpress%20for%20other%20language.md)
- 3rd party APIs must be updated to support to locale
	- VIP ~ Kendrick
    - route53 ~ Kendrick
	- Algolia ~ any Dev
	- google console ~ Kendrick or Kyu



## Critical Pages

Below is a table that lists the app's critical pages and the location + file type for translations on that page. This is helpful for helping you to locate where certain translations are stored, in order for translations to be updated or fixed.


| Page             | Repo            | Filename                            | File path                                                                                      |
|------------------|-----------------|-------------------------------------|------------------------------------------------------------------------------------------------|
| Accounts         | assets          | assets-en.js                        | js/src/translation/locales/{{locale name}}.js                                                  |
| Brand            | assets          | assets-en.js                        | js/src/translation/locales/{{locale name}}.js                                                  |
| Editor           | infograph       | infograph-messages.po               | locale/{{locale_name}}/LC_MESSAGES/messages.po                                                 |
| Infographics     | assets          | infograph-en.js                     | html/app/src/translation/locales/{{locale name}}.js                                            |
| Landing Page     | homepage        | homepage-messages.po                | locale/{{locale_name}}/LC_MESSAGES/messages.po                                               |
| Onboarding       | assets          | assets-en.js                        | js/src/translation/locales/{{locale name}}.js                                                  |
| Pricing          | homepage        | homepage-messages.po                | locale/{{locale_name}}/LC_MESSAGES/messages.po                                                 |
| Static Templates | assets & homepage | assets-en.js & homepage-messages.po | js/src/translation/locales/{{locale name}}.js, locale/{{locale_name}}/LC_MESSAGES/messages.po |
| Templates        | infograph       | infograph-en.js                     | html/app/src/translation/locales/{{locale_name}}.js                                           |


**IMPORTANT** make sure to note that testing must be done on both the mobile and web versions for these critical pages



## Release Checklist

**Marketer**

- [x] Thoroughly test new locale across all critical pages
- [x] Test for missing translations throughout content, 
- [x] Test for weird UI and irregular characters
- [ ] *need marketing input @John*

**SEO**

- [x] Test to make sure SEO is implemented and functions properly
- [x] Check that sitemaps have been implemented correctly
    - [ ] sitemap-static.xml for [venngage.com/stiemap-static](https://venngage.com/sitemap-static.xml)
    - [ ] sitemap-categories.xml [venngage.com/sitemap-categories](https://venngage.com/sitemap-categories.xml)
    - [ ] sitemap.xml (main sitemap) [venngage.com/sitemap](https://venngage.com/sitemap.xml)
- [x] Complete SEO checklist can be found here [SEO docs](https://docs.google.com/document/d/1vlTrZ-z37wJL9bHavslqRFzuci2P65Vk3OXctZ701tQ/edit?pli=1)

**Product**

- Test critical pages and upgrade paths
    - [x] [Critical pages](#critical-pages)
    - [x] Upgrade paths
    - [x] Export
    - [x] Business Features


**Developer**

- [x] Check browser console and make sure there are no console.log errors
- [x] Blog checklist
    - *need input @Ken*
- [x] Ensure all 3rd party APIs have been updated to support new language
    - VIP ~ Kendrick
    - route53 ~ Kendrick
	- Algolia ~ any Dev
	- google console ~ Kendrick or Kyu



## What **NOT** To Do

Below are a few examples of what *not* to do when 




*Still TODO*
- Blog release process/checklist for new languages
- Find a more efficient way to manage master docs