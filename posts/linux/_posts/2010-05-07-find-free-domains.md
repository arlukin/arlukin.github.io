---
layout: post
title: Find free domains
---

A couple of days ago I was starting to think about a new company name,
the current Awxire AB is not as good as it could be. I like the name
myself, but other people can hardly pronounce it or spell it, and I
can’t blame them.

What I’m looking for is something short, just one word, easy to
pronounce, easy to spell, sound good both in english and Swedish, start
with an "A" to get a high position in alphabetical ordered lists, if
possible it shouldn’t generate any hits on google (so I easily can find
all internet relations to the company), if possible tell something about
the company but not to much (if I’d like to change products in the future,
Smart Home System is not a good name for an oil company) and the .
com domain should be free.

The last requirement gave me the idea to write a short php script, to
check all available .com addresses from a.com to azzzz.com wich is about
456976 domains. I did some googling first for a script like this, but
didn’t find anything I liked. So I thought that this is something that
I can write myself in 30-60 minutes. But hell I was wrong, it took me
about 10 hours, and I still can’t understand why. It’s so simple.

You can download the script [here](/projects/freedomains/freedomains.sh)
and [view](/projects/freedomains/freedomains.sh.txt) it here, and the
result aftera 30 hours run can be downloaded [here](/projects/freedomains/freedomains.txt.zip).

The script uses [pcntl_fork](http://www.php.net/manual/en/function.pcntl-fork.php)
so your php installation needs to be compiled with –enable-pcntl, you
can read more [here](http://www.php.net/manual/en/pcntl.installation.php).
You also need to have the phpwhois from [www.phpwhois.org/](http://www.phpwhois.org/)
installed in the same directory as the script. I did use phpwhois-4.1.3.tar.gz
and have a mirror of that file [here](/projects/freedomains/phpwhois-4.1.3.tar.gz),
just to be sure the right installation will be available in case future
releases will not work with my script.

You can now execute the script like this.

    # Create the SQLite database
    ./freedomain.sh 1

    # Scan all domains from aa.com to az.com
    ./freedomain 2 2 a .com 1

    #Count number of scanned domains
    freedomain 3

    # List all scanned domains in the database with the wildcard an%.com
    # that are not registered.
    freedomain 4 an%.com 0
