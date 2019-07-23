# site

Performs website specific operations

Usage :

```bash
wo site (command) [options]
```

| subcommand               | description                       |
|--------------------------|-----------------------------------|
| [create](#site-create)   | Create site with WordOps          |
| [update](#site-update)   | Update site type or configuration |
| [show](#site-show)       | Show site Nginx configuration     |
| [edit](#site-edit)       | Edit site Nginx configuration     |
| [delete](#site-delete)   | Delete site                       |
| [list](#site-list)       | list all sites                    |
| [enable](#site-enable)   | Enable site in Nginx              |
| [disable](#site-disable) | Disable site in Nginx             |
| [cd](#site-cd)           | Move into site webroot directory  |

## site create

<video align="center" src="/images/wo-site.webm" width="720" autoplay loop>
</video>

### Usage

```bash
wo site create  [<site_name>] [options]
```

### Basic sites

#### HTML site

To create simple html website use this command.

```bash
wo site create site.tld --html
```

#### PHP site

To create simple php website with no database use this command.

```bash
wo site create site.tld --php
```

#### PHP+MySQL site

To create simple php website with database use this command.

```bash
wo site create site.tld --mysql
```

NOTE: You can find MySQL database details in `/var/www/site.tld/wo-config.php`.

#### Proxy site

To create site with Proxy configuration you can use --proxy during site creation

```bash
wo site create site.tld --proxy=127.0.0.1:3000
```

This will create proxy site site.tld with proxy destination as 127.0.0.1:3000. Port is optional. Default port : 80.

### WordPress

Following are the WordPress website types you can create website based on Cache Mechanism

#### Standard sites

| cache          | PHP     | example                                     |
|----------------|---------|---------------------------------------------|
| no cache       | PHP 7.2 | `wo site create site.tld --wp`              |
| no cache       | PHP 7.3 | `wo site create site.tld --wp --php73`      |
| fastcgi_cache  | PHP 7.2 | `wo site create site.tld --wpfc`            |
| fastcgi_cache  | PHP 7.3 | `wo site create site.tld --wpfc --php73`    |
| wp-super-cache | PHP 7.2 | `wo site create site.tld --wpsc`            |
| wp-super-cache | PHP 7.3 | `wo site create site.tld --wpsc --php73`    |
| redis-cache    | PHP 7.2 | `wo site create site.tld --wpredis`         |
| redis-cache    | PHP 7.3 | `wo site create site.tld --wpredis --php73` |

#### Multisite subdirectory

| cache          | PHP     | example                                                |
|----------------|---------|--------------------------------------------------------|
| no cache       | PHP 7.2 | `wo site create site.tld --wpsubdir`                   |
| no cache       | PHP 7.3 | `wo site create site.tld --wpsubdir --php73`           |
| fastcgi_cache  | PHP 7.2 | `wo site create site.tld --wpsubdir --wpfc`            |
| fastcgi_cache  | PHP 7.3 | `wo site create site.tld --wpsubdir --wpfc --php73`    |
| wp-super-cache | PHP 7.2 | `wo site create site.tld --wpsubdir --wpsc`            |
| wp-super-cache | PHP 7.3 | `wo site create site.tld --wpsubdir --wpsc --php73`    |
| redis-cache    | PHP 7.2 | `wo site create site.tld --wpsubdir --wpredis`         |
| redis-cache    | PHP 7.3 | `wo site create site.tld --wpsubdir --wpredis --php73` |

#### Multisite subdomain

| cache          | PHP     | example                                                |
|----------------|---------|--------------------------------------------------------|
| no cache       | PHP 7.2 | `wo site create site.tld --wpsubdom`                   |
| no cache       | PHP 7.3 | `wo site create site.tld --wpsubdom --php73`           |
| fastcgi_cache  | PHP 7.2 | `wo site create site.tld --wpsubdir --wpfc`            |
| fastcgi_cache  | PHP 7.3 | `wo site create site.tld --wpsubdom --wpfc --php73`    |
| wp-super-cache | PHP 7.2 | `wo site create site.tld --wpsubdom --wpsc`            |
| wp-super-cache | PHP 7.3 | `wo site create site.tld --wpsubdom --wpsc --php73`    |
| redis-cache    | PHP 7.2 | `wo site create site.tld --wpsubdom --wpredis`         |
| redis-cache    | PHP 7.3 | `wo site create site.tld --wpsubdom --wpredis --php73` |

#### Cheatsheet

| Cache                   | single site | multisite w/ subdir  | multisite w/ subdom     |
|-------------------------|-------------|----------------------|-------------------------|
| **NO Cache**            | --wp        | --wpsubdir           | --wpsubdomain           |
| **WP Super Cache**      | --wpsc      | -wpsubdir --wpsc     | --wpsubdomain --wpsc    |
| **Nginx fastcgi_cache** | --wpfc      | --wpsubdir --wpfc    | --wpsubdomain --wpfc    |
| **Redis cache**         | --wpredis   | --wpsubdir --wpredis | --wpsubdomain --wpredis |

#### Extra settings

Define WordPress administrator user To define wordpress administrator user during site creation use

```bash
wo site create site.tld --user=admin
```

This will create admin as administrator user in wordpress during installation. If not defined it will take git user name.

Define WordPress administrator password To define wordpress administrator password during site creation use

```bash
wo site create site.tld --pass=password
```

This will set defined password as administrator password. If not defined it will generate random pasword for administrator. If you have special characters, you can quote them using single quotes like this :

--pass='my$secret&' Define WordPress administrator email To define wordpress administrator email during site creation use

```bash
wo site create site.tld --email=wo@site.tld
```

This will set defined email as administrator email. If not defined it will set git email as administrator email.

### Additional features

#### Let's Encrypt

WordOps supports Let's Encrypt out of the box.

##### Domain

```bash
wo site create site.tld --wp --letsencrypt
```

This command will issue a certificate for site.tld + www.site.tld.

##### Subdomain

You can also issue Let's Encrypt certificates with subdomains.

```bash
wo site create sub.site.tld --wp --letsencrypt=subdomain
```

##### Wildcard

**Since the release v3.9.6**, WordOps supports Let's Encrypt Wildcard SSL certificates with DNS API validation. Before issuing a wildcard certificate, it require to define the DNS API crendentials for acme.sh.

<video align="center" src="/images/wo-wildcard.webm" width="720" autoplay loop>
</video>


Example with Cloudflare DNS :

```bash
export CF_Key="sdfsdfsdfljlbjkljlkjsdfoiwje"
export CF_Email="xxxx@sss.com"
```

!!! info
    More example in our guide about [DNS API configuration](/how-to/configure-letsencrypt-dns-api-validation.md)

After you define those variables with the command `export`, you can issue your certificate with

```bash
wo site create site.tld --wp --letsencrypt=wildcard --dns=dns_cf
```

* `--dns=dns_cf` can be replaced with another DNS provider supported by acme.sh. For DigitalOcean, it would be `--dns=dns_do`

#### HSTS

Additionally you can enable HSTS on your site by adding the flag `--hsts` with `--letsencrypt`

```bash
wo site create site.tld --wp --letsencrypt --hsts
```

#### PHP 7.3

To create site with PHP 7.3 you can use --php73 during site creation

For example, you can create WordPress site running on PHP 7.3 using following command:

```bash
wo site create site.tld --wp --php73
```

To create simple php site running with PHP 7.3 with no database, you can use this command :

```bash
wo site create site.tld --php73
```

## site update

Update site configuration

### Pre-update policy

`wo site update` command follows following procedure while updating current site.

Before Updating any site :

* Creates nginx configuration backup for site.
* Moves htdocs to backup while updating html/php/mysql site.
* Creates database dump in backup.
* While updating current mysql site WordOps uses same database for installing wordpress tables.
* All these backup are stored outside htdocs, in backup directory.

Example : updating site from basic wp to wp + fastcgi_cache :

<video align="center" src="/images/wo-site-update.webm" width="720" autoplay loop></video>

### Usage

Usage :

```bash
wo site update  [<site_name>] [options]
```

| options                    | description                                            |
|----------------------------|--------------------------------------------------------|
| `--html`                   | update to html site                                    |
| `--php`                    | update to php site                                     |
| `--mysql`                  | update to MySQL + PHP site                             |
| `--php73`                  | update site to PHP 7.3                                 |
| `--php73=off`              | disable PHP 7.3                                        |
| `--wp`                     | update site to WordPress without cache                 |
| `--wpfc`                   | update site to WordPress with fastcgi_cache            |
| `--wpsc`                   | update site to WordPress with wp-super-cache           |
| `--wpredis`                | update site to WordPress with redis-cache              |
| `--wpsubdir`               | update site to WordPress multisite on subdirectories   |
| `--wpsubdomain`            | update site to WordPress multisite on subdomains       |
| `--password`               | update admin password for a WordPress site             |
| `--letsencrypt`,`-le`      | secure site with Let's Encrypt SSL certificate         |
| `--letsencrypt=subdomain`  | secure site running on a subdomain with Let's Encrypt  |
| `--letsencrypt=wildcard`   | secure site/multisite with a wildcard SSL certificates |
| `--letsencrypt=off`        | disable Let's Encrypt SSL certificate                  |
| `--dns`, `--dns=<dns api provider>` | issue Let's Encrypt certificate with DNS validation. default : `dns_cf`    |
| `--hsts`                   | Enable HSTS on site secured with Let's Encrypt         |
| `--hsts=off`               | Disable HSTS                                           |

### Examples

Update a WordPress site without cache (`--wp`), to WordPress with Nginx fastcgi_cache

```bash
wo site update site.tld --wpfc
```

Update a WordPress site running with PHP 7.2 to PHP 7.3

```bash
wo site update site.tld --php73
```

Disable PHP 7.3 and use PHP 7.2 :

```bash
wo site update site.tld --php73=off
```

Update a WordPress site wiht Nginx fastcgi_cache to WordPress with redis-cache

```bash
wo site update site.tld --wpredis
```

## site info

Get site information including cache backend, PHP version or user database credentials

Usage :

```bash
wo site info [<site_name>]
```

## site delete

Delete site including webroot and database :

Usage :

```bash
wo site delete  [<site_name>] [options]
```

| options       | description                                |
|---------------|--------------------------------------------|
| `--no-prompt` | delete website without confirmation prompt |
| `--files`     | delete only website files                  |
| `--db`        | delete only database                       |

## site edit

Edit site Nginx configuration

Usage :

```bash
wo site edit [<site_name>]
```

You will be prompted to choose the text editor you prefer. Nano is highly recommended for beginners.

## site cd

Move into a site webroot directory

Usage :

```bash
wo site cd  [<site_name>]
```

## site list

List all sites managed with WordOps

Usage :

```bash
wo site list
```

## site show

Display site Nginx configuration

Usage :

```bash
wo site show  [<site_name>]
```

## site disable

Disable site Nginx vhost

Usage :

```bash
wo site disable  [<site_name>]
```

## site enable

Enable site Nginx vhost

Usage :

```bash
wo site enable  [<site_name>]
```