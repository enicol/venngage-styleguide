# Internationalization

## Introduction

---

This walkthrough is intended to go over the steps needed in order to implement support for new languages within Venngage's Web App.

## [Outline](#outline)
1. [Release Schedule](#release-schedule)
1. [Critical Pages](#critical-pages)
1. [Roles and Responsibilities](#roles-and-responsibilities)
1. [Release Checklist](#release-checklist)


## Release Schedule
---

### Stage 1: Initial File Translation

**Translator, Developer** ~ 2 weeks

- Ensure the most recent version of master docs is uploaded to Trello card (this happens first day of new release schedule)
- Translator downloads the up-to-date version of master docs from Trello card and begins translating them
- Once the translator is done with the files, they should ensure that 
	- all translations are consistent across files 
	- that all files are UTF-8 encoded 
- Once both of these checks are done, translators should re-upload them to  Trello card [INSERT TRELLO CARD NAME] and notify the card that the files have been updated
- The developer should then download and begin implementing and testing the the new language files. Once this is done, the developer should create pull requests to start merging the new language files in time for internal release


### Stage 2: Internal Release

**QA Translator, Developer** ~ 2 weeks

- Developer
    - If changes were made to the translation files between receiving and uploading them to staging, the developer must re-upload the modified files to the master docs Trello card so they are ready to hand off to the QA Translator
- QA Translator
    - Once this is done and the new language is live on staging,  the QA Translator can begin auditing the site and updating the translation files with their changes
	- Ensure any changes to specific translations are consistent and updated across all other files containing the same translation
- Developer
    - Make sure that the sitemap has been added to the subdomain for SEO
    - You can test this by going to  https://pt.venngage.com/sitemap.xml
- QA Translator
    - After the 2 weeks QA translator uploads their edited translation files to the Trello card and notifies the card that the new files are ready to be updated on staging

### Stage 3: Product Sprint

#### Infograph Fixes ~ 1 week

**QA Translator, Developer 1, Engineering QA** 

- **NOTE:** Stage 3 should be split into 2 separate tasks within the Product Sprint, with one developer focusing on the fixes for Infograph and the other focusing on the fixes for Homepage and SEO
- Developer
    - Once the QA has uploaded the new files to Trello, the developer should upload the new files to staging as soon as possible so that the Engineering QA’s can have ample time to carry out testing
- **Pt 1:** Developer
    - The first part of this stage focuses on fixes to these critical pages within to tool:
        - My Designs
        - Templates page
        - Brand
- **Pt 2:** Developer
    - Once the fixes have been made for those pages, move on to the secondary areas of the app:
        - Editor
        - Account pages


### Stage 3: Product Sprint continued

#### Homepage & SEO ~ 1 week

**QA Translator, Developer 2, Engineering QA**

- Developer
    - Once the QA has uploaded the new files to Trello, another developer should upload the new files to staging as soon as possible so that the Engineering QA’s can have ample time to carry out testing
- A second developer is needed to complete this stage of translations. Their primary focus will be on fixes for the Homepage translations and then eventually on SEO requirements and wordpress
- **Pt 1:** Developer
    - Landing Page
    - Top navigation
    - Pricing page
    - Templates Page
- **Pt 2:** Developer
    - SEO Requirements
    - Landing Page Admin
    - Wordpress


## Critical Pages
------
Below is a table of the app's critical pages and the file type for translations on that page


| Page             | Repo            | Filename                            | File path                                                                                      |
|------------------|-----------------|-------------------------------------|------------------------------------------------------------------------------------------------|
| Accounts         | assets          | assets-en.js                        | js/src/translation/locales/{{locale name}}.js                                                  |
| Brand            | assets          | assets-en.js                        | js/src/translation/locales/{{locale name}}.js                                                  |
| Editor           | infograph       | infograph-messages.po               | locale/{{locale_name}}/LC_MESSAGES/messages.po                                                  |
| Infographics     | infograph       | infograph-en.js                     | html/app/src/translation/locales/{{locale name}}.js                                            |
| Landing Page     | homepage        | homepage-messages.po                | locale/{{locale_name}}/LC_MESSAGES/messages.po                                               |
| Onboarding       | assets          | assets-en.js                        | js/src/translation/locales/{{locale name}}.js                                                  |
| Pricing          | homepage        | homepage-messages.po                | locale/{{locale_name}}/LC_MESSAGES/messages.po                                                 |
| Static Templates | assets/homepage | assets-en.js & homepage-messages.po | js/src/translation/locales/{{locale name}}.js, locale/{{locale_name}}/LC_MESSAGES/messages.po |
| Templates        | infograph       | infograph-en.js                     | html/app/src/translation/locales/{{locale_name}}.js                                           |

*need to add SEO pages @Cecilien*


## Roles and Responsibilities
----
Below is a list of roles and responsibilities for the different groups involved in the translations process

**Marketer:**

- establish contact with the Translator and 
- ensure Master Docs files are updated prior to translation handoff to Translator
- *need marketer input @John*

**Translator**

- Downloads the master docs from Trello card
- Complete translations within 2 week period & re-upload to Trello card
- Ensures all new translations are:
	- Consistent
	- Similar character count to English translations

**QA Translator**

- Must make an list of all translation issues & inconsistencies throughout site
- Update the Master Docs files with changes from their list

**Product**

- New translations should not drastically affect the appearance elements within the app
    - i.e Causing a button's text to wrap to a new line or go beyond its container
- Prior to release, ensure the following critical areas function properly with the new translations
    - upgrade paths
    - export
    - business features
- *need Product input @Joanna*

**SEO**

- Make sure “add a sitemap” is added to the checklist of the international card
- *need SEO input @Cecilien*

**Developer**

- Upload the most recent version of the translation files to Master Docs Trello card
- Any new features need to be internationalization friendly (no inline HTML/CSS etc) 
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
- 3rd party APIs must be updated to support to locale
	- VIP ~ Kendrick
    - route53 ~ Kendrick
	- Algolia ~ any Dev
	- google console ~ Kendrick or Kyu

## Release Checklist
------

**Marketer**

- Thoroughly test new locale across all critical pages
- test - missing translations, weird ui irregular characters
- *need marketing input @John*

**SEO**

- Test to make sure SEO is implemented and functions properly
- *need SEO input @Cecilien*

**Product**

- Test critical pages and upgrade paths
    - Upgrade paths
    - [Critical pages](#critical-pages)
    - Export
    - Business Features
- *need product input @Joanna*

**Developer**

- Check browser console and make sure there are no console.log errors
- Ensure all 3rd party APIs have been updated to support new language
    - VIP ~ Kendrick
    - route53 ~ Kendrick
	- Algolia ~ any Dev
	- google console ~ Kendrick or Kyu