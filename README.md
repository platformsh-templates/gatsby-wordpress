> [!WARNING]
> **This repository is no longer maintained by our internal teams.**  
> The template is provided *as is* and will not receive updates, bug fixes, or new features.  
> You are welcome to contribute on it or fork the repository and modify it for your own use.
> To deploy this template on [Upsun](https://www.upsun.com), you can use the command [upsun project:convert](https://docs.upsun.com/administration/cli/reference.html#projectconvert)
> on this codebase to convert the existing `.platform.app.yaml` configuration file to the [Upsun Flex format](https://docs.upsun.com/create-apps/app-reference/single-runtime-image.html).

# Gatsby Wordpress multi-app for Platform.sh

This template builds a two application project to deploy the Headless CMS pattern using Gatsby as its frontend and Wordpress for its backend. The `gatsby-source-wordpress` source plugin is used to pull data from Wordpress during the `post_deploy` hook into the Gatsby Data Layer and build the frontend site. Gatsby utilizes the Platform.sh Configuration Reader library for Node.js to define the backend data source in its configuration. It is intended for you to use as a starting point and modify for your own needs.

Note that after you have completed the Wordpress installation, the project will require a redeploy to build and deploy Gatsby for the first time. See the included README's post-install section for details.

Gatsby is a free and open source framework based on React that helps developers build statically-generated websites and apps, and WordPress is a blogging and lightweight CMS written in PHP.

## Features

* Node.js 14
* PHP 7.4
* MariaDB 10.4
* Automatic TLS certificates
* npm-based build for Gatsby
* Composer-based build for Wordpress
* Multi-app configuration
* Delayed SSG build (post deploy hook)

## Post-install

1. When you initially deploy the template, you will receive a `403` error on the base route. There is not yet any content to build the Gatsby site, because Wordpress has not yet been fully installed. Visit the `backend.<generated url>` subdomain, and run through the Wordpress installer as normal. You will not be asked for database credentials as those are already provided.
2. Once you have completed the Wordpress install, redeploy the environment with `platform redeploy -p <PROJECT ID> -e master`.

## Customizations

The following files and additions make the framework work.  If using this project as a reference for your own existing project, replicate the changes below to your project.

* The `.platform.app.yaml`, `.platform/services.yaml`, and `.platform/routes.yaml` files have been added.  These provide Platform.sh-specific configuration and are present in all projects on Platform.sh.  You may customize them as you see fit.
* Additional Platform.sh configuration reader modules for both [PHP](https://github.com/platformsh/config-reader-php) and [Node.js](https://github.com/platformsh/config-reader-nodejs) have been added. They provide convenience wrappers for accessing the Platform.sh environment variables.
* `gatsby-config.js` has been modified to read the Wordpress backend url and assign it to the `baseUrl` attribute for the `gatsby-source-wordpress` plugin. Since routes are not available during the build hook, and since we want this value to be generated and unique on each environment, `gatsby build` runs and pulls in content from the Wordpress app during the `post_deploy` hook on the mounted `public` directory. `gatbsby-source-wordpress` can have additional parameters set to modify your configuration, so consult the [documentation](https://www.gatsbyjs.org/packages/gatsby-source-wordpress/#how-to-use).

## References

* [Gatsby](https://www.gatsbyjs.org/)
* [gatsby-source-wordpress](https://www.gatsbyjs.org/packages/gatsby-source-wordpress/)
* [WordPress Source Plugin Tutorial](https://www.gatsbyjs.org/tutorial/wordpress-source-plugin-tutorial/)
* [Node.js http](https://nodejs.org/api/http.html#http_http)
* [Node.js on Platform.sh](https://docs.platform.sh/languages/nodejs.html)
* [WordPress](https://wordpress.org/)
* [WordPress on Platform.sh](https://docs.platform.sh/frameworks/wordpress.html)
* [PHP on Platform.sh](https://docs.platform.sh/languages/php.html)
