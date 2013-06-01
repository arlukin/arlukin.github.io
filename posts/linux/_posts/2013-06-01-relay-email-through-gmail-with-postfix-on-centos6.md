---
layout: post
title: Relay email through gmail with postfix on centos6.
---

Relay email through gmail with postfix on centos6.

Gist can be found here https://gist.github.com/arlukin/5689526

    #!/bin/bash
    #
    # Configure postfix to relay all emails through gmail. Tested on Centos 6.4.
    #
    # Also install mailx to send testmails.
    #
    # Based on
    #   http://mhawthorne.net/posts/postfix-configuring-gmail-as-relay.html
    #   http://carlton.oriley.net/blog/?p=31
    #   http://opentodo.net/2013/03/postfix-smtp-relay-to-smtp-gmail-com/
    #

    __author__="daniel.lindh@amivono.com"
    __copyright__="Copyright 2013, Amivono AB"


    ## CONFIG BEGIN

    # The gmail account to relay through
    GMAIL_USER="noreply@renter.se"
    GMAIL_PWD="1337BoysGetCrazyInTheHeadAndSuffer"

    # The email to send test mails to.
    TEST_EMAIL="daniel@cybercow.se"

    ## CONFIG END


    echo "Configure postfix to relay all emails through gmail."


    # Requires to be executed by root.
    if [ "`whoami`" != "root" ] ;
    then
        echo "
    ERROR:
    You must be root to run this script. You are loged in as \"`whoami`\".
    EXIT"
        exit
    fi


    # Only tested on Centos 6.4, dissalow other dists.
    grep "CentOS release 6." /etc/redhat-release > /dev/null || \
    (
        echo "
    ERROR:
    This script is only tested on centos 6.4. It will probably work on other
    Centos 6 versions, so that is allowed. But you are running an
    \"`cat /etc/redhat-release`\" so you are not allowed to run this script.
    EXIT"
    exit
    )


    # Install postifx rpm, usually installed by default.
    if ! rpm -q postfix > /dev/null ;
    then
        echo "* Install postfix rpm"
        yum install -y postfix
    fi


    # Make sure postfix has been built with the necessary dependencies.
    ldd `which postfix` | grep libsasl > /dev/null || \
    (
        echo "* Invalid postfix version" &&
        echo "EXIT"
        exit
    )

    ldd `which postfix` | grep libssl > /dev/null || \
    (
        echo "* Invalid postfix version" &&
        echo "EXIT"
        exit
    )


    # Install cyrus-sasl-plain to get CAs to accept google certs.
    if ! rpm -q cyrus-sasl-plain > /dev/null ;
    then
        echo "* Install cyrus-sasl-plain rpm"
        yum install -y cyrus-sasl-plain
    fi


    # Add postfix config at the bottom of the file /etc/postfix/main.cf.
    # The last setting for any option is the one that is saved, so anything
    # above this will not be affect these final settings:"
    if ! grep "### GMAIL RELAY CONFIG BEGIN" /etc/postfix/main.cf > /dev/null ;
    then
        echo "* Configure /etc/postfix/main.cf"

        cat >> /etc/postfix/main.cf << EOF

    ### GMAIL RELAY CONFIG BEGIN

    # Sets gmail as relay
    relayhost = [smtp.gmail.com]:587

    #
    # TLS parameters
    #
    smtpd_use_tls=yes
    smtpd_tls_session_cache_database = btree:${data_directory}/smtpd_scache
    smtp_tls_session_cache_database = btree:${data_directory}/smtp_scache
    smtp_tls_policy_maps = hash:/etc/postfix/tls_policy

    # Log the hostname of a remote SMTP server that offers STARTTLS,
    # when TLS is not already enabled for that server.
    smtp_tls_note_starttls_offer = yes

    #
    # SASL Configuration
    #

    # Use sasl when authenticating to foreign SMTP servers
    smtp_sasl_auth_enable = yes

    # Path to password map file
    smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd

    # Disallow methods that allow anonymous authentication
    smtp_sasl_security_options = noanonymous
    smtp_sasl_tls_security_options = noanonymous
    smtp_sasl_mechanism_filter = plain

    # To accept cert from gmail.
    smtpd_tls_CAfile=/etc/pki/tls/certs/ca-bundle.trust.crt
    smtp_tls_CAfile=/etc/pki/tls/certs/ca-bundle.trust.crt

    # Our box only allows ipv4
    inet_protocols = ipv4

    ### GMAIL RELAY CONFIG END
    EOF
    fi


    # Create /etc/postfix/sasl_passwd file with your GMail login credentials.
    if [ ! -f /etc/postfix/sasl_passwd.db ] ;
    then
        echo "* Configure /etc/postfix/sasl_passwd"
        cat > /etc/postfix/sasl_passwd << EOF
    [smtp.gmail.com]:587  $GMAIL_USER:$GMAIL_PWD
    EOF

        sudo postmap /etc/postfix/sasl_passwd

        #
        echo "* Set perimissons on /etc/postfix/sasl_passwd"
        chmod o-r /etc/postfix/sasl_passwd
        chmod o-r /etc/postfix/sasl_passwd.db
        chown root:root /etc/postfix/sasl_passwd
        chown root:root /etc/postfix/sasl_passwd.db
    fi


    # Create /etc/postfix/tls_policy file with your GMail login credentials.
    # Config to force the use of ssl with the gmail smtp server."
    if [ ! -f /etc/postfix/tls_policy.db ] ;
    then
        echo "* Configure /etc/postfix/tls_policy"
        cat > /etc/postfix/tls_policy << EOF
    [smtp.gmail.com]:587 encrypt
    EOF

        sudo postmap /etc/postfix/tls_policy

        #
        echo "* Set perimissons on /etc/postfix/tls_policy"
        chmod o-r /etc/postfix/tls_policy
        chmod o-r /etc/postfix/tls_policy.db
        chown root:root /etc/postfix/tls_policy
        chown root:root /etc/postfix/tls_policy.db
    fi


    #
    echo
    echo "Restart Postfix and your server are ready to relay through gmail."
    service postfix restart


    # Setup iptables
    echo "* Delete old iptables rules."
    iptables -D OUTPUT -p tcp -j postfix_out
    iptables -F postfix_out
    iptables -X postfix_out

    echo "* Add new iptables rules."
    iptables -N postfix_out
    iptables -I OUTPUT -p tcp -j postfix_out
    iptables -A postfix_out -p tcp -m multiport --dports 587,25 -j ACCEPT
    iptables -A postfix_out -p udp -m multiport --dports 587,25 -j ACCEPT


    # Install mailx rpm to be able to send emails from command line.
    if ! rpm -q mailx > /dev/null ;
    then
        echo "* Install mailx rpm"
        yum install -y mailx
    fi
    echo "* Send test mail"
    mail -s "Test mail from `hostname`" $TEST_EMAIL << EOF
    Just executed install-postfix on `hostname`.
    EOF
