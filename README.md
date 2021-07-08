# pfParse

A python command line script to parse pfSense XML configs and output to an Excel file. Supports the following features:

* Output to xlsx format
* Parsing of rules, alias, interfaces, general config settings, snmpd  
* Guessing of pfSense version based of config version
* Checking of pfSense security advisories
* Basic vulnerability checks against pfSense security advisories

## Dependancies
You probably need the following python dependancies

* tabulate
* BeautifulSoup
* xlsxwriter

## How to use!
--version is optional as the pfSense XML config version number does not directly translate to the pfSense release version. Further to this the CONFIG_VER parameter may need updating against https://doc.pfsense.org/index.php/Versions_of_pfSense_and_FreeBSD to make sure the script is comparing against the latest data.
```sh
$ python pfParser.py --version=2.4 examples/config_1.xml
```

## Example output

```
 _______  _______  _______  _______  ______    _______  _______  ______
|       ||       ||       ||   _   ||    _ |  |       ||       ||    _ |
|    _  ||    ___||    _  ||  |_|  ||   | ||  |  _____||    ___||   | ||
|   |_| ||   |___ |   |_| ||       ||   |_||_ | |_____ |   |___ |   |_||_
|    ___||    ___||    ___||       ||    __  ||_____  ||    ___||    __  |
|   |    |   |    |   |    |   _   ||   |  | | _____| ||   |___ |   |  | |
|___|    |___|    |___|    |__| |__||___|  |_||_______||_______||___|  |_|
Parses pfSense XML configuration files and outlines any vulnerbilities
[+] Parsing --version=2.4

[+] PFSENSE VERSION INFORMATION
[+] pfSense version: 2.4
[+] 2.4/RELENG_2_4_0 (FreeBSD 11.1-RELEASE-p1) - Previous release [12/10/2017]
        [+] Security Advisories
                pfSense-SA-17_10.webgui - 2017-12-04 00:00:00 (https://www.pfsense.org/security/advisories/pfSense-SA-17_10.webgui.asc)
                pfSense-SA-17_11.webgui - 2017-12-04 00:00:00 (https://www.pfsense.org/security/advisories/pfSense-SA-17_11.webgui.asc)
                pfSense-SA-17_09.webgui - 2017-11-14 00:00:00 (https://www.pfsense.org/security/advisories/pfSense-SA-17_09.webgui.asc)
                pfSense-SA-17_08.webgui - 2017-11-14 00:00:00 (https://www.pfsense.org/security/advisories/pfSense-SA-17_08.webgui.asc)

[+] HOST INFORMATION
[+] Hostname:                   pfSense
[+] Domain:                     localdomain

[+] WEBCONFIGURATOR
[+] GUI Protocol:               HTTP
[+] GUI Autocomplete:           TRUE
[+] NTPD servers:               None
[+] Serial console:             None
[+] HTTP_REFERER enforcement:   TRUE
[+] GUI lockout rule:           TRUE

[+] SSH SETTINGS
[+] SSH Enabled:                ENABLED
[+] SSH Port:                   22

[+] GROUPS:
    [+] GROUP
        NAME: all
        DESCRIPTION: All Users
        SCOPE: system
        GID: 1998
        MEMBER: 0
    [+] GROUP
        NAME: admins
        DESCRIPTION: System Administrators
        SCOPE: system
        GID: 1999
        MEMBER: 0
        PRIV: page-all

[+] USERS:
    [+] USER
        NAME: admin
        DESCR: System Administrator
        SCOPE: system
        GROUPNAME: admins
        PASSWORD: $1$gkygk87c$lfFXeLHzJ/c2qFjBV9./Z.
        BCRYPT-HASH: $2b$10$13u6qwCOwODv34GyCMgdWub6oQF3RX0rG7c3d3X4JvzuEmAXLYDd2
        UID: 0
        PRIV: user-shell-access

[+] SNMPD INFORMATION
[+] Sys location:                       None
[+] Sys contact:                        None
[+] Community string:                   public

[+] CRON JOB INFORMATION
m     h    dom    mon    dow    user    command
----  ---  -----  -----  -----  ------  --------------------------------------------------------------------------------
1,31  0-5  *      *      *      root    /usr/bin/nice -n20 adjkerntz -a
1     3    1      *      *      root    /usr/bin/nice -n20 /etc/rc.update_bogons.sh
*/60  *    *      *      *      root    /usr/bin/nice -n20 /usr/local/sbin/expiretable -v -t 3600 sshlockout
*/60  *    *      *      *      root    /usr/bin/nice -n20 /usr/local/sbin/expiretable -v -t 3600 webConfiguratorlockout
1     1    *      *      *      root    /usr/bin/nice -n20 /etc/rc.dyndns.update
*/60  *    *      *      *      root    /usr/bin/nice -n20 /usr/local/sbin/expiretable -v -t 3600 virusprot
30    12   *      *      *      root    /usr/bin/nice -n20 /etc/rc.update_urltables
1     0    *      *      *      root    /usr/bin/nice -n20 /etc/rc.update_pkg_metadata

[+] INTERFACE INFORMATION
    [+] NAME: wan
        INTERFACE: em0
        IPADDRESS: dhcp
        SUBNET: None
    [+] NAME: lan
        INTERFACE: em1
        IPADDRESS: 192.168.1.1
        SUBNET: 24

[+] ALIASES
+--------+-----------+---------------+
| Name   | Address   | Description   |
+========+===========+===============+
+--------+-----------+---------------+

[+] FIREWALL RULES - lan
+------------------------------------+--------+------------+---------------+------------+---------------+------------+
| Description                        | Type   | Protocol   | Source        | Src Port   | Destination   | Dst Port   |
+====================================+========+============+===============+============+===============+============+
| Default allow LAN to any rule      | PASS   | *          | lan (network) | *          | *             | *          |
+------------------------------------+--------+------------+---------------+------------+---------------+------------+
| Default allow LAN IPv6 to any rule | PASS   | *          | lan (network) | *          | *             | *          |
+------------------------------------+--------+------------+---------------+------------+---------------+------------+

[+] VULNERBILITIES - 10 found
+-------------------------------------------+------------------+
| NAME                                      | RISK             |
+===========================================+==================+
| Security advisory pfSense-SA-17_10.webgui | LOW/MEDIUM/HIGH? |
+-------------------------------------------+------------------+
| Security advisory pfSense-SA-17_11.webgui | LOW/MEDIUM/HIGH? |
+-------------------------------------------+------------------+
| Security advisory pfSense-SA-17_09.webgui | LOW/MEDIUM/HIGH? |
+-------------------------------------------+------------------+
| Security advisory pfSense-SA-17_08.webgui | LOW/MEDIUM/HIGH? |
+-------------------------------------------+------------------+
| MD5 passwd hash in use for user "admin"   | LOW              |
+-------------------------------------------+------------------+
| Clear text protocol for WebGUI            | HIGH             |
+-------------------------------------------+------------------+
| WebGUI allows Autocomplete                | LOW              |
+-------------------------------------------+------------------+
| No Central Time Server Configured         | LOW              |
+-------------------------------------------+------------------+
| Default SNMP Community String in Use      | LOW              |
+-------------------------------------------+------------------+
| Firewall rule allows to any destination   | MEDIUM           |
+-------------------------------------------+------------------+
```
