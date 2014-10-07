# Change mail attachment size

[TOC]

To change mail attachment size, we have to change 3 settings.

## Change message size limit in postfix

Postfix is MTA, so we have to change its setting to transfer mail with large
attachment.

To allow mail with 100Mb attachment, please change 'message_size_limit' setting
like below:

```
# postconf -e message_size_limit='104857600'
```

Restart postfix to make it work:

```
# /etc/init.d/postfix restart
```

__NOTES__:

* `104857600` is 100 (MB) x 1024 (KB) x 1024 (Bit).
* Mail will be encoded by mail user agent (Outlook, Thunderbird, etc) before
  transferred, the actual message size will be larger than 100MB, you can
  simplily increase above setting to 110Mb or 120Mb to make it work as expected.

If you use mail clients such as Outlook, thunderbird to send mails, it's now
ok to sent large attachment with above setting.

## Change upload file size in webmail

If you have webmail, you have to change two more settings:

### Change PHP setting to allow to upload large attachment

You should change `memory_limit`, `upload_max_filesize` and `post_max_size` in
PHP config file `/etc/php.ini`

* on RHEL/CentOS: it's `/etc/php.ini`
* on Debian/Ubuntu, it's `/etc/php5/apache2/php.ini`
* on FreeBSD, it's `/usr/local/etc/php.ini` for Apache, or
  `/etc/php5/fpm/php.ini` for Nginx.
* on OpenBSD, it's `/etc/php-5.4.ini`. If you're running different PHP release,
  the version number `5.4` will be different.

```
memory_limit = 200M;
upload_max_filesize = 100M;
post_max_size = 100M;
```

### Change Roundcube webmail settings to allow large attachment

Change same settings in file `.htaccess` under roundcube root directory:

* on RHEL/CentOS, it's `/var/www/roundcubemail/.htaccess`
* on Debian/Ubuntu, it's `/usr/share/apache2/roundcubemail/.htaccess`
* on FreeBSD, it's `/usr/local/www/roundcubemail/.htaccess`
* on OpenBSD, it's `/var/www/roundcubemail/.htaccess`

```
php_value    memory_limit   200M
php_value    upload_max_filesize    100M
php_value    post_max_size  100M
```

Restart Apache or Nginx web server to make it work.