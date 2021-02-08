---
id: central-platform
title: Central Platform
---

The Central Platform of Centreon is composed of:

- Web UI, a graphical interface to administrate, configure and visualize
  everything,
- Gorgone, a task manager for distributed architecture.

The following chapters describe how to compile each one of those components.

The tested distributions and versions are:

| Distribution    | Version | Gorgone | Web |
|-----------------|---------|---------|-----|
| CentOS          | 8.3     |         |     |
| CentOS          | 7.9     |         |     |
| Oracle Linux    | 8.3     |         |     |
| Debian          | 10.8    |         |     |
| Ubuntu          | 20.10   |         |     |
| Ubuntu          | 20.04   |         |     |
| openSUSE Leap   | 15.2    |         |     |

## Centreon Gorgone

To install and run Centreon Gorgone, you will need the following external
dependencies:

- Perl, the Perl interpreter and core modules,
- ZMQ, an asynchronous messaging library,
- libssh development files (for SSH communication),
- Perl libssh, a Perl binding to libssh,
- Perl compilation modules.

#### Common packages

Use the system package manager to install them:

<!--DOCUSAURUS_CODE_TABS-->

<!--CentOS 8-->

```shell

```

<!--CentOS 7-->

```shell

```

<!--Debian-->

```shell
apt install perl libzmq3-dev libzmq5 libssh-dev libextutils-makemaker-cpanfile-perl libmodule-build-perl libmodule-install-perl libcryptx-perl libschedule-cron-perl libcrypt-cbc-perl libjson-xs-perl libjson-pp-perl libxml-simple-perl libnet-smtp-ssl-perl libconfig-yaml-perl libyaml-libyaml-perl libdbd-sqlite3-perl libdbd-mysql-perl libdbi-perl libdata-uuid-perl libhttp-daemon-perl libhttp-message-perl libmime-base64-perl libdigest-md5-file-perl libwww-curl-perl libhttp-daemon-ssl-perl libnetaddr-ip-perl libhash-merge-perl libdata-clone-perl
```

<!--openSUSE-->

```shell
zypper install perl zeromq zeromq-devel libssh-devel perl-ExtUtils-MakeMaker perl-ExtUtils-MakeMaker-CPANfile perl-Module-Build perl-Module-Install perl-CryptX perl-Crypt-CBC perl-JSON-XS perl-JSON perl-XML-Simple perl-Net-SMTP-SSL perl-YAML perl-YAML-LibYAML perl-DBD-SQLite perl-DBD-mysql perl-DBI perl-Data-UUID perl-HTTP-Daemon perl-HTTP-Message perl-Digest-Perl-MD5 perl-HTTPS-Daemon perl-NetAddr-IP perl-Hash-Merge perl-Clone
```

perl(Net::Curl::Easy)

<!--END_DOCUSAURUS_CODE_TABS-->

#### Other packages

```shell
wget http://search.cpan.org/CPAN/authors/id/M/MO/MOSCONI/ZMQ-LibZMQ4-0.01.tar.gz
tar zxf ZMQ-LibZMQ4-0.01.tar.gz
cd ZMQ-LibZMQ4-0.01
sed -i -e "s/tools/.\/tools/g" Makefile.PL
perl Makefile.PL
make
make install
```

```shell
wget https://cpan.metacpan.org/authors/id/D/DM/DMAKI/ZMQ-Constants-1.04.tar.gz
tar zxf ZMQ-Constants-1.04.tar.gz
cd ZMQ-Constants-1.04
perl Makefile.PL
make
make install
```

```shell
git clone https://github.com/garnier-quentin/perl-libssh.git
cd perl-libssh
perl Makefile.PL
make
make install
```

### Prepare

#### Get sources

Centreon Gorgone can be checked out from the
[GitHub repository](https://github.com/centreon/centreon-gorgone)
or downloaded from Centreon
[download website](https://download.centreon.com/)

<!--DOCUSAURUS_CODE_TABS-->

<!--GitHub repository-->

```shell
git clone -b x.y.z https://github.com/centreon/centreon-gorgone
```

<!--Download website-->

```shell
wget http://files.download.centreon.com/public/centreon-gorgone/centreon-gorgone-x.y.z.tar.gz
tar xzf centreon-gorgone-x.y.z.tar.gz
```

<!--END_DOCUSAURUS_CODE_TABS-->

With x.y.z being the version you want to compile.

### Install

Run the installation script:

```shell
./install.sh -i
```

<!--DOCUSAURUS_CODE_TABS-->

<!--Debian-->

```shell
Do you accept the license ?
[y/n], default to [n]:
> y
------------------------------------------------------------------------
        Checking all needed binaries
------------------------------------------------------------------------
rm                                                         OK
cp                                                         OK
mv                                                         OK
/usr/bin/chmod                                             OK
/usr/bin/chown                                             OK
echo                                                       OK
more                                                       OK
mkdir                                                      OK
find                                                       OK
/usr/bin/grep                                              OK
/usr/bin/cat                                               OK
/usr/bin/sed                                               OK

------------------------------------------------------------------------
        Checking the mandatory folders
------------------------------------------------------------------------

Do you want to be asked for confirmation before creating missing resources ?
[y/n], default to [y]:
> n
Ask confirmation before creation                           NO

Where is your Gorgone log folder
default to [/var/log/centreon-gorgone]
> 
Path /var/log/centreon-gorgone                             OK

Where is your Gorgone database folder
default to [/var/lib/centreon-gorgone]
> 
Path /var/lib/centreon-gorgone                             OK

Where is your Gorgone config (etc) folder
default to [/etc/centreon-gorgone]
> 
Path /etc/centreon-gorgone                                 OK
Creating folder /etc/centreon-gorgone/config.d             OK
Path /etc/centreon-gorgone/config.d                        OK

Where are your Gorgone user's folder
default to [/usr/bin/]
> 
Path /usr/bin/                                             OK

Where are your Gorgone's perl files
default to [/usr/share/perl5]
> 
Path /usr/share/perl5                                      OK

Where is your sysconfig folder ?
default to [/etc/sysconfig]
> /etc/default
Path /etc/default                                          OK
------------------------------------------------------------------------
        Checking the required users
------------------------------------------------------------------------

What is the Gorgone group ?
default to [centreon-gorgone]
> 
Creating group centreon-gorgone                            OK

What is the Gorgone user ?
default to [centreon-gorgone]
> 
Creating user centreon-gorgone (Gorgone user)              OK

------------------------------------------------------------------------
        Adding Gorgone user to the mandatory folders
------------------------------------------------------------------------
Modify owner of /var/log/centreon-gorgone                  OK
Modify rights of /var/log/centreon-gorgone                 OK
Modify owner of /var/lib/centreon-gorgone                  OK
Modify rights of /var/lib/centreon-gorgone                 OK
------------------------------------------------------------------------
        Installing Gorgone daemon
------------------------------------------------------------------------
Creating and adding rights on gorgoned.service             OK
Creating and adding rights on gorgoned                     OK
Creating and adding rights on gorgoned                     OK
Creating and adding rights on config.yaml                  OK
Creating and adding rights on gorgoned                     OK
Creating and adding rights on gorgone_config_init.pl       OK
------------------------------------------------------------------------
        Starting gorgoned.service
------------------------------------------------------------------------
Created symlink /etc/systemd/system/multi-user.target.wants/gorgoned.service → /etc/systemd/system/gorgoned.service.
Created symlink /etc/systemd/system/centreon.service.wants/gorgoned.service → /etc/systemd/system/gorgoned.service.

###############################################################################
#                                                                             #
#                         Thanks for using Gorgone.                           #
#                          -----------------------                            #
#                                                                             #
#           Please add the configuration in a file in the folder :            #
#                           /etc/centreon-gorgone/config.d                             #
#                     Then start the gorgoned.service                         #
#                                                                             #
#                You can read the documentation available here :              #
#      https://github.com/centreon/centreon-gorgone/blob/master/README.md     #
#                                                                             #
#      ------------------------------------------------------------------     #
#                                                                             #
#     Report bugs at https://github.com/centreon/centreon-gorgone/issues      #
#                                                                             #
#                        Contact : contact@centreon.com                       #
#                          http://www.centreon.com                            #
#                                                                             #
#                          -----------------------                            #
#              For security issues, please read our security policy           #
#           https://github.com/centreon/centreon-gorgone/security/policy      #
#                                                                             #
###############################################################################
```

<!--openSUSE-->

```shell
Do you accept the license ?
[y/n], default to [n]:
> y
------------------------------------------------------------------------
        Checking all needed binaries
------------------------------------------------------------------------
rm                                                         OK
cp                                                         OK
mv                                                         OK
/usr/bin/chmod                                             OK
/usr/bin/chown                                             OK
echo                                                       OK
more                                                       OK
mkdir                                                      OK
find                                                       OK
/usr/bin/grep                                              OK
/usr/bin/cat                                               OK
/usr/bin/sed                                               OK

------------------------------------------------------------------------
        Checking the mandatory folders
------------------------------------------------------------------------

Do you want to be asked for confirmation before creating missing resources ?
[y/n], default to [y]:
> n
Ask confirmation before creation                           NO

Where is your Gorgone log folder
default to [/var/log/centreon-gorgone]
> 
Path /var/log/centreon-gorgone                             OK

Where is your Gorgone database folder
default to [/var/lib/centreon-gorgone]
> 
Path /var/lib/centreon-gorgone                             OK

Where is your Gorgone config (etc) folder
default to [/etc/centreon-gorgone]
> 
Path /etc/centreon-gorgone                                 OK
Creating folder /etc/centreon-gorgone/config.d             OK
Path /etc/centreon-gorgone/config.d                        OK

Where are your Gorgone user's folder
default to [/usr/bin/]
> 
Path /usr/bin/                                             OK

Where are your Gorgone's perl files
default to [/usr/lib/perl5/vendor_perl/5.26.1]
> 
Path /usr/lib/perl5/vendor_perl/5.26.1                     OK
------------------------------------------------------------------------
        Checking the required users
------------------------------------------------------------------------

What is the Gorgone group ?
default to [centreon-gorgone]
> 
Creating group centreon-gorgone                            OK

What is the Gorgone user ?
default to [centreon-gorgone]
> 
Creating user centreon-gorgone (Gorgone user)              OK

------------------------------------------------------------------------
        Adding Gorgone user to the mandatory folders
------------------------------------------------------------------------
Modify owner of /var/log/centreon-gorgone                  OK
Modify rights of /var/log/centreon-gorgone                 OK
Modify owner of /var/lib/centreon-gorgone                  OK
Modify rights of /var/lib/centreon-gorgone                 OK
------------------------------------------------------------------------
        Installing Gorgone daemon
------------------------------------------------------------------------
Creating and adding rights on gorgoned.service             OK
Creating and adding rights on gorgoned                     OK
Creating and adding rights on gorgoned                     OK
Creating and adding rights on config.yaml                  OK
Creating and adding rights on gorgoned                     OK
Creating and adding rights on gorgone_config_init.pl       OK
------------------------------------------------------------------------
        Starting gorgoned.service
------------------------------------------------------------------------
Impossible to install your run level for gorgoned          FAIL

###############################################################################
#                                                                             #
#                         Thanks for using Gorgone.                           #
#                          -----------------------                            #
#                                                                             #
#           Please add the configuration in a file in the folder :            #
#                           /etc/centreon-gorgone/config.d                             #
#                     Then start the gorgoned.service                         #
#                                                                             #
#                You can read the documentation available here :              #
#      https://github.com/centreon/centreon-gorgone/blob/master/README.md     #
#                                                                             #
#      ------------------------------------------------------------------     #
#                                                                             #
#     Report bugs at https://github.com/centreon/centreon-gorgone/issues      #
#                                                                             #
#                        Contact : contact@centreon.com                       #
#                          http://www.centreon.com                            #
#                                                                             #
#                          -----------------------                            #
#              For security issues, please read our security policy           #
#           https://github.com/centreon/centreon-gorgone/security/policy      #
#                                                                             #
###############################################################################
```

<!--END_DOCUSAURUS_CODE_TABS-->

## Centreon Web

### Prerequisites

To install and run Centreon Web, you will need the following external
dependencies:

- Apache 2.4, the web server,
- MariaDB 10.3, the database management system to store the configuration
  and realtime data,
- PHP 7.2 and common PHP libraries,
- Composer, a PHP package manager,
- npm, a JavaScript package manager,
- Perl 5 and some Perl libraries,
- RRDTool 1.7, the database management system to store metrics and draw graphs.

#### Common packages

Use the system package manager to install them:

<!--DOCUSAURUS_CODE_TABS-->

<!--CentOS 8-->

```shell
dnf install dnf-plugins-core epel-release
dnf config-manager --set-enabled powertools
dnf install httpd MariaDB-client MariaDB-common MariaDB-shared MariaDB-server net-snmp net-snmp-libs net-snmp-perl net-snmp-utils perl perl-Crypt-DES perl-DBD-MySQL perl-DBI perl-Digest-HMAC perl-Digest-SHA1 perl-HTML-Parser perl-IO-Socket-INET6 perl-rrdtool perl-Socket6 perl-Sys-Syslog php php-zip php-xml php-fpm php-process php-common php-pdo php-intl php-pear php-json php-mysqlnd php-ldap php-gd php-cli php-mbstring php-snmp rrdtool rrdtool-perl rsync sudo
```

<!--CentOS 7-->

```shell
yum install centos-release-scl
yum install httpd24-httpd MariaDB-client MariaDB-common MariaDB-shared MariaDB-server net-snmp net-snmp-libs net-snmp-perl net-snmp-utils perl perl-Crypt-DES perl-DBD-MySQL perl-DBI perl-Digest-HMAC perl-Digest-SHA1 perl-HTML-Parser perl-IO-Socket-INET6 perl-rrdtool perl-Socket6 perl-Sys-Syslog rh-php72 rh-php72-php-zip rh-php72-php-xml rh-php72-php-fpm rh-php72-php-process rh-php72-php-common rh-php72-php-pdo rh-php72-php-intl rh-php72-php-pear rh-php72-php-json rh-php72-php-mysqlnd rh-php72-php-ldap rh-php72-php-gd rh-php72-php-cli rh-php72-php-mbstring rh-php72-php-snmp rrdtool rrdtool-perl rsync sudo
```

<!--Debian-->

```shell
apt-get install wget apt-transport-https lsb-release ca-certificates
wget https://packages.sury.org/php/apt.gpg -O /etc/apt/trusted.gpg.d/php.gpg
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" >> /etc/apt/sources.list.d/php.list
apt-get update
apt-get install apache2 libapache2-mod-php7.2 libcrypt-des-perl libdigest-hmac-perl libdigest-sha-perl libgd-perl mariadb-client-10.3 mariadb-server-10.3 php7.2 php7.2-cli php7.2-common php7.2-curl php7.2-fpm php7.2-gd php7.2-intl php7.2-json php7.2-ldap php7.2-mbstring php7.2-mysql php7.2-opcache php7.2-readline php7.2-snmp php7.2-sqlite3 php7.2-xml php7.2-zip php-date php-pear rsync sudo
```

<!--openSUSE-->

```shell
zypper addrepo --priority 70 --gpgcheck --refresh https://yum.mariadb.org/10.3/sles/15/x86_64 mariadb103
zypper addrepo --priority 70 --gpgcheck --refresh https://download.opensuse.org/repositories/devel:/languages:/php:/php72/openSUSE_Leap_15.2 php72
zypper --gpg-auto-import-keys refresh
zypper install httpd MariaDB-client MariaDB-common MariaDB-shared MariaDB-server net-snmp libsnmp30 perl-Net-SNMP perl perl-Crypt-DES perl-DBD-mysql perl-DBI perl-Digest-HMAC perl-Digest-SHA1 perl-HTML-Parser perl-IO-Socket-INET6 perl-rrdtool perl-Socket6 php7 php7-ctype php7-curl php7-dom php7-fpm php7-gd php7-iconv php7-intl php7-json php7-ldap php7-mbstring php7-mysql php7-openssl php7-pdo php7-pear php7-pear-Archive_Tar php7-phar php7-snmp php7-sqlite php7-tokenizer php7-xmlreader php7-xmlwriter php7-zip php7-zlib rrdtool perl-rrdtool rsync sudo
```

perl-Sys-Syslog net-snmp-utils php-common php-cli php-process

<!--END_DOCUSAURUS_CODE_TABS-->

#### Configuration

<!--DOCUSAURUS_CODE_TABS-->

<!--CentOS 8-->

```shell
tee /etc/php.d/50-centreon.ini <<EOF
date.timezone = Europe/Paris
max_execution_time = 300
session.use_strict_mode = 1
session.gc_maxlifetime = 7200
EOF
```

<!--CentOS 7-->

```shell
tee /etc/opt/rh/rh-php72/php.d/50-centreon.ini <<EOF
date.timezone = Europe/Paris
max_execution_time = 300
session.use_strict_mode = 1
session.gc_maxlifetime = 7200
EOF
```

<!--Debian-->

```shell
a2enmod proxy_fcgi setenvif proxy rewrite
a2enconf php7.2-fpm
a2dismod php7.2
```

```shell
tee /etc/php/7.2/apache2/conf.d/50-centreon.ini <<EOF
date.timezone = Europe/Paris
max_execution_time = 300
session.use_strict_mode = 1
session.gc_maxlifetime = 7200
EOF
```

```shell
systemctl restart apache2 php7.2-fpm
```

```shell
mkdir /etc/systemd/system/mariadb.service.d
```

```shell
tee /etc/systemd/system/mariadb.service.d/limitnofile.conf <<EOF
[Service]
LimitNOFILE=32000
EOF
```

```shell
tee /etc/mysql/conf.d/centreon.cnf <<EOF
#
# Custom MySQL/MariaDB server configuration for Centreon
#
[server]
innodb_file_per_table=1
open_files_limit = 32000

key_buffer_size = 256M
sort_buffer_size = 32M
join_buffer_size = 4M
thread_cache_size = 64
read_buffer_size = 512K
read_rnd_buffer_size = 256K
max_allowed_packet = 8M

# For 4 Go Ram
#innodb_additional_mem_pool_size=512M
#innodb_buffer_pool_size=512M

# For 8 Go Ram
#innodb_additional_mem_pool_size=1G
#innodb_buffer_pool_size=1G
EOF
```

```shell
systemctl daemon-reload
systemctl restart mariadb
```

<!--openSUSE-->

```shell
tee /etc/php7/conf.d/centreon.ini <<EOF
date.timezone = Europe/Paris
max_execution_time = 300
session.use_strict_mode = 1
session.gc_maxlifetime = 7200
EOF
```

```shell
systemctl restart apache2 php-fpm
```

```shell
tee /etc/systemd/system/mariadb.service.d/limitnofile.conf <<EOF
[Service]
LimitNOFILE=32000
EOF
```

```shell
tee /etc/mysql/conf.d/centreon.cnf <<EOF
#
# Custom MySQL/MariaDB server configuration for Centreon
#
[server]
innodb_file_per_table=1
open_files_limit = 32000

key_buffer_size = 256M
sort_buffer_size = 32M
join_buffer_size = 4M
thread_cache_size = 64
read_buffer_size = 512K
read_rnd_buffer_size = 256K
max_allowed_packet = 8M

# For 4 Go Ram
#innodb_additional_mem_pool_size=512M
#innodb_buffer_pool_size=512M

# For 8 Go Ram
#innodb_additional_mem_pool_size=1G
#innodb_buffer_pool_size=1G
EOF
```

```shell
systemctl daemon-reload
systemctl restart mariadb
```

<!--END_DOCUSAURUS_CODE_TABS-->

--common

```shell
groupadd centreon
useradd -g centreon -m -r -d /var/spool/centreon centreon
```

```shell
usermod -aG centreon centreon-broker centreon-engine centreon-gorgone
```

### Prepare

#### Get sources

Centreon Web can be checked out from the
[GitHub repository](https://github.com/centreon/centreon)
or downloaded from Centreon
[download website](https://download.centreon.com/)

<!--DOCUSAURUS_CODE_TABS-->

<!--GitHub repository-->

```shell
git clone -b x.y.z https://github.com/centreon/centreon
```

<!--Download website-->

```shell
wget http://files.download.centreon.com/public/centreon/centreon-web-x.y.z.tar.gz
tar xzf centreon-web-x.y.z.tar.gz
```

<!--END_DOCUSAURUS_CODE_TABS-->

With x.y.z being the version you want to compile.

#### Composer

Install Composer:

```shell
wget https://getcomposer.org/installer -O composer-setup.php
php composer-setup.php --install-dir=/usr/bin --filename=composer
```

It should result to:

```shell
All settings correct for using Composer
Downloading...

Composer (version 2.0.9) successfully installed to: /root/composer.phar
Use it: php /usr/bin/composer
```

Then install the PHP dependencies from Centreon Web source folder:

```shell
cd /path_to_centreon_web
composer install --no-dev --optimize-autoloader
```

That should result as follow:

```shell
The "symfony/flex" plugin was skipped because it is not compatible with Composer 2+. Make sure to update it to version 1.9.8 or greater.
Installing dependencies from lock file
Verifying lock file contents can be installed on current platform.
Warning: The lock file is not up to date with the latest changes in composer.json. You may be getting outdated dependencies. It is recommended that you run `composer update` or `composer update <package name>`.
Nothing to install, update or remove
Package phpunit/php-token-stream is abandoned, you should avoid using it. No replacement was suggested.
Generating optimized autoload files
Class Tests\Centreon\Domain\Monitoring\StringConverterTest located in ./tests/php/Utility/StringConverterTest.php does not comply with psr-4 autoloading standard. Skipping.
Class Tests\Centreon\Application\Controller\Filter\FilterControllerTest located in ./tests/php/Centreon/Application/Controller/FilterControllerTest.php does not comply with psr-4 autoloading standard. Skipping.
Class Tests\Centreon\Application\Controller\Monitoring\Metric\MetricControllerTest located in ./tests/php/Centreon/Application/Controller/Monitoring/MetricControllerTest.php does not comply with psr-4 autoloading standard. Skipping.
Class Tests\Centreon\Application\Controller\Monitoring\AcknowledgementControllerTest located in ./tests/php/Centreon/Application/Controller/AcknowledgementControllerTest.php does not comply with psr-4 autoloading standard. Skipping.
Class Tests\Centreon\Domain\Monitoring\FilterServiceTest located in ./tests/php/Centreon/Domain/Filter/FilterServiceTest.php does not comply with psr-4 autoloading standard. Skipping.
Class CentreonLegacy\Core\Install\Step\stepInterface located in ./src/CentreonLegacy/Core/Install/Step/StepInterface.php does not comply with psr-4 autoloading standard. Skipping.
Class Centreon\Tests\Application\DataRepresenter\NavigationListTest located in ./src/Centreon/Tests/Application/DataRepresenter/Topology/NavigationListTest.php does not comply with psr-4 autoloading standard. Skipping.
Class Centreon\Tests\Application\DataRepresenter\CentreonValidatorTranslatorTest located in ./src/Centreon/Tests/Application/Validation/CentreonValidatorTranslatorTest.php does not comply with psr-4 autoloading standard. Skipping.
Class Centreon\Tests\Application\DataRepresenter\CentreonValidatorFactoryTest located in ./src/Centreon/Tests/Application/Validation/CentreonValidatorFactoryTest.php does not comply with psr-4 autoloading standard. Skipping.
Class Centreon\Tests\Infrastructure\CentreonLegacyDB\WebServiceAbstractTest located in ./src/Centreon/Tests/Infrastructure/Webservice/WebServiceAbstractTest.php does not comply with psr-4 autoloading standard. Skipping.
51 packages you are using are looking for funding.
Use the `composer fund` command to find out more!
```

#### npm

Install Node.js:

<!--DOCUSAURUS_CODE_TABS-->

<!--CentOS 8-->

```shell
```

<!--CentOS 7-->

```shell
```

<!--Debian-->

```shell
wget https://deb.nodesource.com/setup_14.x
chmod +x setup_14.x
./setup_14.x
apt-get install nodejs
```

<!--openSUSE-->

```shell
zypper install nodejs14
```

<!--END_DOCUSAURUS_CODE_TABS-->

Then install the JavaScript dependencies from Centreon Web source
folder:

```shell
cd /path_to_centreon_web
npm install
npm run build
rm -rf node_modules
```

### Install

Run the installation script:

```shell
cd /path_to_centreon_web
./install.sh -i
```

#### Prerequisites check

> If the Prerequisites installation step has been run successfully you should have
> no problem during this stage. Otherwise repeat the Prerequisites installation
> process:

```shell
###############################################################################
#                                                                             #
#                         Centreon (www.centreon.com)                         #
#                          Thanks for using Centreon                          #
#                                                                             #
#                                    vYY.MM.x                                 #
#                                                                             #
#                              infos@centreon.com                             #
#                                                                             #
#                   Make sure you have installed and configured               #
#         centreon-gorgone - sudo - sed - php - apache - rrdtool - mysql      #
#                                                                             #
###############################################################################
------------------------------------------------------------------------
        Checking all needed binaries
------------------------------------------------------------------------
rm                                                         OK
cp                                                         OK
mv                                                         OK
/bin/chmod                                                 OK
/bin/chown                                                 OK
echo                                                       OK
more                                                       OK
mkdir                                                      OK
find                                                       OK
/bin/grep                                                  OK
/bin/cat                                                   OK
/bin/sed                                                   OK

------------------------------------------------------------------------
        Check mandatory gorgone service status
------------------------------------------------------------------------

Is the Gorgone module already installed?
[y/n], default to [n]:
> y
```

#### License agreement

```shell
This General Public License does not permit incorporating your program into
proprietary programs.  If your program is a subroutine library, you may
consider it more useful to permit linking proprietary applications with the
library.  If this is what you want to do, use the GNU Library General
Public License instead of this License.

Do you accept GPLv2 license ?
[y/n], default to [n]:
> y
```

#### Main components

Answer **[y]** to all the questions:

```shell
------------------------------------------------------------------------
        Please choose what you want to install
------------------------------------------------------------------------

Do you want to install : Centreon Web Front
[y/n], default to [n]:
> y

Do you want to install : Centreon Nagios Plugins
[y/n], default to [n]:
> y

Do you want to install : CentreonTrapd process
[y/n], default to [n]:
> y
```

#### Definition of installation paths

```shell
------------------------------------------------------------------------
        Start CentWeb Installation
------------------------------------------------------------------------

Where is your Centreon directory ?
default to [/usr/local/centreon]
> /usr/share/centreon
Path /usr/share/centreon                                   OK

Where is your Centreon log directory ?
default to [/usr/local/centreon/log]
> /var/log/centreon

Do you want me to create this directory ? [/var/log/centreon]
[y/n], default to [n]:
> y
Path /var/log/centreon                                     OK

Where is your Centreon etc directory ?
default to [/etc/centreon]
>

Do you want me to create this directory ? [/etc/centreon]
[y/n], default to [n]:
> y
Path /etc/centreon                                         OK

Where is your Centreon variable state information directory ?
default to [/var/lib/centreon]
>

Do you want me to create this directory ? [/var/lib/centreon]
[y/n], default to [n]:
> y
Path /var/lib/centreon                                     OK

Where is rrdtool
default to [/usr/bin/rrdtool]
> /opt/rrdtool-broker/bin/rrdtool
/opt/rrdtool-broker/bin/rrdtool                            OK

/usr/bin/mail                                              OK

Where is your php binary ?
default to [/usr/bin/php]
>
/usr/bin/php                                               OK

Where is PEAR [PEAR.php]
default to [/usr/share/pear/PEAR.php]
> /usr/share/php/PEAR.php
Path /usr/share/php/PEAR.php                               OK
/usr/bin/perl                                              OK
Composer dependencies are installed                        OK
Frontend application is built                              OK
Enable Apache configuration                                OK
Conf centreon already enabled
Finding Apache user :                                      www-data
Finding Apache group :                                     www-data
```

#### Centreon user and group

Le groupe d'applications **centreon** est utilisé pour les droits d'accès
entre les différents logiciels de la suite Centreon:

```shell
What is the Centreon group ? [centreon]
default to [centreon]
>

What is the Centreon user ? [centreon]
default to [centreon]
>
```

#### Monitoring user

This is the user used to run the monitoring engine (Centreon Engine).

```shell
What is the Monitoring engine user ? [centreon-engine]
default to [centreon-engine]
>
```

This is the user used to run the stream broker (Centreon Broker).

```shell
What is your Centreon Broker user ? [centreon-broker]
default to [centreon-broker]
>
```

#### Monitoring logs directory

```shell
What is the Monitoring engine log directory ?[/var/log/centreon-engine]
default to [/var/log/centreon-engine]
>
Path                                                       OK
Path                                                       OK
Add group centreon to user www-data                        OK
Add group centreon to user centreon-engine                 OK
Add group centreon-engine to user www-data                 OK
Add group centreon-engine to user centreon                 OK
Add group www-data to user centreon                        OK
```

#### Sudo configuration

```shell
------------------------------------------------------------------------
        Configure Sudo
------------------------------------------------------------------------

Where is sudo configuration file ?
default to [/etc/sudoers.d/centreon]
>

Do you want me to create this file ? [/etc/sudoers.d/centreon]
[y/n], default to [n]:
>  y
/etc/sudoers.d/centreon                                    OK

What is the Monitoring engine binary ? [/usr/sbin/centengine]
default to [/usr/sbin/centengine]
>

Where is the Monitoring engine configuration directory ? [/etc/centreon-engine]
default to [/etc/centreon-engine]
>

Where is the configuration directory for broker module ? [/etc/centreon-broker]
default to [/etc/centreon-broker]
>

Where is your service command binary ?
default to [/usr/sbin/service]
>
Your sudo is not configured

Do you want me to configure your sudo ? (WARNING)
[y/n], default to [n]:
>  y
Configuring Sudo                                           OK
```

#### Apache configuration

```shell
------------------------------------------------------------------------
        Configure Apache server
------------------------------------------------------------------------

Do you want to add Centreon Apache sub configuration file ?
[y/n], default to [n]:
> y
Create '/etc/apache2/conf-available/centreon.conf'         OK
Configuring Apache                                         OK

Do you want to reload your Apache ?
[y/n], default to [n]:
> y
Reloading Apache service                                   OK
```

#### PHP FPM configuration

```shell
------------------------------------------------------------------------
        Configure PHP FPM service
------------------------------------------------------------------------

Do you want to add Centreon PHP FPM sub configuration file ?
[y/n], default to [n]:
> y
Creating directory /var/lib/centreon/sessions              OK
Create 'etc/php/7.2/fpm/pool.d/centreon.conf'              OK
Configuring PHP FPM                                        OK

Do you want to reload PHP FPM service ?
[y/n], default to [n]:
> y
Reloading PHP FPM service                                  OK
Preparing Centreon temporary files                         OK
Change right on /var/log/centreon                          OK
Change right on /etc/centreon                              OK
Change macros for insertBaseConf.sql                       OK
Change macros for sql update files                         OK
Change macros for php files                                OK
Change macros for php config file                          OK
Change macros for perl binary                              OK
Change right on /etc/centreon-engine                       OK
Add group centreon-broker to user www-data                 OK
Add group centreon-broker to user centreon-engine          OK
Add group centreon to user centreon-broker                 OK
Change right on /etc/centreon-broker                       OK
Copy CentWeb in system directory                           OK
Install CentWeb (web front of centreon)                    OK
Change right for install directory                         OK
Install libraries                                          OK
Write right to Smarty Cache                                OK
Change macros for centreon.cron                            OK
Install Centreon cron.d file                               OK
Change macros for centAcl.php                              OK
Change macros for downtimeManager.php                      OK
Change macros for centreon-backup.pl                       OK
Install cron directory                                     OK
Change right for eventReportBuilder                        OK
Change right for dashboardBuilder                          OK
Change right for centreon-backup.pl                        OK
Change right for centreon-backup-mysql.pl                  OK
Change macros for centreon.logrotate                       OK
Install Centreon logrotate.d file                          OK
Prepare centFillTrapDB                                     OK
Install centFillTrapDB                                     OK
Prepare centreon_trap_send                                 OK
Install centreon_trap_send                                 OK
Prepare centreon_check_perfdata                            OK
Install centreon_check_perfdata                            OK
Prepare centreonSyncPlugins                                OK
Install centreonSyncPlugins                                OK
Prepare centreonSyncArchives                               OK
Install centreonSyncArchives                               OK
Prepare generateSqlLite                                    OK
Install generateSqlLite                                    OK
Install changeRrdDsName.pl                                 OK
Prepare export-mysql-indexes                               OK
Install export-mysql-indexes                               OK
Prepare import-mysql-indexes                               OK
Install import-mysql-indexes                               OK
Prepare clapi binary                                       OK
Install clapi binary                                       OK
Centreon Web Perl lib installed                            OK
```

#### Pear module installation

```shell
------------------------------------------------------------------------
Pear Modules
------------------------------------------------------------------------
Check PEAR modules
PEAR                            1.4.9       1.10.8         OK
DB                              1.7.6       1.9.2          OK
Date                            1.4.6       1.4.7          OK
All PEAR modules                                           OK
```

#### Configuration file installation

```shell
------------------------------------------------------------------------
            Centreon Post Install
------------------------------------------------------------------------
Create /usr/share/centreon/www/install/install.conf.php    OK
Create /etc/centreon/instCentWeb.conf                      OK
```

#### Performance data component (Centstorage) installation

```shell
------------------------------------------------------------------------
        Starting CentStorage Installation
------------------------------------------------------------------------

Where is your Centreon Run Dir directory?
default to [/var/run/centreon]
>

Do you want me to create this directory ? [/var/run/centreon]
[y/n], default to [n]:
> y
Path /var/run/centreon                                     OK

Where is your CentStorage binary directory ?
default to [/usr/share/centreon/bin]
>
Path /usr/share/centreon/bin                               OK

Where is your CentStorage RRD directory ?
default to [/var/lib/centreon]
>
Path /var/lib/centreon                                     OK
Preparing Centreon temporary files
/tmp/centreon-setup exists, it will be moved...
install www/install/createTablesCentstorage.sql            OK
Creating Centreon Directory '/var/lib/centreon/status'     OK
Creating Centreon Directory '/var/lib/centreon/metrics'    OK
Change right : /var/run/centreon                           OK
Install logAnalyserBroker                                  OK
Install nagiosPerfTrace                                    OK
Change macros for centstorage.cron                         OK
Install CentStorage cron                                   OK
Create /etc/centreon/instCentStorage.conf                  OK
```

#### Plugin installation

```shell
------------------------------------------------------------------------
        Starting Centreon Plugins Installation
------------------------------------------------------------------------
Path                                                       OK
Path                                                       OK

Where is your CentPlugins lib directory
default to [/var/lib/centreon/centplugins]
>

Do you want me to create this directory ? [/var/lib/centreon/centplugins]
[y/n], default to [n]:
> y
Path /var/lib/centreon/centplugins                         OK
Create /etc/centreon/instCentPlugins.conf                  OK
```

#### Centreon SNMP trap management installation

```shell
------------------------------------------------------------------------
        Starting CentreonTrapD Installation
------------------------------------------------------------------------

Path                                                       OK
Path                                                       OK

Where is your SNMP configuration directory ?
default to [/etc/snmp]
>
/etc/snmp                                                  OK

Where is your CentreonTrapd binaries directory ?
default to [/usr/local/centreon/bin]
> /usr/share/centreon/bin
/usr/share/centreon/bin                                    OK

Finding Apache user :                                      www-data
Preparing Centreon temporary files
Change macros for snmptrapd.conf                           OK
Replace CentreonTrapd init script Macro                    OK
Replace CentreonTrapd default script Macro                 OK

Do you want me to install CentreonTrapd init script ?
[y/n], default to [n]:
> y
CentreonTrapd init script installed                        OK
CentreonTrapd default script installed                     OK

Do you want me to install CentreonTrapd run level ?
[y/n], default to [n]:
> y
trapd Perl lib installed                                   OK
Install : snmptrapd.conf                                   OK
Install : centreontrapdforward                             OK
Install : centreontrapd                                    OK
Change macros for centreontrapd.logrotate                  OK
Install Centreon Trapd logrotate.d file                    OK
Create /etc/centreon/instCentPlugins.conf                  OK
```

#### End

```shell
###############################################################################
#                                                                             #
#                         Thanks for using Centreon.                          #
#                          -----------------------                            #
#                                                                             #
#                 Go to the URL : http://localhost.localdomain/centreon/      #
#                          to finish the setup                                #
#                                                                             #
#                Please read the documentation available here :               #
#                         documentation.centreon.com                          #
#                                                                             #
#      ------------------------------------------------------------------     #
#                                                                             #
#         Report bugs at https://github.com/centreon/centreon/issues          #
#                                                                             #
#                        Contact : contact@centreon.com                       #
#                          http://www.centreon.com                            #
#                                                                             #
#                          -----------------------                            #
#              For security issues, please read our security policy           #
#              https://github.com/centreon/centreon/security/policy           #
#                                                                             #
###############################################################################
```

### Any operating system

SELinux should be disabled; for this, you have to modify the
file **/etc/sysconfig/selinux** and replace **enforcing** by
**disabled**:

```shell
SELINUX=disabled
```

## Web installation

Conclude installation by performing
[web installation steps](../web-and-post-installation.html#web-installation).
