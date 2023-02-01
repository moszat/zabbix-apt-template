# zabbix-apt-template
Zabbix template for APT package management

Works for Zabbix 3.x (according to zabbix documentation, tested on version 6.0)

# Requirements
Linux distribution with apt package management

# Installation on Zabbix client

    # for Zabbix agent
    cp apt.conf /etc/zabbix/zabbix_agentd.d/
    systemctl restart zabbix-agent

    # for Zabbix agent2
    cp apt.conf /etc/zabbix/zabbix_agent2.d/plugins.d/
    systemctl restart zabbix-agent2

Finally import template_app_zabbix.xml and attach it to your host
