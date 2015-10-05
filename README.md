# owa-to-pop3

Suppose that you work for a company that uses Outlook as their email solution.
If you want to sync your email account with another client (like Gmail or
Fastmail), you can do it using POP3. But if the POP3 access is disabled, you're
almost out of luck. Unless if you use something like [DavMail][davmail].

> DavMail is a POP/IMAP/SMTP/Caldav/Carddav/LDAP exchange gateway allowing users
> to use any mail/calendar client (e.g. Thunderbird with Lightning or Apple
> iCal) with an Exchange server.

This repo is an Ansible playbook that installs DavMail on a VPS running FreeBSD,
creating a POP3 proxy for any Outlook/Exchange-based email.

## Requirements

* A VPS running FreeBSD. You can get one on [Digital Ocean][referral].
* [Ansible][ansible].
* An Outlook mail server.

## Gmail users

Gmail no longer accepts POP3 connections with self-signed SSL certs. So you must
have a certificate issued by a valid Certificate Authority (CA).

## Usage

First you must have a PKCS12 key in your VPS. You can create one with the
following command:

```
keytool -genkey -keyalg rsa -keysize 2048 -storepass <password> \
  -keystore /usr/local/etc/davmail/davmail.p12 -storetype pkcs12 \
  -validity 3650 -dname cn=<yourdomain.com>,ou=davmail,o=sf,o=net
```

Please note that you must provide a _password_ in the argument _-storepass_.
Do not forget to keep it secret. Also change _yourdomain.com_ with your
own domain.

Copy configuration files and change them properly with your own information:

```
cp group_vars/davmail.yml.example group_vars/davmail.yml
cp hosts.example hosts
```

Now you can run the playbook:

```
ansible-playbook davmail.yml
```

[davmail]: http://davmail.sourceforge.net/
[referral]: https://www.digitalocean.com/?refcode=32f57d59d5c4
[ansible]: http://docs.ansible.com/index.html
