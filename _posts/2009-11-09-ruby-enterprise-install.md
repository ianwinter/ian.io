---
layout: post
status: publish
title: Ruby Enterprise Install


date: '2009-11-09 13:31:41 +0000'
date_gmt: '2009-11-09 13:31:41 +0000'
tags:
- ruby
- rails
- enterprise
- installation
---
Trying to install Ruby Enterprise on a fresh RHEL5 64 bit box this morning and hit a few issues. Initially just the standard install requests for gcc, gcc-c++, readline-devel, openssl-devel and zlib-devel. They all installed fine, but, when I tried running the installer I got an error like this:
``` shell
In file included from /usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/algorithm:65,
from src/tcmalloc.cc:112:
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/stl_algobase.h:64:28: error: bits/c++config.h: No such file or directory
In file included from /usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/stl_algobase.h:69,
from /usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/algorithm:65,
from src/tcmalloc.cc:112:
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/iosfwd:45:29: error: bits/c++locale.h: No such file or directory
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/iosfwd:46:25: error: bits/c++io.h: No such file or directory
In file included from /usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/memory:54,
from /usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/stl_tempbuf.h:64,
from /usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/stl_algo.h:66,
from /usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/algorithm:68,
from src/tcmalloc.cc:112:
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/allocator.h:52:31: error: bits/c++allocator.h: No such file or directory
In file included from /usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/basic_string.h:45,
from /usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/string:52,
from src/base/commandlineflags.h:52,
from src/tcmalloc.cc:114:
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/atomicity.h:38:30: error: bits/atomic_word.h: No such file or directory
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/allocator.h:82: error: expected template-name before &lsquo;&amp;lt;&rsquo; token
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/allocator.h:82: error: expected `{' before &lsquo;&amp;lt;&rsquo; token
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/allocator.h:82: error: expected unqualified-id before &lsquo;&amp;lt;&rsquo; token
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/stl_algo.h: In function &lsquo;void std::random_shuffle(_RandomAccessIterator, _RandomAccessIterator)&rsquo;:
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/stl_algo.h:1906: error: &lsquo;rand&rsquo; is not a member of &lsquo;std&rsquo;
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/atomicity.h: At global scope:
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/atomicity.h:44: error: expected constructor, destructor, or type conversion before &lsquo;__exchange_and_add&rsquo;
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/atomicity.h:48: error: expected &lsquo;,&rsquo; or &lsquo;...&rsquo; before &lsquo;*&rsquo; token
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/basic_string.h:150: error: &lsquo;_Atomic_word&rsquo; does not name a type
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/basic_string.h: In member function &lsquo;void std::basic_string&amp;lt;_CharT, _Traits, _Alloc&amp;gt;::_Rep::_M_dispose(const _Alloc&amp;amp;)&rsquo;:
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/basic_string.h:232: error: &lsquo;__exchange_and_add&rsquo; is not a member of &lsquo;__gnu_cxx&rsquo;
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/basic_string.h: In member function &lsquo;typename _Alloc::rebind&amp;lt;_CharT&amp;gt;::other::size_type std::basic_string&amp;lt;_CharT, _Traits, _Alloc&amp;gt;::_M_check(typename _Alloc::rebind&amp;lt;_CharT&amp;gt;::other::size_type, const char*) const&rsquo;:
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/basic_string.h:306: error: there are no arguments to &lsquo;__N&rsquo; that depend on a template parameter, so a declaration of &lsquo;__N&rsquo; must be available
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/basic_string.h:306: error: (if you use &lsquo;-fpermissive&rsquo;, G++ will accept your code, but allowing the use of an undeclared name is deprecated)
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/basic_string.h: In member function &lsquo;void std::basic_string&amp;lt;_CharT, _Traits, _Alloc&amp;gt;::_M_check_length(typename _Alloc::rebind&amp;lt;_CharT&amp;gt;::other::size_type, typename _Alloc::rebind&amp;lt;_CharT&amp;gt;::other::size_type, const char*) const&rsquo;:
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/basic_string.h:314: error: there are no arguments to &lsquo;__N&rsquo; that depend on a template parameter, so a declaration of &lsquo;__N&rsquo; must be available
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/basic_string.h: In member function &lsquo;typename _Alloc::rebind&amp;lt;_CharT&amp;gt;::other::const_reference std::basic_string&amp;lt;_CharT, _Traits, _Alloc&amp;gt;::at(typename _Alloc::rebind&amp;lt;_CharT&amp;gt;::other::size_type) const&rsquo;:
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/basic_string.h:726: error: tnction &lsquo;typename _Alloc::rebind&amp;lt;_CharT&amp;gt;::other::const_reference std::basic_string&amp;lt;_CharT, _Traits, _Alloc&amp;gt;::at(typename _Alloc::rebind&amp;lt;_CharT&amp;gt;::other::size_type) const&rsquo;:
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/basic_string.h:726: error: there are no arguments to &lsquo;__N&rsquo; that depend on a template parameter, so a declaration of &lsquo;__N&rsquo; must be available
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/basic_string.h: In member function &lsquo;typename _Alloc::rebind&amp;lt;_CharT&amp;gt;::other::reference std::basic_string&amp;lt;_CharT, _Traits, _Alloc&amp;gt;::at(typename _Alloc::rebind&amp;lt;_CharT&amp;gt;::other::size_type)&rsquo;:
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/basic_string.h:745: error: there are no arguments to &lsquo;__N&rsquo; that depend on a template parameter, so a declaration of &lsquo;__N&rsquo; must be available
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/basic_string.tcc: In static member function &lsquo;static _CharT* std::basic_string&amp;lt;_CharT, _Traits, _Alloc&amp;gt;::_S_construct(_InIterator, _InIterator, const _Alloc&amp;amp;, std::forward_iterator_tag)&rsquo;:
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/basic_string.tcc:145: error: there are no arguments to &lsquo;__N&rsquo; that depend on a template parameter, so a declaration of &lsquo;__N&rsquo; must be available
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/basic_string.tcc: In static member function &lsquo;static typename std::basic_string&amp;lt;_CharT, _Traits, _Alloc&amp;gt;::_Rep* std::basic_string&amp;lt;_CharT, _Traits, _Alloc&amp;gt;::_Rep::_S_create(typename _Alloc::rebind&amp;lt;_CharT&amp;gt;::other::size_type, typename _Alloc::rebind&amp;lt;_CharT&amp;gt;::other::size_type, const _Alloc&amp;amp;)&rsquo;:
/usr/lib/gcc/x86_64-redhat-linux/4.1.2/../../../../include/c++/4.1.2/bits/basic_string.tcc:533: error: there are no arguments to &lsquo;__N&rsquo; that depend on a template parameter, so a declaration
```
The fix was two steps, first a simple install of libstdc++-devel then follow the <a href="http://www.rubyenterpriseedition.com/documentation.html#_step_3_install_tcmalloc">instructions </a>to install tcmalloc which is required. Odd that on a 32 bit system it installed straight away.
