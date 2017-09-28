---
layout: post
status: publish
title: Get a domain from a URL


date: '2009-10-07 17:11:14 +0100'
date_gmt: '2009-10-07 16:11:14 +0100'
tags:
- coldfusion
- code
---
Been looking for a function in ColdFusion to get the root domain from a url. There are a few but none actually cope with the many combinations. This function by no means copes with every combination however it does cover quite a lot. Comments inline indicate which domains the rules handle.
```

<cffunction name="getRootDomain" returntype="string" output="no" hint="Take a URL and get the domain from it.">
	<cfargument name="url" type="string" required="true">
	<cfset var dots = listLen(arguments.url,".")>
	</cfset><cfset var domain = "">
	</cfset><cfset host = arguments.url>
	<!--- host.com, host.biz, host.ly --->
	<cfif dots eq 2>
		<cfset domain = host>
	</cfset></cfif>
	<cfif dots eq 3>
		<!--- www.host.com, sub.host.com, www.host.mobi, www.host.ly, del.icio.us --->
		</cfif><cfif listfirst(host,".") eq "www" or
			((len(listgetat(host,3,".")) eq 3 or len(listgetat(host,3,".")) eq 2) and len(listgetat(host,"2",".")) gte 3)>
			<cfset domain = listdeleteat(host,1,".")>
		</cfset></cfif>
		<!--- host.co.uk, host.com.za --->
		<cfif (len(listgetat(host,3,".")) eq 2 and (len(listgetat(host,2,".")) eq 2) or len(listgetat(host,2,".")) eq 3)>
			<cfset domain = host>
		</cfset></cfif>
	<cfif dots eq 4>
		<!--- www.host.ltd.uk, www.host.co.uk --->
		</cfif><cfif len(listgetat(host,4,".")) eq 2 and (len(listgetat(host,3,".")) eq 2 or len(listgetat(host,3,".")) eq 3)>
			<cfset domain = listdeleteat(host,1,".")>
		</cfset></cfif>
		<!--- www.sub.host.com, sub.sub.host.com --->
		<cfif len(listgetat(host,4,".")) eq 3 and len(listgetat(host,4,".")) gte 3>
			<cfset domain = listdeleteat(listdeleteat(host,1,"."),1,".")>
		</cfset></cfif>
	<cfif dots eq 5>
		<!--- sub.sub.host.co.uk, www.sub.host.co.uk, www.sub.host.com.za --->
		</cfif><cfif len(listgetat(host,5,".")) eq 2 and (len(listgetat(host,4,".")) eq 2 or len(listgetat(host,4,".")) eq 3)>
			<cfset domain = listdeleteat(listdeleteat(host,1,"."),1,".")>
		</cfset></cfif>
	<cfreturn domain />
</cfset></cfargument></cffunction>

```
