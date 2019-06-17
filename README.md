# Nginx/Dehydrated config template

This has been extracted from my own servers. It supports best practices with regards
to security while also being quite flexible and (in my opinion) understandable and
organized.

## Usage

This repo reflects the filesystem layout for all files that are to be changed.

In some files there are placeholders to be replaced, notably:

* `/etc/dehydrated/config`: replace `__YOUR_EMAIL_HERE__` with the contact email
  used for Let's Encrypt
* `/etc/nginx/nginx.conf` and `/etc/nginx/sites-available/_template`:
  replace `__SERVER_IP__` (and, optionally, `__SERVER_IP6__`) with your server's
  public or NAT IPv4 (and IPv6, respectively) addresses.
  (Within `/etc/nginx/sites-available/_template`, the `__DOMAIN__` placeholder
  should remain -- it's used by the `provision-domain` script.)

Within `/etc/nginx/nginx.conf` within comments there are some notes with regards
to files that have to be created or generated for a particular instance.

* `/etc/nginx/dh.pem` should contain the Diffie-Hellmann parameters, which can
  be generated via `openssl dhparam -out /etc/nginx/dh.pem 4096`
* `/etc/nginx/ssl-stapling-set.pem` should contain the chain of trust for
  used certificates; if you're using Let's Encrypt, an example is provided
  within lines 51 through 55.
* `/etc/nginx/default_cert.crt` and `default_cert.key` are the certificates
  that are used when your server is accessed via an unknown hostname. Those
  can be anything you want, but lines 77-80 provide an example of generating
  a self-signed cert.
* If you want access logs, uncomment line 21.

## Directory structure

This template imposes a particular directory structure and expects some files
to be in certain places. Notably:

* Nginx virtual host configuration is contained within `/etc/nginx/sites-available`
  and `sites-enabled`, and a good practice is to define no more than two servers
  (one for a plain HTTP redirect to HTTPS, and the other for HTTPS) per file.
* Static file roots are located within `/var/www/sites`, so the root for
  `example.com` would be within `/var/www/sites/example.com/public`.

Also provided is a script which automates setting up a new site:
[provision-domain](./usr/sbin/provision-domain). It's written with this
particular template in mind and expects to be run within a POSIX-compliant
environment. (Note that, depending on your system, the `http` user and/or
group on line 38 might have to be changed. For Debian/Ubuntu, this needs to
be `www-data`, if I recall correctly.)

## Further questions?

Feel free to contact me over ways provided within <https://pn.id.lv>. I'll be
glad to help.

## License

```
Copyright (c) 2019 paulsnar (Github: @paulsnar)

Permission to use, copy, modify, and/or distribute this software for any
purpose with or without fee is hereby granted.

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES WITH
REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY
AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR ANY SPECIAL, DIRECT,
INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM
LOSS OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR
OTHER TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR
PERFORMANCE OF THIS SOFTWARE.
```
