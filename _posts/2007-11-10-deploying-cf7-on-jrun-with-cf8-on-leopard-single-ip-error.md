---
layout: post
status: publish
title: Deploying CF7 on JRun with CF8 on Leopard, Single IP Error

date: '2007-11-10 14:23:00 +0000'
date_gmt: '2007-11-10 14:23:00 +0000'
tags:
- coldfusion
- leopard
- apple
- mac
- jrun
---
Having got ColdFusion 8 running in multi-server  mode on Leopard I decided to try and deploy CF7 to it as well. Started out going well, following the same process I've always used. I created a new server, removed the default-ear, started the proxy service and stopped the web service then dropped in the cf7 ear file. Setup all the apache2 config and restarted that OK. For reference I've got a jrun.conf with the module in then a cf702.conf included by default with the jrun.conf in httpd.conf then a cf8.conf I include in vhosts as needed. The server started OK but when I tried browsing the cf7.local vhost I created I was shown the message:
```
500
java.lang.NullPointerException
	at coldfusion.license.LicenseManager._isSingleIP(LicenseManager.java:171)
	at coldfusion.license.LicenseManager.isSingleIP(LicenseManager.java:68)
	at coldfusion.CfmServlet.getFilterChain(CfmServlet.java:66)
	at coldfusion.CfmServlet.service(CfmServlet.java:103)
	at coldfusion.bootstrap.BootstrapServlet.service(BootstrapServlet.java:78)
	at jrun.servlet.ServletInvoker.invoke(ServletInvoker.java:106)
	at jrun.servlet.JRunInvokerChain.invokeNext(JRunInvokerChain.java:42)
	at jrun.servlet.JRunRequestDispatcher.invoke(JRunRequestDispatcher.java:284)
	at jrun.servlet.ServletEngineService.dispatch(ServletEngineService.java:543)
	at jrun.servlet.jrpp.JRunProxyService.invokeRunnable(JRunProxyService.java:203)
	at jrunx.scheduler.ThreadPool$ThreadThrottle.invokeRunnable(ThreadPool.java:428)
	at jrunx.scheduler.WorkerThread.run(WorkerThread.java:66)
```
Interesting as it's the same box, both configured on 127.0.0.1. A reload didn't cure it, so off to google. I found a couple of posts relating to flash remoting. The first one was over on <a href="http://www.talkingtree.com/blog/index.cfm/2004/9/15/flashgateway">Steven Erat's</a> blog from way back in 2004 related to something with flash remoting. I found a few other posts saying I'd need to remove the flash remoting ear file, which, I didn't have showing in the JMC. 
I decided to take a look in the JRun4 directory to see what flash related files there were:
```
macmini:JRun4 ianwinter$ find . -name "*flash*"
./docs/jmchelp/gs-flash-jrun.htm
./servers/cf702/SERVER-INF/temp/cfusion.war-1439562941/CFIDE/administrator/images/flashmenu.swf
./servers/cf702/SERVER-INF/temp/cfusion.war-1439562941/WEB-INF/cfform/flash-unicode-table.xml
./servers/cf702/SERVER-INF/temp/cfusion.war-1439562941/WEB-INF/cfusion/lib/flashgateway.jar
./servers/cf702/SERVER-INF/temp/cfusion.war-1439562941/WEB-INF/cfusion/lib/flashremoting_update.jar
./servers/cfusion/cfusion-ear/cfusion-war/WEB-INF/cfform/flash-unicode-table.xml
./servers/cfusion/cfusion-ear/cfusion-war/WEB-INF/cfusion/lib/flashgateway.jar
./servers/cfusion/cfusion-ear/cfusion-war/WEB-INF/cfusion/lib/preso/loadflash.js
./servers/samples/flashsamples-ear
./servers/samples/flashsamples-ear/ejbs/flashgateway
./servers/samples/SERVER-INF/classes/flashgateway
```
Some of these obviously can stay but as I don't use flash remoting I decided to remove the flashgateway.jar and flashremoting-update.jar files. I backed up the servers directory as well first and have at this point stopped the servers.
```
macmini:JRun4 ianwinter$ sudo rm
./servers/cf702/SERVER-INF/temp/cfusion.war-1439562941/WEB-INF/cfusion/lib/flashgateway.jar
./servers/cf702/SERVER-INF/temp/cfusion.war-1439562941/WEB-INF/cfusion/lib/flashremoting_update.jar
./servers/cfusion/cfusion-ear/cfusion-war/WEB-INF/cfusion/lib/flashgateway.jar
```
Restart all the JRun servers and browse back to cf7.local and voila it works. Bit of a faf and no good if you want flash remoting but it works. I don't know if you remove just one of the flash remoting jar's, like just from the cf7 server if it'd work, or, perhaps you can change mappings somewhere, solves my problem so I'm happy.
