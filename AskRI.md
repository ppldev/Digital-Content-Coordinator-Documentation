# AskRI
---

**Table of contents**

* [Site Theme](#site-theme)
* [User Accounts](#user-accounts)

## Site Theme
:link: [Live Site](https://askri.org)
:open_file_folder: **Local Development Site** - /Users/dcc/Sites/askri-v2/

The AskRI theme is based off of the [_underscores starter theme](https://underscores.me/). Most of the contents of the theme are custom code built on top of the minimal framework _underscores provides.

**Directory Structure**

* :open_file_folder: - **askri**
  - :open_file_folder: - **dist** - contains the minified css, font files, images and minified js files being called when the site loads. --
  - :open_file_folder: - **inc** - theme funtions called in the functions.php file. These files include the code for User Account registration, navigation modifications, and custom theme settings.
  - :open_file_folder: - **js** - javascript files for ajax functions, login functions, main js and navigation
  - :open_file_folder: - **languages** - folder that comes with the _underscores theme. Not currently being used.
  - :open_file_folder: - **layouts** - css for the sidebar areas of the theme. Not currently being used.
  - :open_file_folder: - **sass** - SCSS files that are compiled and written to the style.css file in the `dist/css/` folder.
  - :open_file_folder: - **template-parts** - template files for most of the sections of the site. These files contain most of the post loops, calls to retrieve Advanced Custom Fields and structure of most pages.

I won't break down all of the files in the root directory. Here are files that are important to the site theme that you may need to work with in the future.

* - **archive-resources.php** - Call the resources template `template-parts/content-resources.php` which displays the AskRI databases on the [homepage](https://askri.org) and on [this page](https://www.askri.org/resources/)
* - **footer.php** - site footer used on all pages of the site
* - **header.php** - site header, contains google analytics tracking code and the call for the search bar plus UI for login, menu, search.
* - **page-additional-resources.php** - calls the templates that display the content on the [additional resource pages archive page](https://www.askri.org/additional-askri-resources/)
* - **page-my-account.php** - The page that displays for users who are logged in with [AskRI.org accounts](https://www.askri.org/my-account/).
* - **page-resource-finder.php** - The template for the [resource-finder](https://www.askri.org/resource-finder/). Not currently in use...
* - **page-resource-list.php** - The template for [individual additional resource pages](https://www.askri.org/consumer-protection/)
* **single-resources.php** - page for the display of each [single database resource](https://www.askri.org/resources/world-book/). Includes calls to templates for video-tutorials, widgets, and promo material if that resource has data for those templates.

## User Accounts

The functions that handle the login and admin of user accounts can be found in `inc/askri-accounts.php`. This file includes registration functions and functions to allow users to edit their account data.

The functions for saving or deleting resources, video tutorials, widgets and promo items from user accounts can be found in `inc/askri-ajax.php`. All of these functions use ajax calls to handle calling the save/delete functions and then displaying relevant messages to the user. You can find the ajax js functions in `js/ajax.js`.

Currently the site has 1,000 + users. Many of them, based on email address, appear legitimate. I would go through on a regular basis and prune ones with domains from countries that can't really use AskRI (ex: Russia). Those are most likely spammers.
 
