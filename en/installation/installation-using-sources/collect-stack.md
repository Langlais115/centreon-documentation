---
id: collect-stack
title: Collect Stack
---

## Centreon Clib

### Prerequisites

To build Centreon Clib, you will need the following external dependencies:

- a C++ compilation environment,
- CMake, a cross-platform build system.

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

<!--Debian / Ubuntu / Raspbian-->

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

<!--Debian / Ubuntu / Raspbian-->

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

Your Centreon Clib can be tweaked to your particular needs using CMake's
variable system. Variables can be set like this:

```shell
cmake -D<variable1>=<value1> [-D<variable2>=<value2>] ..
```

Here's the list of variables available and their description:

| Variable                | Description                                                                                                                       | Default value                  |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| `CMAKE_BUILD_TYPE`      | Specifie the build type depending on wanted optimizations.                                                                        | `Debug`                        |
| `USE_CXX11_ABI`         | Define whether the declarations in the library headers use the old or new ABI. To be enabled if GCC version > 5.                  | `Off`                          |
| `WITH_COVERAGE`         | Add code coverage on unit tests.                                                                                                  | `Off`                          |
| `WITH_PKGCONFIG_DIR`    | Use to install pkg-config files.                                                                                                  | `${WITH_PREFIX_LIB}/pkgconfig` |
| `WITH_PKGCONFIG_SCRIPT` | Enable or disable install pkg-config files.                                                                                       | `On`                           |
| `WITH_PREFIX`           | Base directory for Centreon Clib installation. If other prefixes are expressed as relative paths, they are relative to this path. | `/usr/local`                   |
| `WITH_PREFIX_INC`       | Define specific directory for Centreon Engine headers.                                                                            | `${WITH_PREFIX}/include`       |
| `WITH_PREFIX_LIB`       | Define specific directory for Centreon Engine modules.                                                                            | `${WITH_PREFIX}/lib`           |
| `WITH_SHARED_LIB`       | Create or not a shared library.                                                                                                   | `On`                           |
| `WITH_STATIC_LIB`       | Create or not a static library.                                                                                                   | `Off`                          |
| `WITH_TESTING`          | Build unit test.                                                                                                                  | `Off`                          |

Recommended command:

```shell
cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DUSE_CXX11_ABI=On \
    -DWITH_PREFIX=/usr \
    -DWITH_PREFIX_LIB=/usr/lib \
    -DWITH_TESTING=On \
    -DWITH_PKGCONFIG_DIR=/usr/lib/pkgconfig ..
```

At this step, the software will check for existence and usability of the
prerequisites. If one cannot be found, an appropriate error message will
be printed. Otherwise an installation summary will be printed.

> If you need to change the options you used to compile your software, you
> might want to remove the *CMakeCache.txt* file that is in the *build*
> directory. This will remove cache entries that might have been computed
> during the last configuration step.

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
- CMake  a cross-platform build system.
- Centreon Clib, the Centreon core library.

<!--DOCUSAURUS_CODE_TABS-->

<!--CentOS 7-->

| Software                    | Package name               | Description                                               |
|-----------------------------|----------------------------|-----------------------------------------------------------|
| C++ compilation environment | gcc gcc-c++ make pkgconfig | Mandatory tools to compile                                |
| CMake                       | cmake3                     | Read the build script and prepare sources for compilation |
| Centreon Clib               | centreon-clib-devel        | Core library used by Centreon                             |

<!--CentOS 8-->

| Software                    | Package name                        | Description                                               |
|-----------------------------|-------------------------------------|-----------------------------------------------------------|
| C++ compilation environment | gcc gcc-c++ make pkgconf-pkg-config | Mandatory tools to compile                                |
| CMake                       | cmake                               | Read the build script and prepare sources for compilation |
| Centreon Clib               | centreon-clib-devel                 | Core library used by Centreon                             |

<!--Debian / Ubuntu / Raspbian-->

| Software                    | Package name               | Description                                                |
|-----------------------------|----------------------------|------------------------------------------------------------|
| C++ compilation environment | build-essential pkg-config | Mandatory tools to compile.                                |
| CMake                       | cmake                      | Read the build script and prepare sources for compilation. |
| Centreon Clib               | centreon-clib-devel        | Core library used by Centreon                              |

<!--openSUSE-->

| Software                    | Package name                | Description                                                |
|-----------------------------|-----------------------------|------------------------------------------------------------|
| C++ compilation environment | gcc gcc-c++ make pkg-config | Mandatory tools to compile.                                |
| CMake                       | cmake                       | Read the build script and prepare sources for compilation. |
| Centreon Clib               | centreon-clib-devel         | Core library used by Centreon                              |

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

<!--Debian / Ubuntu / Raspbian-->

```shell
apt-get install build-essential cmake pkg-config
```

<!--openSUSE-->

```shell
zypper install gcc gcc-c++ make cmake pkg-config
```

<!--END_DOCUSAURUS_CODE_TABS-->

And follow the [above procedure](#centreon-clib) to install Centreon Clib.

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

Your Centreon Engine can be tweaked to your particular needs using CMake's
variable system. Variables can be set like this:

```shell
cmake -D<variable1>=<value1> [-D<variable2>=<value2>] ..
```

Here's the list of variables available and their description:

| Variable                         | Description                                                                                                                         | Default value                            |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------|
| `CMAKE_BUILD_TYPE`               | Specifie the build type depending on wanted optimizations.                                                                          | `Debug`                                  |
| `USE_CXX11_ABI`                  | Define whether the declarations in the library headers use the old or new ABI. To be enabled if GCC version > 5.                    | `Off`                                    |
| `WITH_BENCH`                     | Build benchmarking tools.                                                                                                           | `Off`                                    |
| `WITH_CENTREON_CLIB_INCLUDE_DIR` | Set the directory path of centreon-clib include.                                                                                    | auto detection                           |
| `WITH_CENTREON_CLIB_LIBRARIES`   | Set the centreon-clib library to use.                                                                                               | auto detection                           |
| `WITH_CENTREON_CLIB_LIBRARY_DIR` | Set the centreon-clib library directory (don't use it if you use WITH_CENTREON_CLIB_LIBRARIES).                                     | auto detection                           |
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

Recommended command:

```shell
cmake \
    -DCMAKE_BUILD_TYPE=Release \
    -DWITH_CREATE_FILES=On \
    -DWITH_LOGROTATE_SCRIPT=On \
    -DWITH_GROUP=centreon-engine \
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

At this step, the software will check for existence and usability of the
rerequisites. If one cannot be found, an appropriate error message will
be printed. Otherwise an installation summary will be printed.

> If you need to change the options you used to compile your software, you
> might want to remove the *CMakeCache.txt* file that is in the *build*
> directory. This will remove cache entries that might have been computed
> during the last configuration step.

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
