# tribute-wp-deploy

[![CircleCI](https://circleci.com/gh/Masterworks/tribute-wp-deploy.svg?style=shield)](https://circleci.com/gh/Masterworks/tribute-wp-deploy)
[![Dashboard tribute-wp-deploy](https://img.shields.io/badge/dashboard-tribute_wp_deploy-yellow.svg)](https://dashboard.pantheon.io/sites/024558ce-6d0b-436a-9b7d-eb0a8b4833ac#dev/code)
[![Dev Site tribute-wp-deploy](https://img.shields.io/badge/site-tribute_wp_deploy-blue.svg)](http://dev-tribute-dp-deploy.pantheonsite.io/)

**Architecture**

**5 moving pieces**

***1) Tribute WP Plugin***

Communicates with our *Tribute Service* to handle:
- User Authentication
- Retrieval of Gifts/Responses
- Interplay with theme (shortcodes, etc.)

***2) WP Theme (Parent)***

Provides general layout and theme specific functionality that is shared across clients
Ideally - as an enhancement or offering we could release a new Parent Theme each year 

***3) WP Theme (Child)***

Client-specific [https://codex.wordpress.org/Child_Themes](child theme). Overrides styles in the parent theme that are client-specific. Primarily styles and assets, but in practice could also override individual page templates.

Examples:
- .logo class
- branding

***4) Tribute REST Service(s)***

- Authentication and user validation
    - Sending SMS
    - Sending emails
    - Logging activities
- Returning Constituent Response history
- Printing a PDF (via Gcloud function) and returning that document to WP

***5) Print PDF Google Cloud function***

- Call out to client WP Site
- Save Constituent history to PDF

___

**Github Repos**

***Possible Solutions**

Scenario 1 - Branch per client - would require a lot of merging from master into client branches

Scenario 2 - Fork per client - would require a lot of pulling/merging from forked repo

Scenario 3 - Client specific composer.json - "one" shared repo, customizations are handled in the client-specific composer.json and the build process

Note: Cannot use tags, they are simply references to a specific point in time

***Proposed Solution***

****1) tribute-wp-plugin****

****2) tribute-wp-themes****

Contains both the Parent themes and Child Themes

****3) tribute-service****

****4) tribute-wp-deploy****

Repo containing all of the code necessary to deploy to Pantheon with Composer/Circle. Contains client-specific composer files.

When we want to "add" a client, we follow the same process except for providing the (https://getcomposer.org/doc/03-cli.md#composer)[COMPOSER env variable] 

```COMPOSER=composer-{client_code}.json```

Composer will look for these files:
- composer-{client_code}.json
- composer-{client_code}.lock