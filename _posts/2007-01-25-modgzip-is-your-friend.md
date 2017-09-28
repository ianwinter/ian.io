---
layout: post
status: publish
title: mod_gzip is your friend

date: '2007-01-25 13:15:00 +0000'
date_gmt: '2007-01-25 13:15:00 +0000'
tags:
- coldfusion
- linux
- gzip
- compression
- apache
---
To save bandwidth on one off the servers I look after I decided to implement mod_gzip.
The server in questions is running RedHat ES3, apache 1.3.37, Coldfusion 7.0.2 and has the cPanel/WHM control panel.
First off was to get mod_gzip compiled, now this can be done manually, download a pre compiled one or in my case just go into WHM and rebuild apache with the mod_gzip box checked. The version it puts in is 1.3.26.1a.
As I'd already got Coldfusion running on this webserver the resulting httpd.conf file it creates became invalid so I had to move a few things about. To cut to the chase the order in which modules load is crucial with Coldfusion due to the was the mod_gzip and mod_jrun modules work intercepting requests. The following is my httpd.conf (abbriviated) with the relevant sections.
```
LoadModule ...
--snip--
LoadModule jrun_module /usr/local/coldfusionmx7/runtime/lib/wsconfig/1/mod_jrun.so
<IfModule mod_jrun.c>
    JRunConfig Verbose false
    JRunConfig Apialloc false
    JRunConfig Ssl false
    JRunConfig Ignoresuffixmap false
    JRunConfig Serverstore /usr/local/coldfusionmx7/runtime/lib/wsconfig/1/jrunserver.store
    JRunConfig Bootstrap 127.0.0.1:51011
    #JRunConfig Errorurl
<optionally redirect to this URL on errors>
    #JRunConfig ProxyRetryInterval 600
    #JRunConfig ConnectTimeout 15
    #JRunConfig RecvTimeout 300
    #JRunConfig SendTimeout 15
    AddHandler jrun-handler .jsp .jws .cfm .cfml .cfc .cfr .cfswf
</IfModule>
LoadModule gzip_module        libexec/mod_gzip.so
<IfModule mod_gzip.c>
   mod_gzip_on Yes
   mod_gzip_can_negotiate Yes
   mod_gzip_static_suffix .gz
   AddEncoding gzip .gz
   mod_gzip_update_static No
   mod_gzip_command_version '/mod_gzip_status'
   mod_gzip_temp_dir /tmp
   mod_gzip_keep_workfiles No
   mod_gzip_minimum_file_size 500
   mod_gzip_maximum_file_size 500000
   mod_gzip_maximum_inmem_size 60000
   mod_gzip_min_http 1000
   mod_gzip_handle_methods GET POST
   mod_gzip_item_exclude file .js$
   mod_gzip_item_exclude file .css$
   mod_gzip_item_exclude file .swf$
   mod_gzip_item_exclude mime ^image/
   mod_gzip_item_include file .php$
   mod_gzip_item_include file .cfm$
   mod_gzip_item_include file .jsp$
   mod_gzip_item_exclude file .pdf$
   mod_gzip_item_include file .fic$
   mod_gzip_item_include file .html$
   mod_gzip_item_include file .htm$
   mod_gzip_item_include mime ^text/html
   mod_gzip_item_include mime ^text/plain
   mod_gzip_item_include mime ^text/xml
#mod_gzip_item_include mime ^application/force_download$
#mod_gzip_item_include mime ^application/pdf$
   mod_gzip_item_include handler type-coldfusion
   mod_gzip_item_include handler jrun-handler
   mod_gzip_dechunk Yes
#then the logging directives
   LogFormat "%h %l %u %t "%V %r" %<s %b mod_gzip: %{mod_gzip_result}n In:%{mo
d_gzip_input_size}n -< Out:%{mod_gzip_output_size}n = %{mod_gzip_compression_rat
io}n pct." common_with_mod_gzip_info2
   CustomLog "logs/mod_gzip.log" common_with_mod_gzip_info2
   mod_gzip_add_header_count Yes
   mod_gzip_send_vary On
</IfModule>
AddType type-coldfusion .fic
--snip--
ClearModuleList
AddModule ...
--snip--
AddModule mod_jrun.c
AddModule mod_gzip.c
```
In my test case I had a page show as 18224 bytes originally which compressed down to 3956 bytes a saving of 14268 bytes or 79%! To test the compression I was using the <a href="http://www.port80software.com/tools/compresscheck.asp">port80software.com</a> compression check. You can also see <a href="http://www.port80software.com/tools/compresscheck.asp?url=flatpackedworld.co.uk%2Fblog%2F">this site's</a> report.
More Information (stuff I read):
<ul>
<li><a href="http://www.owensperformance.com/blog_content.cfm?articleid=206">owensperformance.com</a> - cheers Irvin!
</li>
<li><a href="http://sourceforge.net/projects/mod-gzip/">mod_gzip on sourceforge</a>
</li>
<li><a href="http://schroepl.net/projekte/mod_gzip/install.htm">How to install mod_gzip</a>
</li></ul>
