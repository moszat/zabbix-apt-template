# Advanced Packaging Tool (APT) by Zabbix agent
##Overview
Zabbix template for APT package management

For Zabbix version: 3.0 and higher. (according to Zabbix documentation, tested on version 6.0) This template is developed to monitor APT by Zabbix that works without any external scripts.
The template use the apt-check python tool provided by update-notifier-common package.

## Requirements
Linux distribution with apt package management

##Setup on Zabbix client

    # for Zabbix agent
    cp apt.conf /etc/zabbix/zabbix_agentd.d/
    systemctl restart zabbix-agent

    # for Zabbix agent2
    cp apt.conf /etc/zabbix/zabbix_agent2.d/plugins.d/
    systemctl restart zabbix-agent2

##Setup on Zabbix server
Import template_apt_zabbix.xml and attach it to your host

## Items collected
|Group|Name|Description|Type|Key and additional info|
|---|---|---|---|---|
|Templates/APT DEB package management|Last update|Date of last refresh of APT database|ZABBIX_PASSIVE|vfs.file.time[/var/cache/apt/pkgcache.bin]|
|Templates/APT DEB package management|Reboot|If value is 1, updates to need a restart|ZABBIX_PASSIVE|vfs.file.exists[/var/run/reboot-required]|
|Templates/APT DEB package management|Updates|There are updates|ZABBIX_PASSIVE|apt.updates|
|Templates/APT DEB package management|Security updates|There are security updates|ZABBIX_PASSIVE|apt.security|