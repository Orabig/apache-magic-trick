## How to reproduce an unexplained bug in Apache 2.4.10 (Jessie) and turn it into a magic trick

```
$ docker build . -t apache2:2.4.10
$ watch 'docker run --rm -v $(pwd)/buggy.conf:/etc/apache2/sites-enabled/buggy.conf apache2:2.4.10 apache2ctl -V'
```

This shows an apache error (refreshed in real-time), looking like :

```
AH00526: Syntax error on line 6 of /etc/apache2/sites-enabled/buggy.conf:
Invalid command '}"', perhaps misspelled or defined by a module not included in the server configuration
Action '-V' failed.
The Apache error log may have more information.
```

## Magic trick 

Edit `buggy.conf`file : **Add or remove** ANY character inside the LogFormat definition and *Puff* : no more error !!!

(For information : this configuration file should work : LogFormat may contain any string, and `{ ... }` is not supposed to be interpreted : https://httpd.apache.org/docs/current/fr/mod/mod_log_config.html )

## Side note

I know that this bug does not exist in Apache:2.4.25 (which is used in stretch), thus there should be a specific fix between these versions. **If anyone could point it out to me, I'd be very glad (and ready to offer a beer)**