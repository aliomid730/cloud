Protect against SYN Flood
==========================
1. net.ipv4.tcp_syncookies = 1
2. net.ipv4.conf.all.rp_filter = 1
3. net.ipv4.tcp_max_syn_backlog = 1024 

Protect Against Slow Http Request (SlowLoris)
=============================================
1. when using HAProxy set the following.
timeout http-request 5s


1. Install and Configure pound.
	a. apt-get install pound
	
Pound HTTP(S) Load Balancer config
==================================
ListenHTTPS
Address 1.2.3.4

	Port	443

	Cert    "/etc/pound/PATHTOCERT.pem"

	## allow PUT and DELETE also (by default only GET, POST and HEAD)?:

	xHTTP		2

	Service

		BackEnd

			Address	127.0.0.1

			Port	81

		End

	End

End


Number of active connections
	netstat -n | grep :80 |wc –l

Shows logged in IPs
netstat -anp |grep 'tcp\|udp' | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort –n

au

[http-get-dos]

enabled = true

port = http,https

filter = http-get-dos

logpath = /var/log/apache2/access.log

maxretry = 300

findtime = 300

#ban for 5 minutes

bantime = 600

action = iptables[name=HTTP, port=http, protocol=tcp]



# Fail2Ban configuration file
#
# Author: Sayyed Ali Agha Hashimi
#
[Definition]

# Option: failregex
# Note: This regex will match any GET entry in your logs, so basically all valid and not valid entries are a match.
# You should set up in the jail.conf file, the maxretry and findtime carefully in order to avoid false positives.

failregex = ^ -.*GET

# Option: ignoreregex
# Notes.: regex to ignore. If this regex matches, the line is ignored.
# Values: TEXT
#
ignoreregex =



ListenHTTPS

     Address 192.168.1.5

     Port    443

     Cert    "/etc/apache2/ssl/mycertificate.pem"

     Service

           BackEnd

                  Address 192.168.1.80

                  Port 80

           End

           BackEnd


                  Address 192.168.1.81

                  Port 80

           End

     End

End


must be used under service
==========================
#HeadRequire "Host:.*myotherdomain.com.*"
#URL "/(images|js|css)/"




	<Proxy balancer://clusterABCD>

		BalancerMember http://serverA

		BalancerMember http://serverB

		BalancerMember http://serverC

		BalancerMember http://serverD

		Order allow,deny

		Allow from all

	</Proxy>

	ProxyPass / balancer://clusterABCD/
