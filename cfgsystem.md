The **cfgsystem** transAPI module is located inside the `transAPI/cfgsystem/` directory in the Netopeer GIT repository.

  * **Note**: This module accesses and modifies system configuration files. Use it on your own risk. It can harm your system configuration files.

# cfgsystem.so #

TransAPI module primarily intended for the Netopeer server, but in general for libnetconf based NETCONF servers. The module should correctly work on RHEL, SUSE and Debian based distros.

## Installation ##

```
$ ./configure
$ make
# make install
```

### configure options ###

`--with-netopeer-confdir=DIR`

> If you changed the sysconfdir of the Netopeer server, please reflect this change in this option. By default, the Netopeer server stores its configuration files in ${sysconfdir}/netopeer/ directory. Instead of the complete path, you can alternatively specify the sysconfdir value (`--sysconfdir=DIR`) in the same way as in case of the Netopeer server.

`--with-[useradd|userdel|shutdown]=PATH`

> On some distros (RHEL based) when a normal user builds (run configure) it is unable to detect, that these tools are available when you are root. Therefore, if configure cannot detect those tools, you can specify them manually. Otherwise, configure switches off the functionality of the cfgsystem that depends on that tools.

## Functionality ##

This module implements the **ietf-system** data model as defined in <a href='http://tools.ietf.org/html/rfc7317'>RFC 7317</a>. It implements only the folowing features of the data model:

  * **local-users** - configuration of local user authentication by manipulating with the `/etc/passwd`, `/etc/shadow` and `~/.ssh/authorized_keys` files.
  * **ntp** - configuration of usage NTP server(s) by manipulating with `/etc/ntp.conf` file and the ntp (or ntpd on RHEL based distros) service.
  * **timezone-name** - allows timezone configuration using TZ database.

The module also allows configuration of:

  * **DNS resolver** by manipulating with the `/etc/resolv.conf`.

For the detailed description of content for the specific configuration element, please read the <a href='http://tools.ietf.org/html/rfc7317'>RFC 7317</a>.

# cfgsystem-init #

This small utility is able to load all the configurations managed by the ietf-system model and store them as the startup configuration data for use in the **netopeer-server(8)**.

The tool is automatically used with `make install` when the Netopeer server (**netopeer-manager(1)**) is already installed, so after that, you don't need to run it manually.

Remember, that having empty startup datastore on **netopeer-server(8)** startup with the cfgsystem module enabled causes removing all current configuration settings (NTP and DNS servers, users,...)!

## Usage ##

```
# ./cfgsystem-init <cfgsystem's datastore path>
```