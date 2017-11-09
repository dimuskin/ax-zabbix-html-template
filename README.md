# AX Zabbix Trigger Alert HTML Template
Default Zabbix trigger email alerts is not too attractive. Some tips on how to fix this and do something similar to it:

Alert email:
<br>
<kbd><img src="screenshots/alert.png?raw=true" /></kbd>

Recovery email:
<br>
<kbd><img src="screenshots/recovery.png?raw=true" /></kbd>

## Instalation

1. Install sendEmail
```
$ sudo apt-get install sendemail
```

2. Find AlertScriptsPath in Zabbix configuration file:
```
$ cat /etc/zabbix/zabbix_server.conf | grep AlertScriptsPath
### Option: AlertScriptsPath
# AlertScriptsPath=${datadir}/zabbix/alertscripts
AlertScriptsPath=/usr/lib/zabbix/alertscripts
$cd /usr/lib/zabbix/alertscripts
```

3. Create new alert script:
```
$ sudo nano html_email.sh
```

4. Paste following or simmilar content, don't forget to fill it with your real SMTP data.
```
#!/bin/sh
export smtpemailfrom=zabbix@yourdomain.com
export zabbixemailto="$1"
export zabbixsubject="$2"
export zabbixbody="$3"
export smtpserver=localhost # or your SMTP server
export smtplogin=SMTP_LOGIN
export smtppass=SMTP_PASSWORD
/usr/bin/sendEmail -f $smtpemailfrom -t $zabbixemailto -u $zabbixsubject \-m $zabbixbody -s $smtpserver:25  -o tls=no \-o message-content-type=html 
```

5. Login to your Zabbix GUI and go to Administration -> Media Types -> Email

6. Change type to Script and put 'html_email.sh' into script name.

7. Add next script parameters and click Update:
```
{ALERT.SENDTO}
{ALERT.SUBJECT}
{ALERT.MESSAGE}
```

8. Open Email alert action tab: Configuration -> Action -> Event source [Triggers] -> Report problems to Zabbix administrators

9. Copy/paste to Operations Default message content from file templates/alert.html and to Recovery operations from templates/recovery.html

10. Click Update.
