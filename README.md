# Advanced Packaging Tool (APT) by Zabbix agent
## Overview
Zabbix template for APT package management

For Zabbix version: 3.0 and higher. (according to Zabbix documentation, tested on version 6.0) This template is developed to monitor APT database status by Zabbix.

## Requirements
Linux distribution with apt package management

## Setup on Zabbix client

For Zabbix agent

    cp apt.conf /etc/zabbix/zabbix_agentd.d/
    systemctl restart zabbix-agent

For Zabbix agent2

    cp apt.conf /etc/zabbix/zabbix_agent2.d/plugins.d/
    systemctl restart zabbix-agent2

Maybe on systems with slow I/O the Updates/Security updates items can return with timeout error. If the default 3 second is not enough, increase the value of Timeout in the configuration of Zabbix agent:

    ### Option: Timeout
    #	Spend no more than Timeout seconds on processing
    #
    # Mandatory: no
    # Range: 1-30
    # Default:
    # Timeout=3

## Setup on Zabbix server
Import template_apt_zabbix.xml and attach it to your host

## Items collected
|Group|Name|Description|Type|Key and additional info|
|---|---|---|---|---|
|Templates/APT DEB package management|Last update|Date of last refresh of APT database|ZABBIX_PASSIVE|vfs.file.time[/var/cache/apt/pkgcache.bin]|
|Templates/APT DEB package management|Reboot|If value is 1, updates to need a restart|ZABBIX_PASSIVE|vfs.file.exists[/var/run/reboot-required]|
|Templates/APT DEB package management|Updates|Number of  updates|ZABBIX_PASSIVE|apt.updates|
|Templates/APT DEB package management|Security updates|Number of security updates|ZABBIX_PASSIVE|apt.security|

## Triggers
|Name|Description|Expression|Severity|Dependencies and additional info|
|---|---|---|---|---|
|There was no update for more than 1 day|APT's database is older than 1 day|nodata(/Template APT DEB package management/vfs.file.time[/var/cache/apt/pkgcache.bin],3660)=0 and fuzzytime(/Template APT DEB package management/vfs.file.time[/var/cache/apt/pkgcache.bin],86400s)=0|WARNING||
|Reboot needed|/var/run/reboot-required is exsist|nodata(/Template APT DEB package management/vfs.file.exists[/var/run/reboot-required],3660)=0 and last(/Template APT DEB package management/vfs.file.exists[/var/run/reboot-required])=1|WARNING||
|There are {ITEM.LASTVALUE1} updates|Updates more than 0|nodata(/Template APT DEB package management/apt.updates,3660)=0 and last(/Template APT DEB package management/apt.updates)>0|INFO||
|There are {ITEM.LASTVALUE1} security updates|Security updates more than 0|nodata(/Template APT DEB package management/apt.security,3660)=0 and last(/Template APT DEB package management/apt.security)>0|WARNING||

