---
layout: post
title: FareOfficeLib
---

FareOfficeLib is a collection of general php functions that we are using
at Fareoffice Car Rental Solutions AB. Since 2001 we have created over
600 000 lines of PHP code. A big part of that code are general php
classes and functions, that can be used by useful for anybody.

Currently when writing this text, most of the “FareOfficeLib” code are
still in the closed source trunk. But our goal is to move as much of
that code into the FareOfficeLib trunk. This will take sometime, so be
patient. Another goal is that all code in the FareOfficeLib should be
well written and documented.

[Click here]({{ site.url }}/projects/fareofficelib/doc/) for the manual.

# Installation

## Step 1: Download and extract the code

[Download fareoffice0.4.tar.gz here.](http://articles.cybercow.se/w/projects/fareofficelib/fareofficelib0.4.tar.gz)

    # Change the folder to your document_root.
    cd /var/www
    wget http://articles.cybercow.se/projects/fareofficelib/fareofficelib0.4.tar.gz .
    tar zxf fareofficelib0.4.tar.gz
    rm fareofficelib0.4.tar.gz

## Step 2: Try the code

Change the path in fareofficelib/.htaccess Browse the file
http://localhost/fareofficelib/UnitTestHtDocs/index.php
Click [RUN ALL TESTS] to see that the lib works on your server.

## Step 3: Move the code to the right folders.

In the fareofficelib folder you find 3 different folders.

PhpInclude The contents of this folder should be moved to your php
include_path folder. If you don’t have a specified include_path folder,
you can just move the contents to your apache document_root folder.

In the fareofficelib root folder, you can find a .htaccess file, which
contains this row.

php_value include_path “.:/var/www/fareofficelib/PhpInclude”

UnitTestHtDocs This folder (not just the contents) should be moved to
your apache document_root folder. When browsing the UnitTestHtDocs/index.php
file from your web browser, you will execute all/some Unit Tests for this project.

Documentation This folder contains the manual for the FareOfficeLib.
