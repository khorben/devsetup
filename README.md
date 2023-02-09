# Development environment

## Getting started

The `devsetup` script contains everything required.

### Bootstrapping

Bootstrapping and managing the environment can be done entirely with `devsetup`.
Everything is built from source, and the resulting environment should be
consistent regardless of the host platform, including when deploying in staging
or production.

This is thanks to pkgsrc (https://pkgsrc.org,
https://www.netbsd.org/docs/pkgsrc/), the packaging system for NetBSD, which is
portable and should work equally well on all \*BSDs, Linux, Windows, Illumos,
and more.

Basically, to get started, simply run:

```shell-session
$ ./devsetup init
```

This should provide a working environment, after a fair amount of compilation
work.

### Environment

For convenience, it is also possible to run:

```shell-session
$ $(./devsetup environment)
```
    
This should:

* Prefix `$MANPATH` with `~/opt/pkgsrc-2022Q4/man` if necessary
* Prefix `$PATH` with `~/opt/pkgsrc-2022Q4/sbin:~/opt/pkgsrc-2022Q4/bin` if
  necessary
* Alias `devsetup` within the environment

### Usage

The usage screen is as follows:

```shell-session
$ ./devsetup -?
devsetup: -?: Command not supported
Usage: devsetup clean|init|environment [-qv]
       devsetup install|configure|update|uninstall [-qv][package...]
       devsetup start|stop|status|restart [-qv][service...]

devsetup clean [-qv]
  Removes the temporary files resulting from compilation.

devsetup init [-qv]
  Clones the package repositories and installs the default packages.

devsetup environment [-qv]
  Outputs variables needing to be updated to help work with devsetup.

devsetup install|configure|update|uninstall [-qv][package...]
  Installs, configures, updates, or uninstalls specific packages, or the list of
  default packages.

devsetup start|stop|status|restart [-qv][service...]
  Starts, stops, checks, or restarts specific services, or the list of default
  services.
```

### Examples

To install additional packages:

```shell-session
$ devsetup install misc/figlet misc/figlet-fonts
devsetup: Installing packages...
devsetup: misc/figlet: Package already installed (figlet-2.2.5nb1)
devsetup: misc/figlet-fonts: Package already installed (figlet-fonts-20021023nb1)
devsetup: Packages installed
devsetup: Configuring the packages...
devsetup: Packages configured
```

To monitor the services:

```shell-session
$ devsetup status -q
dovecot is running as pid 41645.
nginx is running as pid 45946.
pgsql is running as pid 41667 41669 41670 41671 41672 41673 41674.
php_fpm is running as pid 45975.
slapd is running as pid 41763.
```

To restart specific services:

```shell-session
$ devsetup restart nginx php_fpm
devsetup: nginx: Restarting service...
Stopping nginx.
Waiting for PIDS: 41654.
Starting nginx.
devsetup: nginx: service restarted
devsetup: php_fpm: Restarting service...
Stopping php_fpm.
Waiting for PIDS: 41681.
Starting php_fpm.
devsetup: php_fpm: service restarted
```

## Assumptions

1. `devsetup` installs the compiled environment in `~/opt/pkgsrc-2022Q4`

## Services

The services running by default are:

| *Name*    | *Software* | *Package*                | *Address*        | *Comments*                                           |
| --------- | ---------- | ------------------------ | ---------------- | ---------------------------------------------------- |
| `dovecot` | Dovecot    | `mail/dovecot2`          | `127.0.0.1:1143` | [webmail](http://127.0.0.1:8080/webmail)             |
| `nginx`   | Nginx      | `www/nginx`              | `127.0.0.1:8080` | [default page](http://127.0.0.1:8080/devsetup)       |
| `pgsql`   | PostgreSQL | `databases/postgresql13` | `127.0.0.1:5432` | [administration](http://127.0.0.1:8080/phppgadmin)   |
| `php_fpm` | PHP-FPM    | `www/php-fpm`            | `127.0.0.1:9000` | Fast CGI                                             |
| `slapd`   | OpenLDAP   | `databases/openldap`     | `127.0.0.1:3389` | [administration](http://127.0.0.1:8080/phpldapadmin) |

Every service can be configured to run on a different address; this can be
useful when using a VM for `devsetup` for instance.

## Configuration

A lot of the behaviour can otherwise be configured locally in
`~/.config/devsetup.conf`.
