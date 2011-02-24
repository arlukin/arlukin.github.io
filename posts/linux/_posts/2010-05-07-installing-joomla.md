---
layout: post
title: Installing Joomla
---

This article describes very rough how I created Arlukin Articles in
Joomla. The Joomla version is not alive any longer and two other
versions has existed after that. One in [wordpress](http://wordpress.org/)
and currently with [Jekyll](https://github.com/mojombo/jekyll#readme).

I have
done it in a step by step form, so it should be possible to just repeat
all my steps and get a nice working Joomla site. Of course you don’t
even need to go through all these steps to get your Joomla site up and
running. This is more of a guide to the tweaks I have done to this
web-page. After the main installation I have done a few tweaks which
are not commented in the below text. I will most certainly do more tweaks
in the future. But this gives a pretty good understanding of how you install Joomla.

## Joomla extensions used on this site.

### [AdSense Module (ClickSafe – Special Edition)](http://extensions.joomla.org/extensions/ads-&-affiliates/google-ads/243/details)

Simple module to view the Google adsense module that can be seen in the right column on the website.To learn more about Google adsense click [here](http://www.google.com/adsense). I did have some trouble to get my account verified. Actually still after 2 weeks, it’s not verfied. I’m starting to think that the my account has been droped in the trach or something.

### [BIGSHOT Google Analytics](http://extensions.joomla.org/extensions/site-management/site-analytics/6170/details)

If you wold like to get some nice visiting statistics, this is the app for you. Read more on Google analytics by clicking [here](http://www.google.com/analytics).

### [sh404sef](http://extensions.joomla.org/extensions/site-management/sef/2380/details)

Rewrites your URLs, meta tags etc to make your webpage more searchengine friendly.

### [ChronoComments](http://extensions.joomla.org/extensions/contacts-&-feedback/articles-comments/5839/details)

Adds a comment area below every article on the site. So visiters of the site can comment what I have been writing.

### [Motherbot template](http://www.themes-online.com/2009/03/motherbot-joomla/)

The template I have based the webpage on. Slightly modified, I hope that is okay with the guys at http://joomla.server-germany.de/ that did the template.

## Install the Joomla code

Download joomla from [here][http://www.joomla.org/download.html]

    mkdir /var/www/articles.cybercow.se
    cd /var/www/articles.cybercow.se
    wget http://joomlacode.org/gf/download/frsrelease/9910/37908/Joomla_1.5.10-Stable-Full_Package.zip
    unzip Joomla_1.5.10-Stable-Full_Package.zip
    rm Joomla_1.5.10-Stable-Full_Package.zip

* Browse http://localhost
* Click next
* Click next
* Click next
* Choose
mysqli
localhost
username
pass
database name
delete existing tables
* Click Next
* Click Next
* Enter
Site name: Articles
Email: articles at cybercow dot se
* Click “Install sample data”
* Click next


      rm -R installation/
      rm INSTALL.php
      sudo chown -R www-data:www-data .
      mv htaccess.txt .htaccess

## Remove old data

### Choose menu “Content – Article Manager”

* Click “#” to select all check boxes
* Click Trash
* Repeat 3 times

### Choose menu “Content – Trash manager”

* Click “#” to select all check boxes
* Click Delete
* Click Delete
* repeat 3 times


### Choose menu “Content – Category manager”

* Click “#” to select all check boxes
* Click Delete

### Choose menu “Content – Section manager”

* Click “#” to select all check boxes
* Click Delete

### Choose menu “Menus – Menu manager”

* Click “TopMenu”
* Click Delete
* Click Delete
* Repeat for “Resources”, “Example Pages”, “Key Concepts”

### Choose menu “Menus – main menu”

* Click all items
* Click Trash

### Choose menu “Menus – menu trash”

* Click “#” to select all check boxes
* Click Delete
* Click Delete

### Choose menu “Components – News Feeds – Feeds”

* Click “#” to select all check boxes
* Click Delete
* Click Categories
* Click “#” to select all check boxes
* Click Delete
* Click New
* Title: What’s new
* Click Save
* Click Feeds
* Click New
* Name

## Add new articles

### Choose menu “Content – Section Manager”

* Click New
* Title: Articles
* Click Save

### Choose menu “Content – Category Manager”

* Click New
* Title: General
* Click Save
* Repeat with Development and Linux

### Choose menu “Content – Article manager”

* Click New
* Title: Welcome to articles.cybercow.se
* Section: Article
* Category: General
* Body: XXXXX
* Click Save
* (Repeat for all categorys)

### Choose menu “Content – Article manager”

* Click New
* Title: XXXX
* Section: Articles
* Category: General
* Body: XXXXX
* Click Save
* (Repeat for all categorys)

### Choose menu “Menus – Main menu”

* Click new
* Click article layout
* Title: Home
* Select Article: Welcome to article.cybercow.se
* Click Save
* Click Home
* Click Default

## Install Autgen Menu Extension

[Found here ](http://extensions.joomla.org/extensions/core-enhancements/menu-systems/menu-editors/7191/details)

    cd ~
    wget http://blog.janzikmund.cz/wp-content/uploads/2009/01/mod_autgen_menu.zip

### Choose menu “Extensions- Install/Uninstall”

* Browse the file
* Click Update file & Install

### Choose menu “Extensions – Module manager”

* Click “AutGen menu”
* Enter
Title: Articles
Enabled: yes
Position: Right
Choose section: Articles
Category ordering: By order as set in administration
Section ordering: By order as set in administration
* Click Save
* Repeat with Development/Linux)

### Install template

[Found here](http://www.themes-online.com/2009/03/motherbot-joomla/)

    cd templates
    wget http://www.themes-online.com/wp-content/uploads/2009/03/motherbot.zip
    unzip motherbot.zip
    rm motherbot.zip

### Choose menu “Extensions – Template manager”

* Click “Your favorite Template”
* Click default

## Site configuration

### Choose menu “Site – User Manager”    Click Administrator

* Name: Daniel Lindh
* Click Save

### Choose menu “Site -  Global configuration”

* Click Site tab
* Write Global site Meta desc
* Write Global Site Meta Keywords
* Search Engine Friendly URL: yes
* Use Apache mod_rewrite: Yes
* Add suffix to URLs: yes
* Click system tab
* Allow User Registration: No
* New User Account Activation: No
* Front-end User Parameters: No
* Click server tab
* Time zone: UTC +1
* Click save

### Choose “Components – Polls”

* Click check box for “Joomla is used for?”
* Click delete
* Click new
Title/Alias: This site
Published: yes
Options: “Is crap”, “Is good”, “I’ll be back”
* Click save

### Choose “Components – Banner – Banners”

* Click “#” all Text ads but Joomla
* Click delete
* Click joomla
Title: Hyra fritidshus
Custom Banner Code: ….
* Click save

### Choose “Components – Banner – Client manager”

* Click “Open source matters.”
Client name: Sveaborg
Contact email: xxx@cybercow.se
* Click save

### Choose menu “Site -  Module Manager”

* Choose “Login Form”
Enabled: Yes
Position: Right
Menus: All
* Click Save
* Choose User Menu
Position:Right
Click Save
Choose Polls
Position: Right
Menus: All
Poll: This site
* Click save
* Choose Advertisement
Show title: no
Menus: all
Footer text: [remove]
* Click save
* Choose Random Image
Show title: no
Menus all
Image folder: images/stories
* Click save
* Choose Latest news
Title: Latest Articles
Position:right
Show title: yes
Enabled: yes
Order: Recently modified first
Menus: all
* Click save
* Choose “Who’s Online”, “Footer”, “Statistics”, “Archive”, “Sections”, “Related Items”, “Wrapper”, “Feed display”, “Main menu”, “breadcrumbs”, “News flash”, “Popular”
* Click Delete
* Change the orders for the componets to reflect this order.
Latest Articles
Articles
User Menu
Advertisement
Polls
Random image
Login form
* Click the floppy to save.

## Install Google Adsense plugin
[Found here ](http://extensions.joomla.org/extensions/ads-&-affiliates/google-ads/243/details)

[File to download](http://www.joomlaspan.com/index2.php?option=com_sobi2&sobi2Task=dd_download&fid=5&no_html=1)

## Choose menu “extensions- Install/Uninstall”

* Browse the file
* Click Update file & Install

### Choose menu “Site -  Module Manager”    Choose “Sponsored links”

* Enter
Enabled: Yes
Show title: No
Position: Right
Menus: All
* Click Save
* Order field for Sponsored Links 10
Click floppy to save.

## Install Google analytics plugin
[Found here](http://extensions.joomla.org/extensions/site-management/site-analytics/6170/details)

[File to download:](http://www.thinkbigshot.com/images/stories/extensions/plugin_bigshot_google_analytics.zip)

### Choose menu “extensions- Install/Uninstall”

* Browse the file
* Click Update file & Install

### Choose menu “Site -  Plugin manager”

* Choose “System – BIGSHOT Google Analytics”
Enabled: Yes
Web Property ID: {found on www.google.com/analytics}
* Click Save

## Install sh404sef Extension

[Found here](http://joomlacode.org/gf/project/sh404sef/frs/)

[File to download:](http://joomlacode.org/gf/download/frsrelease/9236/34672/com_sh404SEF-15_1.0.16_Beta_build_222.joomla1.5.x.zip)

### Choose menu “extensions- Install/Uninstall”

* Browse the file
* Click Update file & Install

### Choose menu “Components – sh404SEF”

* Click “Click here to extended display..”
* Choose sh404sef cofiguration
Enabled: yes
Unique ID: yes
* Click tab Plugins
* Click yes for all under “Content configuration”
* Click tab Advanced
Rewriting mode “With .htaccess)
Encode URL: yes
* Click tab META/SEO
Insert h1 tags: yes
* Click save

## Install chrono comments extension
[Found here](http://extensions.joomla.org/extensions/contacts-&-feedback/articles-comments/5839/details)

[File to download:]( http://www.chronoengine.com/downloads/75-ChronoComments%201.2%20Express%20Install.zip.html)

[File to download]( http://www.chronoengine.com/downloads/74-ChronoComments%201.2%20plugin.zip.html)

[File to download](http://www.chronoengine.com/downloads/77-mod_chronocomments%201.2.zip.html)

### Choose menu “extensions- Install/Uninstall”

* Browse the “ChronoComents…. Express Install” file
* Click Update file & Install
* Browse the “mod_chronocomments” file
* Click Update file & Install

### Choose menu “Components – Chrono comments – comments manager”

* Click Parameters
Sections: Articles
Show comments link in blog view: Enabled
Guest can post: yes
New comments default to: Published
Show Captcha: Enabled
Enable BBCode: Yes
Enable BBCode Panel: yes
Enable sh404SEF mode: yes
* Click save
