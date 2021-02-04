---
id: collect-stack
title: Collect Stack
---

## Centreon Clib

### Prerequisites

To build Centreon Clib, you will need the following external dependencies:

- a C++ compilation environment,
- CMake 3, a cross-platform build system.

<!--DOCUSAURUS_CODE_TABS-->

<!--CentOS 7-->

| Software                    | Package name               | Description                                               |
|-----------------------------|----------------------------|-----------------------------------------------------------|
| C++ compilation environment | gcc gcc-c++ make pkgconfig | Mandatory tools to compile                                |
| CMake                       | cmake3                     | Read the build script and prepare sources for compilation |

<!--CentOS 8-->

| Software                    | Package name                        | Description                                               |
|-----------------------------|-------------------------------------|-----------------------------------------------------------|
| C++ compilation environment | gcc gcc-c++ make pkgconf-pkg-config | Mandatory tools to compile                                |
| CMake                       | cmake                               | Read the build script and prepare sources for compilation |

<!--Debian / Raspbian-->

| Software                    | Package name               | Description                                                |
|-----------------------------|----------------------------|------------------------------------------------------------|
| C++ compilation environment | build-essential pkg-config | Mandatory tools to compile.                                |
| CMake                       | cmake                      | Read the build script and prepare sources for compilation. |

<!--openSUSE-->

| Software                    | Package name                | Description                                                |
|-----------------------------|-----------------------------|------------------------------------------------------------|
| C++ compilation environment | gcc gcc-c++ make pkg-config | Mandatory tools to compile.                                |
| CMake                       | cmake                       | Read the build script and prepare sources for compilation. |

<!--END_DOCUSAURUS_CODE_TABS-->

Use the system package manager to install them:

<!--DOCUSAURUS_CODE_TABS-->

<!--CentOS 7-->

```shell
yum install gcc gcc-c++ make cmake3 pkgconfig
```

<!--CentOS 8-->

```shell
dnf install gcc gcc-c++ make cmake pkgconf-pkg-config
```

<!--Debian / Raspbian-->

```shell
apt-get install build-essential cmake pkg-config
```

<!--openSUSE-->

```shell
zypper install gcc gcc-c++ make cmake pkg-config
```

<!--END_DOCUSAURUS_CODE_TABS-->

### Build

#### Get sources

Centreon Clib can be checked out from GitHub at
<https://github.com/centreon/centreon-clib>. On a Linux box with
git installed this is just a matter of:

```shell
git clone -b x.y.z https://github.com/centreon/centreon-clib
```

With x.y.z being the targeted version you want to compile.

Or you can get the latest Centreon Clib's sources from its
[download website](https://download.centreon.com/). Once downloaded,
extract it:

```shell
wget http://files.download.centreon.com/public/centreon-clib/centreon-clib-x.y.z.tar.gz
tar xzf centreon-clib-x.y.z.tar.gz
```

With x.y.z being the downloaded version you want to compile.

#### Configuration

At the root of the project directory, create a build directory, then
place yourself in it:

```shell
mkdir /path_to_centreon_clib/build
cd /path_to_centreon_clib/build
```

Then, launch the *cmake* command that will prepare the compilation
environment.

Recommended command for your distribution:

<!--DOCUSAURUS_CODE_TABS-->

<!--CentOS 7-->

```shell
cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DUSE_CXX11_ABI=Off \
    -DWITH_PREFIX=/usr \
    -DWITH_PREFIX_LIB=/usr/lib \
    -DWITH_TESTING=On \
    -DWITH_PKGCONFIG_DIR=/usr/lib/pkgconfig ..
```

<!--CentOS 8-->

```shell
cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DUSE_CXX11_ABI=On \
    -DWITH_PREFIX=/usr \
    -DWITH_PREFIX_LIB=/usr/lib \
    -DWITH_TESTING=On \
    -DWITH_PKGCONFIG_DIR=/usr/lib/pkgconfig ..
```

<!--Debian / Raspbian-->

```shell
cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DUSE_CXX11_ABI=On \
    -DWITH_PREFIX=/usr \
    -DWITH_PREFIX_LIB=/usr/lib \
    -DWITH_TESTING=On \
    -DWITH_PKGCONFIG_DIR=/usr/lib/pkgconfig ..
```

<!--openSUSE-->

```shell
cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DUSE_CXX11_ABI=On \
    -DWITH_PREFIX=/usr \
    -DWITH_PREFIX_LIB=/usr/lib \
    -DWITH_TESTING=On \
    -DWITH_PKGCONFIG_DIR=/usr/lib/pkgconfig ..
```

<!--END_DOCUSAURUS_CODE_TABS-->

At this step, the software will check for existence and usability of the
prerequisites. If one cannot be found, an appropriate error message will
be printed. Otherwise an installation summary will be printed.

> If you need to change the options you used to compile your software, you
> might want to remove the *CMakeCache.txt* file that is in the *build*
> directory. This will remove cache entries that might have been computed
> during the last configuration step.

Your Centreon Clib can be tweaked to your particular needs using CMake's
variable system. Variables can be set like this:

```shell
cmake -D<variable1>=<value1> [-D<variable2>=<value2>] ..
```

Here's the list of variables available and their description:

| Variable                | Description                                                                                                                       | Default value                  |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| `WITH_COVERAGE`         | Add code coverage on unit tests.                                                                                                  | `Off`                          |
| `WITH_PKGCONFIG_DIR`    | Use to install pkg-config files.                                                                                                  | `${WITH_PREFIX_LIB}/pkgconfig` |
| `WITH_PKGCONFIG_SCRIPT` | Enable or disable install pkg-config files.                                                                                       | `On`                           |
| `WITH_PREFIX`           | Base directory for Centreon Clib installation. If other prefixes are expressed as relative paths, they are relative to this path. | `/usr/local`                   |
| `WITH_PREFIX_INC`       | Define specific directory for Centreon Engine headers.                                                                            | `${WITH_PREFIX}/include`       |
| `WITH_PREFIX_LIB`       | Define specific directory for Centreon Engine modules.                                                                            | `${WITH_PREFIX}/lib`           |
| `WITH_SHARED_LIB`       | Create or not a shared library.                                                                                                   | `On`                           |
| `WITH_STATIC_LIB`       | Create or not a static library.                                                                                                   | `Off`                          |
| `WITH_TESTING`          | Build unit test.                                                                                                                  | `Off`                          |

#### Compilation

Once properly configured, the compilation process is really simple:

```shell
make -jx
```

Where *x* is the number of core on your server, useful to make compilation
run faster.

And wait until compilation completes.

### Install

Once compiled, the following command must be run as privileged user to
finish installation:

```shell
make install
```

And wait for its completion.

## Centreon Engine

### Prerequisites

If you decide to build Centreon Engine from sources, we heavily
recommand that you create dedicated system user and group for security
purposes.

On all systems the commands to create a user and a group both named
**centreon-engine** are as follow (need to run these as root):

```shell
groupadd centreon-engine
useradd -g centreon-engine -m -r -d /var/lib/centreon-engine centreon-engine
```

Please note that these user and group will be used in the next steps. If
you decide to change user and/or group name here, please do so in
further steps too.

To build Centreon Engine, you will need the following external
dependencies:

- a C++ compilation environment,
- CMake 3, a cross-platform build system,
- pip and Conan, package managers for Python and C/C++,
- Centreon Clib, the Centreon core library.

<!--DOCUSAURUS_CODE_TABS-->

<!--CentOS 7-->

| Software                    | Package name               | Description                                               |
|-----------------------------|----------------------------|-----------------------------------------------------------|
| C++ compilation environment | gcc gcc-c++ make pkgconfig | Mandatory tools to compile                                |
| CMake                       | cmake3                     | Read the build script and prepare sources for compilation |
| pip                         | python-pip                 | Package manager for Python                                |
| Conan                       | conan                      | Package manager for C/C++                                 |
| Centreon Clib               | centreon-clib-devel        | Core library used by Centreon                             |

<!--CentOS 8-->

| Software                    | Package name                        | Description                                               |
|-----------------------------|-------------------------------------|-----------------------------------------------------------|
| C++ compilation environment | gcc gcc-c++ make pkgconf-pkg-config | Mandatory tools to compile                                |
| CMake                       | cmake                               | Read the build script and prepare sources for compilation |
| pip                         | python3-pip                         | Package manager for Python                                |
| Conan                       | conan                               | Package manager for C/C++                                 |
| Centreon Clib               | centreon-clib-devel                 | Core library used by Centreon                             |

<!--Debian / Raspbian-->

| Software                    | Package name               | Description                                                |
|-----------------------------|----------------------------|------------------------------------------------------------|
| C++ compilation environment | build-essential pkg-config | Mandatory tools to compile.                                |
| CMake                       | cmake                      | Read the build script and prepare sources for compilation. |
| pip                         | python3-pip                | Package manager for Python                                 |
| Conan                       | conan                      | Package manager for C/C++                                  |
| Centreon Clib               | centreon-clib-devel        | Core library used by Centreon                              |

<!--openSUSE-->

| Software                    | Package name                | Description                                                |
|-----------------------------|-----------------------------|------------------------------------------------------------|
| C++ compilation environment | gcc gcc-c++ make pkg-config | Mandatory tools to compile.                                |
| CMake                       | cmake                       | Read the build script and prepare sources for compilation. |
| pip                         | python3-pip                 | Package manager for Python                                 |
| Conan                       | conan                       | Package manager for C/C++                                  |
| Centreon Clib               | centreon-clib-devel         | Core library used by Centreon                              |

<!--END_DOCUSAURUS_CODE_TABS-->

#### Common packages

Use the system package manager to install them:

<!--DOCUSAURUS_CODE_TABS-->

<!--CentOS 7-->

```shell
yum install gcc gcc-c++ make cmake3 pkgconfig python-pip
```

<!--CentOS 8-->

```shell
dnf install gcc gcc-c++ make cmake pkgconf-pkg-config python3-pip
```

<!--Debian / Raspbian-->

```shell
apt-get install build-essential cmake pkg-config python3-pip
```

<!--openSUSE-->

```shell
zypper install gcc gcc-c++ make cmake pkg-config python3-pip
```

<!--END_DOCUSAURUS_CODE_TABS-->

#### Conan

Install Conan using pip as follow:

```shell
pip3 install conan
```

Then add Centreon's Conan repository:

```shell
conan remote add centreon https://api.bintray.com/conan/centreon/centreon
```

#### Centreon Clib

Follow the [above procedure](#centreon-clib) to install Centreon Clib.

### Build

#### Get sources

Centreon Engine can be checked out from GitHub at
<https://github.com/centreon/centreon-engine>. On a Linux box with
git installed this is just a matter of:

```shell
git clone -b x.y.z https://github.com/centreon/centreon-engine
```

With x.y.z being the targeted version you want to compile.

Or you can get the latest Centreon Engine's sources from its
[download website](https://download.centreon.com/). Once downloaded,
extract it:

```shell
wget http://files.download.centreon.com/public/centreon-engine/centreon-engine-x.y.z.tar.gz
tar xzf centreon-engine-x.y.z.tar.gz
```

With x.y.z being the downloaded version you want to compile.

#### Configuration

At the root of the project directory, create a build directory, then
place yourself in it:

```shell
mkdir /path_to_centreon_engine/build
cd /path_to_centreon_engine/build
```

Install the C++ dependencies using Conan:

<!--DOCUSAURUS_CODE_TABS-->

<!--GCC > 5-->

```shell
conan install --remote centreon --build missing -s compiler.libcxx=libstdc++11 ..
```

<!--GCC < 5-->

```shell
conan install --remote centreon --build missing ..
```

<!--END_DOCUSAURUS_CODE_TABS-->

> All the dependencies pulled by Conan are located in conanfile.txt. If
> you want to use a dependency from your package manager instead of Conan,
> you need to remove it from conanfile.txt.

Finally, launch the *cmake* command that will prepare the compilation
environment.

Recommended command for your distribution:

<!--DOCUSAURUS_CODE_TABS-->

<!--CentOS 7-->

```shell
cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DUSE_CXX11_ABI=Off \
    -DWITH_CREATE_FILES=On \
    -DWITH_GROUP=centreon-engine \
    -DWITH_LOGROTATE_SCRIPT=On \
    -DWITH_PKGCONFIG_SCRIPT=On \
    -DWITH_PKGCONFIG_DIR=/usr/lib/pkgconfig \
    -DWITH_PREFIX=/usr \
    -DWITH_PREFIX_BIN=/usr/sbin \
    -DWITH_PREFIX_CONF=/etc/centreon-engine \
    -DWITH_RW_DIR=/var/lib/centreon-engine/rw \
    -DWITH_SAMPLE_CONFIG=Off \
    -DWITH_STARTUP_DIR=/usr/lib/systemd/system/ \
    -DWITH_STARTUP_SCRIPT=systemd \
    -DWITH_TESTING=On \
    -DWITH_USER=centreon-engine \
    -DWITH_VAR_DIR=/var/log/centreon-engine ..
```

<!--CentOS 8-->

```shell
cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DUSE_CXX11_ABI=On \
    -DWITH_CREATE_FILES=On \
    -DWITH_GROUP=centreon-engine \
    -DWITH_LOGROTATE_SCRIPT=On \
    -DWITH_PKGCONFIG_SCRIPT=On \
    -DWITH_PKGCONFIG_DIR=/usr/lib/pkgconfig \
    -DWITH_PREFIX=/usr \
    -DWITH_PREFIX_BIN=/usr/sbin \
    -DWITH_PREFIX_CONF=/etc/centreon-engine \
    -DWITH_RW_DIR=/var/lib/centreon-engine/rw \
    -DWITH_SAMPLE_CONFIG=Off \
    -DWITH_STARTUP_DIR=/usr/lib/systemd/system/ \
    -DWITH_STARTUP_SCRIPT=systemd \
    -DWITH_TESTING=On \
    -DWITH_USER=centreon-engine \
    -DWITH_VAR_DIR=/var/log/centreon-engine ..
```

<!--Debian / Raspbian-->

```shell
cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DUSE_CXX11_ABI=On \
    -DWITH_CREATE_FILES=On \
    -DWITH_GROUP=centreon-engine \
    -DWITH_LOGROTATE_SCRIPT=On \
    -DWITH_PKGCONFIG_SCRIPT=On \
    -DWITH_PKGCONFIG_DIR=/usr/lib/pkgconfig \
    -DWITH_PREFIX=/usr \
    -DWITH_PREFIX_BIN=/usr/sbin \
    -DWITH_PREFIX_CONF=/etc/centreon-engine \
    -DWITH_RW_DIR=/var/lib/centreon-engine/rw \
    -DWITH_SAMPLE_CONFIG=Off \
    -DWITH_STARTUP_DIR=/usr/lib/systemd/system/ \
    -DWITH_STARTUP_SCRIPT=systemd \
    -DWITH_TESTING=On \
    -DWITH_USER=centreon-engine \
    -DWITH_VAR_DIR=/var/log/centreon-engine ..
```

<!--openSUSE-->

```shell
cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DUSE_CXX11_ABI=On \
    -DWITH_CREATE_FILES=On \
    -DWITH_GROUP=centreon-engine \
    -DWITH_LOGROTATE_SCRIPT=On \
    -DWITH_PKGCONFIG_SCRIPT=On \
    -DWITH_PKGCONFIG_DIR=/usr/lib/pkgconfig \
    -DWITH_PREFIX=/usr \
    -DWITH_PREFIX_BIN=/usr/sbin \
    -DWITH_PREFIX_CONF=/etc/centreon-engine \
    -DWITH_RW_DIR=/var/lib/centreon-engine/rw \
    -DWITH_SAMPLE_CONFIG=Off \
    -DWITH_STARTUP_DIR=/usr/lib/systemd/system/ \
    -DWITH_STARTUP_SCRIPT=systemd \
    -DWITH_TESTING=On \
    -DWITH_USER=centreon-engine \
    -DWITH_VAR_DIR=/var/log/centreon-engine ..
```

<!--END_DOCUSAURUS_CODE_TABS-->

At this step, the software will check for existence and usability of the
prerequisites. If one cannot be found, an appropriate error message will
be printed. Otherwise an installation summary will be printed.

> If you need to change the options you used to compile your software, you
> might want to remove the *CMakeCache.txt* file that is in the *build*
> directory. This will remove cache entries that might have been computed
> during the last configuration step.

Your Centreon Engine can be tweaked to your particular needs using CMake's
variable system. Variables can be set like this:

```shell
cmake -D<variable1>=<value1> [-D<variable2>=<value2>] ..
```

Here's the list of variables available and their description:

| Variable                         | Description                                                                                                                         | Default value                            |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------|
| `WITH_BENCH`                     | Build benchmarking tools.                                                                                                           | `Off`                                    |
| `WITH_CENTREON_CLIB_INCLUDE_DIR` | Set the directory path of centreon-clib include.                                                                                    | auto detection                           |
| `WITH_CENTREON_CLIB_LIBRARIES`   | Set the centreon-clib library to use.                                                                                               | auto detection                           |
| `WITH_CENTREON_CLIB_LIBRARY_DIR` | Set the centreon-clib library directory (don't use it if you use WITH_CENTREON_CLIB_LIBRARIES).                                     | auto detection                           |
| `WITH_COVERAGE`                  | Add code coverage on unit tests.                                                                                                    | `Off`                                    |
| `WITH_CREATE_FILES`              | Create centreon-engine files.                                                                                                       | `On`                                     |
| `WITH_GROUP`                     | Set the group for Centreon Engine installation.                                                                                     | `root`                                   |
| `WITH_LOCK_FILE`                 | Used by the startup script.                                                                                                         | `/var/lock/subsys/centengine.lock`       |
| `WITH_LOG_ARCHIVE_DIR`           | Use to archive log files that have been rotated.                                                                                    | `${WITH_VAR_DIR}/archives`               |
| `WITH_LOGROTATE_DIR`             | Use to install logrotate files.                                                                                                     | `/etc/logrorate.d/`                      |
| `WITH_LOGROTATE_SCRIPT`          | Enable or disable install logrotate files.                                                                                          | `Off`                                    |
| `WITH_PID_FILE`                  | This file contains the process id (PID) number of the running Centreon Engine process.                                              | `/var/run/centengine.pid`                |
| `WITH_PKGCONFIG_DIR`             | Use to install pkg-config files.                                                                                                    | `${WITH_PREFIX_LIB}/pkgconfig`           |
| `WITH_PKGCONFIG_SCRIPT`          | Enable or disable install pkg-config files.                                                                                         | `On`                                     |
| `WITH_PREFIX`                    | Base directory for Centreon Engine installation. If other prefixes are expressed as relative paths, they are relative to this path. | `/usr/local`                             |
| `WITH_PREFIX_BIN`                | Define specific directory for Centreon Engine binary.                                                                               | `${WITH_PREFIX}/bin`                     |
| `WITH_PREFIX_CONF`               | Define specific directory for Centreon Engine configuration.                                                                        | `${WITH_PREFIX}/etc`                     |
| `WITH_PREFIX_INC`                | Define specific directory for Centreon Engine headers.                                                                              | `${WITH_PREFIX}/include/centreon-engine` |
| `WITH_PREFIX_LIB`                | Define specific directory for Centreon Engine modules.                                                                              | `${WITH_PREFIX}/lib/centreon-engine`     |
| `WITH_RW_DIR`                    | Use for files to need read/write access.                                                                                            | `${WITH_VAR_DIR}/rw`                     |
| `WITH_SAMPLE_CONFIG`             | Install sample configuration files.                                                                                                 | `On`                                     |
| `WITH_SHARED_LIB`                | Build shared library for the core library.                                                                                          | `Off`                                    |
| `WITH_STARTUP_DIR`               | Define the startup directory.                                                                                                       | auto detection                           |
| `WITH_STARTUP_SCRIPT`            | Define the startup script.                                                                                                          | auto detection                           |
| `WITH_TESTING`                   | Build unit test.                                                                                                                    | `Off`                                    |
| `WITH_USER`                      | Set the user for Centreon Engine installation.                                                                                      | `root`                                   |
| `WITH_VAR_DIR`                   | Define specific directory for temporary Centreon Engine files.                                                                      | `${WITH_PREFIX}/var`                     |

#### Compilation

Once properly configured, the compilation process is really simple:

```shell
make -jx
```

Where *x* is the number of core on your server, useful to make compilation
run faster.

And wait until compilation completes.

### Install

Once compiled, the following command must be run as privileged user to
finish installation:

```shell
make install
```

And wait for its completion.

### Check-up

After a successful installation, you should check for the existence of
some of the following files.

| File                                          | Description                           |
|-----------------------------------------------|---------------------------------------|
| `${WITH_PREFIX_BIN}/centengine`               | Centreon Engine daemon.               |
| `${WITH_PREFIX_BIN}/centenginestats`          | Centreon Engine statistic.            |
| `${WITH_PREFIX_CONF}/`                        | Centreon Engine sample configuration. |
| `${WITH_PREFIX_LIB}/externalcmd.so`           | External commands module.             |
| `${WITH_STARTUP_DIR}/centengine.conf`         | Startup script for ubuntu.            |
| `${WITH_STARTUP_DIR}/centengine`              | Startup script for other OS.          |
| `${WITH_PREFIX_INC}/include/centreon-engine/` | All devel Centreon Engine's include.  |
| `${WITH_PKGCONFIG_DIR}/centengine.pc`         | Centreon Engine pkg-config file.      |

## Centreon Broker

### Prerequisites

If you decide to build Centreon Broker from sources, we heavily
recommand that you create dedicated system user and group for security
purposes.

On all systems the commands to create a user and a group both named
**centreon-broker** are as follow (need to run these as root):

```shell
groupadd centreon-broker
useradd -g centreon-broker -m -r -d /var/lib/centreon-broker centreon-broker
```

Please note that these user and group will be used in the next steps. If
you decide to change user and/or group name here, please do so in
further steps too.

To build Centreon Broker, you will need the following external
dependencies:

- a C++ compilation environment,
- CMake 3, a cross-platform build system,
- pip and Conan, package managers for Python and C/C++,
- MariaDB **(>= 10.3)** development files (for the SQL module),
- RRDTool development files (for the RRD module),
- Lua development files (for the LUA module),
- GnuTLS **(>= 3.3.29)** development files, a secure communications library,
- Libgcrypt development files, a cryptographic library.

<!--DOCUSAURUS_CODE_TABS-->

<!--CentOS 7-->

| Software                    | Package name                 | Description                                               |
|-----------------------------|------------------------------|-----------------------------------------------------------|
| C++ compilation environment | gcc gcc-c++ make pkgconfig   | Mandatory tools to compile                                |
| CMake                       | cmake3                       | Read the build script and prepare sources for compilation |
| pip                         | python-pip                   | Package manager for Python                                |
| Conan                       | conan                        | Package manager for C/C++                                 |
| MariaDB                     | MariaDB-devel                | MariaDB development files                                 |
| RRDTool                     | rrdtool-devel                | RRDTool development files                                 |
| Lua                         | lua-devel                    | LUA development files                                     |
| GnuTLS                      | gnutls-devel                 | GnuTLS development files                                  |
| Libgcrypt                   | libgcrypt-devel              | Libgcrypt development files                               |

<!--CentOS 8-->

| Software                    | Package name                        | Description                                               |
|-----------------------------|-------------------------------------|-----------------------------------------------------------|
| C++ compilation environment | gcc gcc-c++ make pkgconf-pkg-config | Mandatory tools to compile                                |
| CMake                       | cmake                               | Read the build script and prepare sources for compilation |
| pip                         | python3-pip                         | Package manager for Python                                |
| Conan                       | conan                               | Package manager for C/C++                                 |
| MariaDB                     | MariaDB-devel                       | MariaDB development files                                 |
| RRDTool                     | rrdtool-devel                       | RRDTool development files                                 |
| Lua                         | lua-devel                           | LUA development files                                     |
| GnuTLS                      | gnutls-devel                        | GnuTLS development files                                  |
| Libgcrypt                   | libgcrypt-devel                     | Libgcrypt development files                               |

<!--Debian / Raspbian-->

| Software                    | Package name                            | Description                                                |
|-----------------------------|-----------------------------------------|------------------------------------------------------------|
| C++ compilation environment | build-essential pkg-config              | Mandatory tools to compile.                                |
| CMake                       | cmake                                   | Read the build script and prepare sources for compilation. |
| pip                         | python3-pip                             | Package manager for Python                                 |
| Conan                       | conan                                   | Package manager for C/C++                                  |
| MariaDB                     | libmariadb-dev                          | MariaDB development files                                  |
| RRDTool                     | librrd-dev                              | RRDTool development files                                  |
| Lua                         | liblua5.3-dev (liblua5.2-dev on Jessie) | LUA development files                                      |
| GnuTLS                      | libgnutls28-dev                         | GnuTLS development files                                   |
| Libgcrypt                   | libgcrypt20-dev                         | Libgcrypt development files                                |

<!--openSUSE-->

| Software                    | Package name                 | Description                                                |
|-----------------------------|------------------------------|------------------------------------------------------------|
| C++ compilation environment | gcc gcc-c++ make pkg-config  | Mandatory tools to compile.                                |
| CMake                       | cmake                        | Read the build script and prepare sources for compilation. |
| pip                         | python3-pip                  | Package manager for Python                                 |
| Conan                       | conan                        | Package manager for C/C++                                  |
| MariaDB                     | MariaDB-devel                | MariaDB development files                                  |
| RRDTool                     | rrdtool-devel                | RRDTool development files                                  |
| Lua                         | lua-devel                    | LUA development files                                      |
| GnuTLS                      | libgnutls-devel              | GnuTLS development files                                   |
| Libgcrypt                   | libgcrypt-devel              | Libgcrypt development files                                |

<!--END_DOCUSAURUS_CODE_TABS-->

#### Common packages

Use the system package manager to install them:

<!--DOCUSAURUS_CODE_TABS-->

<!--CentOS 7-->

```shell
yum install gcc gcc-c++ make cmake3 pkgconfig python-pip MariaDB-devel rrdtool-devel lua-devel gnutls-devel libgcrypt-devel
```

<!--CentOS 8-->

```shell
dnf install gcc gcc-c++ make cmake pkgconf-pkg-config python3-pip MariaDB-devel rrdtool-devel lua-devel gnutls-devel libgcrypt-devel
```

<!--Debian / Raspbian-->

```shell
apt-get install build-essential cmake pkg-config python3-pip libmariadb-dev librrd-dev liblua5.3-dev libgnutls28-dev libgcrypt20-dev
```

<!--openSUSE-->

```shell
zypper install gcc gcc-c++ make cmake pkg-config python3-pip MariaDB-devel rrdtool-devel lua-devel libgnutls-devel libgcrypt-devel
```

<!--END_DOCUSAURUS_CODE_TABS-->

#### Conan

Install Conan using pip as follow:

```shell
pip3 install conan
```

Then add Centreon's Conan repository:

```shell
conan remote add centreon https://api.bintray.com/conan/centreon/centreon
```

### Build

#### Get sources

Centreon Broker can be checked out from GitHub at
<https://github.com/centreon/centreon-broker>. On a Linux box with
git installed this is just a matter of:

```shell
git clone -b x.y.z https://github.com/centreon/centreon-broker
```

With x.y.z being the targeted version you want to compile.

Or you can get the latest Centreon Broker's sources from its
[download website](https://download.centreon.com/). Once downloaded,
extract it:

```shell
wget http://files.download.centreon.com/public/centreon-broker/centreon-broker-x.y.z.tar.gz
tar xzf centreon-broker-x.y.z.tar.gz
```

With x.y.z being the downloaded version you want to compile.

#### Configuration

At the root of the project directory, create a build directory, then
place yourself in it:

```shell
mkdir /path_to_centreon_broker/build
cd /path_to_centreon_broker/build
```

Install the C++ dependencies using Conan:

<!--DOCUSAURUS_CODE_TABS-->

<!--GCC > 5-->

```shell
conan install --remote centreon --build missing -s compiler.libcxx=libstdc++11 ..
```

<!--GCC < 5-->

```shell
conan install --remote centreon --build missing ..
```

<!--END_DOCUSAURUS_CODE_TABS-->

> All the dependencies pulled by Conan are located in conanfile.txt. If
> you want to use a dependency from your package manager instead of Conan,
> you need to remove it from conanfile.txt.

Finally, launch the *cmake* command that will prepare the compilation
environment.

Recommended command for your distribution:

<!--DOCUSAURUS_CODE_TABS-->

<!--CentOS 7-->

```shell
cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DUSE_CXX11_ABI=Off \
    -DWITH_CONFIG_FILES=On \
    -DWITH_DAEMONS='central-rrd;central-broker' \
    -DWITH_GROUP=centreon-broker \
    -DWITH_PREFIX=/usr \
    -DWITH_PREFIX_BIN=/usr/sbin \
    -DWITH_PREFIX_CONF=/etc/centreon-broker \
    -DWITH_PREFIX_MODULES=/usr/share/centreon/lib/centreon-broker \
    -DWITH_PREFIX_LIB=/usr/lib/centreon-broker \
    -DWITH_PREFIX_VAR=/var/lib/centreon-broker \
    -DWITH_STARTUP_DIR=/usr/lib/systemd/system/ \
    -DWITH_STARTUP_SCRIPT=systemd \
    -DWITH_TESTING=On \
    -DWITH_USER=centreon-broker ..
```

<!--CentOS 8-->

```shell
cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DUSE_CXX11_ABI=On \
    -DWITH_CONFIG_FILES=On \
    -DWITH_DAEMONS='central-rrd;central-broker' \
    -DWITH_GROUP=centreon-broker \
    -DWITH_PREFIX=/usr \
    -DWITH_PREFIX_BIN=/usr/sbin \
    -DWITH_PREFIX_CONF=/etc/centreon-broker \
    -DWITH_PREFIX_MODULES=/usr/share/centreon/lib/centreon-broker \
    -DWITH_PREFIX_LIB=/usr/lib/centreon-broker \
    -DWITH_PREFIX_VAR=/var/lib/centreon-broker \
    -DWITH_STARTUP_DIR=/usr/lib/systemd/system/ \
    -DWITH_STARTUP_SCRIPT=systemd \
    -DWITH_TESTING=On \
    -DWITH_USER=centreon-broker ..
```

<!--Debian / Raspbian-->

```shell
cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DUSE_CXX11_ABI=On \
    -DWITH_CONFIG_FILES=On \
    -DWITH_DAEMONS='central-rrd;central-broker' \
    -DWITH_GROUP=centreon-broker \
    -DWITH_PREFIX=/usr \
    -DWITH_PREFIX_BIN=/usr/sbin \
    -DWITH_PREFIX_CONF=/etc/centreon-broker \
    -DWITH_PREFIX_MODULES=/usr/share/centreon/lib/centreon-broker \
    -DWITH_PREFIX_LIB=/usr/lib/centreon-broker \
    -DWITH_PREFIX_VAR=/var/lib/centreon-broker \
    -DWITH_STARTUP_DIR=/usr/lib/systemd/system/ \
    -DWITH_STARTUP_SCRIPT=systemd \
    -DWITH_TESTING=On \
    -DWITH_USER=centreon-broker ..
```

<!--openSUSE-->

```shell
cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DUSE_CXX11_ABI=On \
    -DWITH_CONFIG_FILES=On \
    -DWITH_DAEMONS='central-rrd;central-broker' \
    -DWITH_GROUP=centreon-broker \
    -DWITH_PREFIX=/usr \
    -DWITH_PREFIX_BIN=/usr/sbin \
    -DWITH_PREFIX_CONF=/etc/centreon-broker \
    -DWITH_PREFIX_MODULES=/usr/share/centreon/lib/centreon-broker \
    -DWITH_PREFIX_LIB=/usr/lib/centreon-broker \
    -DWITH_PREFIX_VAR=/var/lib/centreon-broker \
    -DWITH_STARTUP_DIR=/usr/lib/systemd/system/ \
    -DWITH_STARTUP_SCRIPT=systemd \
    -DWITH_TESTING=On \
    -DWITH_USER=centreon-broker ..
```

<!--END_DOCUSAURUS_CODE_TABS-->

At this step, the software will check for existence and usability of the
prerequisites. If one cannot be found, an appropriate error message will
be printed. Otherwise an installation summary will be printed.

> If you need to change the options you used to compile your software, you
> might want to remove the *CMakeCache.txt* file that is in the *build*
> directory. This will remove cache entries that might have been computed
> during the last configuration step.

Your Centreon Engine can be tweaked to your particular needs using CMake's
variable system. Variables can be set like this:

```shell
cmake -D<variable1>=<value1> [-D<variable2>=<value2>] ..
```

Here's the list of variables available and their description:

| Variable                                 | Description                                                                                                                         | Default value                            |
|------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------|
| `WITH_ASAN`                              | Add the libasan to check memory leaks and other memory issues. This is an option for developers.                                    | `Off`                                    |
| `WITH_CBWD`                              | Build Centreon Broker watchdog.                                                                                                     | `On`                                     |
| `WITH_CONFIG_FILES`                      | Create Centreon Broker configuration files.                                                                                         | `Off`                                    |
| `WITH_COVERAGE`                          | Add code coverage on unit tests.                                                                                                    | `Off`                                    |
| `WITH_DAEMONS`                           | Set a list of Broker instances to be launched by watchdog.                                                                          | none                                     |
| `WITH_DEBUG_ASIO`                        | Add the Asio debugging flags. This is an option for developers.                                                                     | `Off`                                    |
| `WITH_DEBUG_CONFIG`                      | Enables checks on configuration. This is an option for developers.                                                                  | `Off`                                    |
| `WITH_GROUP`                             | Set the group for Centreon Broker installation.                                                                                     | `root`                                   |
| `WITH_MODULE_BAM`                        | Build BAM module.                                                                                                                   | `On`                                     |
| `WITH_MODULE_CORRELATION`                | Build correlation module.                                                                                                           | `On`                                     |
| `WITH_MODULE_INFLUXDB`                   | Build InfluxDB module.                                                                                                              | `On`                                     |
| `WITH_MODULE_GRAPHITE`                   | Build Graphite module.                                                                                                              | `On`                                     |
| `WITH_MODULE_LUA`                        | Build Lua module.                                                                                                                   | `On`                                     |
| `WITH_MODULE_NEB`                        | Build NEB module.                                                                                                                   | `On`                                     |
| `WITH_MODULE_NOTIFICATION`               | Build notification module.                                                                                                          | `Off`                                    |
| `WITH_MODULE_RRD`                        | Build RRD module.                                                                                                                   | `On`                                     |
| `WITH_MODULE_SIMU`                       | Add a module only used for tests to see data that cbmod should receive.                                                             | `Off`                                    |
| `WITH_MODULE_SQL`                        | Build SQL module.                                                                                                                   | `On`                                     |
| `WITH_MODULE_STATS`                      | Build stats module.                                                                                                                 | `On`                                     |
| `WITH_MODULE_STORAGE`                    | Build storage module.                                                                                                               | `On`                                     |
| `WITH_MODULE_TCP`                        | Build TCP module.                                                                                                                   | `On`                                     |
| `WITH_MODULE_TLS`                        | Build TLS module.                                                                                                                   | `On`                                     |
| `WITH_MONITORING_ENGINE`                 | This is an option for developers.                                                                                                   | `Off`                                    |
| `WITH_MONITORING_ENGINE_INTERVAL_LENGTH` | This is an option for developers.                                                                                                   | `1`                                      |
| `WITH_MONITORING_ENGINE_MODULES`         | This is an option for developers.                                                                                                   |  none                                    |
| `WITH_PREFIX`                            | Base directory for Centreon Broker installation. If other prefixes are expressed as relative paths, they are relative to this path. | `/usr/local`                             |
| `WITH_PREFIX_BIN`                        | Path in which binaries will be installed.                                                                                           | `${WITH_PREFIX}/bin`                     |
| `WITH_PREFIX_CONF`                       | Define specific directory for Centreon Engine configuration.                                                                        | `${WITH_PREFIX}/etc`                     |
| `WITH_PREFIX_INC`                        | Define specific directory for Centreon Broker headers.                                                                              | `${WITH_PREFIX}/include/centreon-broker` |
| `WITH_PREFIX_LIB`                        | Where shared objects (like cbmod.so) will be installed.                                                                             | `${WITH_PREFIX}/lib`                     |
| `WITH_PREFIX_MODULES`                    | Where Centreon Broker modules will be installed.                                                                                    | `${WITH_PREFIX_LIB}/centreon-broker`     |
| `WITH_PREFIX_VAR`                        | Centreon Broker runtime directory.                                                                                                  | `${WITH_PREFIX}/var`                     |
| `WITH_STARTUP_DIR`                       | Define the startup directory.                                                                                                       | auto detection                           |
| `WITH_STARTUP_SCRIPT`                    | Generate and install startup script.                                                                                                | auto detection                           |
| `WITH_TESTING`                           | Enable build of unit tests. Disabled by default.                                                                                    | `Off`                                    |
| `WITH_USER`                              | Set the user for Centreon Broker installation.                                                                                      | `root`                                   |

#### Compilation

Once properly configured, the compilation process is really simple:

```shell
make -jx
```

Where *x* is the number of core on your server, useful to make compilation
run faster.

And wait until compilation completes.

### Install

Once compiled, the following command must be run as privileged user to
finish installation:

```shell
make install
```

And wait for its completion.

### Check-up

After a successful installation, you should check for the existence of
some of the following files.

| File                                       | Description                 |
|--------------------------------------------|-----------------------------|
| `${WITH_PREFIX_BIN}/cbd`                   | Centreon Broker daemon.     |
| `${WITH_PREFIX_BIN}/cbwd`                  | Centreon Broker watchdog.   |
| `${WITH_PREFIX_LIB}/cbmod.so`              | Centreon Broker NEB module. |
| `${WITH_PREFIX_MODULES}/10-neb.so`         | NEB module.                 |
| `${WITH_PREFIX_MODULES}/15-stats.so`       | Statistics module.          |
| `${WITH_PREFIX_MODULES}/20-bam.so`         | Centreon Broker BAM module. |
| `${WITH_PREFIX_MODULES}/20-storage.so`     | Storage module.             |
| `${WITH_PREFIX_MODULES}/30-correlation.so` | Correlation module.         |
| `${WITH_PREFIX_MODULES}/50-tcp.so`         | TCP module.                 |
| `${WITH_PREFIX_MODULES}/60-tls.so`         | TLS (encryption) module.    |
| `${WITH_PREFIX_MODULES}/70-graphite.so`    | Graphite module.            |
| `${WITH_PREFIX_MODULES}/70-influxdb.so`    | InfluxDB module.            |
| `${WITH_PREFIX_MODULES}/70-lua.so`         | Lua module.                 |
| `${WITH_PREFIX_MODULES}/70-rrd.so`         | RRD module.                 |
| `${WITH_PREFIX_MODULES}/80-sql.so`         | SQL module.                 |
