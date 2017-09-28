---
layout: post
status: publish
title: ColdFusion 8 on Leopard part 2

date: '2007-10-31 23:13:00 +0000'
date_gmt: '2007-10-31 23:13:00 +0000'
tags:
- coldfusion
- leopard
- apple
- mac
---
So, just got ColdFusion 8 working on my Mac Mini running an upgraded version of Leopard (from Tiger). Below are the steps I took. As I said before it's basically Geoff's guide with a couple more reboots!
1. Install Apple XCode Tools from Leopard DVD<br>
2. Restart<br>
3. Install ColdFusion 8<br>
	Multi-server, developer edition
	ColdFusion 8 Document and Start ColdFusion on system init both checked
	Installed to /Applications/JRun4
	Web Servers
	- Apache
	- Configuration Directory: /etc/apache2
	- Server Binary: /usr/sbin/httpd
	- Server Control Script: /usr/sbin/apachectl
	CFIDE installed to default documents folder which is /Library/Webserver/Documents
4. Complete wizard ignoring web server startup and connector failure messages
5. Restart
6. Compile the new connector
```
cd /Applications/JRun4/lib
	unzip -d src wsconfig.jar
	cd src/connectors/src
	apxs -c -Wc,-arch -Wc,x86_64 -Wl,-arch -Wl,x86_64 -n jrun22 mod_jrun22.c jrun_maptable_impl.c jrun_property.c jrun_session.c platform.c jrun_mutex.c jrun_proxy.c jrun_utils.c
	apxs -i -n jrun22 -S LIBEXECDIR=/Applications/JRun4/lib/src/connectors/src/ mod_jrun22.la
	strip mod_jrun22.so
```
7. Check the connector with wsconfig
```
cd /Applications/JRun4/lib/
	sudo java -jar wsconfig.jar
```
8. Delete the current connector and put your fresh shiny new one in
```
rm /Applications/JRun4/lib/wsconfig/1/mod_jrun22.so
cp /Applications/JRun4/lib/src/connectors/src/mod_jrun22.so /Application/JRun4/lib/wsconfig/1/mod_jrun22.so
```
9. Restart<br>
10. Start CF<br>
```
sudo /Applications/JRun4/bin/jrun -start cfusion
```
11. Kick apache
```
sudo apachectl restart
```
12. Browse to localhost/CFIDE/administrator/index.cfm (unless you've already added index.cfm as a default doc)<br>
13. Go to bed feeling happy.
