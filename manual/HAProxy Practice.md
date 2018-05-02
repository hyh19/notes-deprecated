# HAProxy Practice

http://www.haproxy.org/

## [Installation](http://www.haproxy.org/#down)

### [Debian/Ubuntu packages](https://haproxy.debian.net/)

```
$ dpkg -L haproxy
/etc/rsyslog.d/49-haproxy.conf
/etc/default/haproxy
/etc/haproxy/errors/504.http
/etc/haproxy/errors/503.http
/etc/haproxy/errors/403.http
/etc/haproxy/errors/500.http
/etc/haproxy/errors/400.http
/etc/haproxy/errors/502.http
/etc/haproxy/errors/408.http
/etc/haproxy/haproxy.cfg
/etc/logrotate.d/haproxy
/etc/init.d/haproxy
```

### [RHEL-based Docker images](https://gitlab.com/aleks001/haproxy)

### [Alpine-based Docker images](https://hub.docker.com/_/haproxy/)

## [Documentation](http://www.haproxy.org/#docs)

http://cbonte.github.io/haproxy-dconv/

### Reference Manual for version 1.6 (stable) :

[Starter guide in HTML](http://cbonte.github.io/haproxy-dconv/1.6/intro.html)

[Configuration Manual in HTML](http://cbonte.github.io/haproxy-dconv/1.6/configuration.html)

[Management Guide in HTML](http://cbonte.github.io/haproxy-dconv/1.6/management.html)

### Reference Manual for version 1.5 (stable) :

#### [Configuration Manual](http://cbonte.github.io/haproxy-dconv/1.5/configuration.html)

##### 2. [Configuring HAProxy](http://cbonte.github.io/haproxy-dconv/1.5/configuration.html#2)

###### 2.1. [Configuration file format](http://cbonte.github.io/haproxy-dconv/1.5/configuration.html#2.1)

HAProxy's configuration process involves 3 major sources of parameters :
```
- the arguments from the command-line, which always take precedence
- the "global" section, which sets process-wide parameters
- the proxies sections which can take form of "defaults", "listen", "frontend" and "backend".
```

###### 2.2. [Time format](http://cbonte.github.io/haproxy-dconv/1.5/configuration.html#2.2)

```
- us : microseconds. 1 microsecond = 1/1000000 second
- ms : milliseconds. 1 millisecond = 1/1000 second. This is the default.
- s  : seconds. 1s = 1000ms
- m  : minutes. 1m = 60s = 60000ms
- h  : hours.   1h = 60m = 3600s = 3600000ms
- d  : days.    1d = 24h = 1440m = 86400s = 86400000ms
```

###### 2.3. [Examples](http://cbonte.github.io/haproxy-dconv/1.5/configuration.html#2.3)

```
# Simple configuration for an HTTP proxy listening on port 80 on all
# interfaces and forwarding requests to a single backend "servers" with a
# single server "server1" listening on 127.0.0.1:8000
global
    daemon
    maxconn 256

defaults
    mode http
    timeout connect 5000ms
    timeout client 50000ms
    timeout server 50000ms

frontend http-in
    bind *:80
    default_backend servers

backend servers
    server server1 127.0.0.1:8000 maxconn 32


# The same configuration defined with a single listen block. Shorter but
# less expressive, especially in HTTP mode.
global
    daemon
    maxconn 256

defaults
    mode http
    timeout connect 5000ms
    timeout client 50000ms
    timeout server 50000ms

listen http-in
    bind *:80
    server server1 127.0.0.1:8000 maxconn 32
```

```
$ sudo haproxy -f configuration.conf -c
```

##### 3. [Global parameters](http://cbonte.github.io/haproxy-dconv/1.5/configuration.html#3)

**Process management and security**
```
- ca-base
- chroot
- crt-base
- daemon
- gid
- group
- log
- log-send-hostname
- nbproc
- pidfile
- uid
- ulimit-n
- user
- stats
- ssl-server-verify
- node
- description
- unix-bind
```

**Performance tuning**
```
- max-spread-checks
- maxconn
- maxconnrate
- maxcomprate
- maxcompcpuusage
- maxpipes
- maxsessrate
- maxsslconn
- maxsslrate
- noepoll
- nokqueue
- nopoll
- nosplice
- nogetaddrinfo
- spread-checks
- tune.bufsize
- tune.chksize
- tune.comp.maxlevel
- tune.http.cookielen
- tune.http.maxhdr
- tune.idletimer
- tune.maxaccept
- tune.maxpollevents
- tune.maxrewrite
- tune.pipesize
- tune.rcvbuf.client
- tune.rcvbuf.server
- tune.sndbuf.client
- tune.sndbuf.server
- tune.ssl.cachesize
- tune.ssl.lifetime
- tune.ssl.force-private-cache
- tune.ssl.maxrecord
- tune.ssl.default-dh-param
- tune.zlib.memlevel
- tune.zlib.windowsize
```

**Debugging**
```
- debug
- quiet
```

##### 4. [Proxies](http://cbonte.github.io/haproxy-dconv/1.5/configuration.html#4)

Proxy configuration can be located in a set of sections:
```
- defaults <name>
- frontend <name>
- backend  <name>
- listen   <name>
```

##### 7. [Using ACLs and fetching samples](http://cbonte.github.io/haproxy-dconv/1.5/configuration.html#7)

###### 7.1. [ACL basics](http://cbonte.github.io/haproxy-dconv/1.5/configuration.html#7.1)

```
acl <aclname> <criterion> [flags] [operator] [<value>] ...
```

The following ACL flags are currently supported :
```
-i : ignore case during matching of all subsequent patterns.
-f : load patterns from a file.
-m : use a specific pattern matching method
-n : forbid the DNS resolutions
-M : load the file pointed by -f like a map file.
-u : force the unique id of the ACL
-- : force end of flags. Useful when a string looks like one of the flags.
```

The pattern matching method must be one of the following :
```
- "found" : only check if the requested sample could be found in the stream, but do not compare it against any pattern.

- "bool"  : check the value as a boolean. 

- "int"   : match the value as an integer. 

- "ip"    : match the value as an IPv4 or IPv6 address.

- "bin"   : match the contents against an hexadecimal string representing a binary sequence.

- "len"   : match the sample's length as an integer.

- "str"   : exact match : match the contents against a string.

- "sub"   : substring match : check that the contents contain at least one of the provided string patterns.

- "reg"   : regex match : match the contents against a list of regular expressions.

- "beg"   : prefix match : check that the contents begin like the provided string patterns.

- "end"   : suffix match : check that the contents end like the provided string patterns.

- "dir"   : subdir match : check that a slash-delimited portion of the contents exactly matches one of the provided string patterns.

- "dom"   : domain match : check that a dot-delimited portion of the contents exactly match one of the provided string patterns.
```

###### 7.2. [Using ACLs to form conditions](http://cbonte.github.io/haproxy-dconv/1.5/configuration.html#7.2)

Some actions are only performed upon a valid condition. A condition is a
combination of ACLs with operators. 3 operators are supported :
```
- AND (implicit)
- OR  (explicit with the "or" keyword or the "||" operator)
- Negation with the exclamation mark ("!")
```

A condition is formed as a disjunctive form:
```
[!]acl1 [!]acl2 ... [!]acln  { or [!]acl1 [!]acl2 ... [!]acln } ...
```
Such conditions are generally used after an "if" or "unless" statement,
indicating when the condition will trigger the action.

```
acl missing_cl hdr_cnt(Content-length) eq 0
block if HTTP_URL_STAR !METH_OPTIONS || METH_POST missing_cl
block if METH_GET HTTP_CONTENT
block unless METH_GET or METH_POST or METH_OPTIONS
```

```
acl url_static  path_beg         /static /images /img /css
acl url_static  path_end         .gif .png .jpg .css .js
acl host_www    hdr_beg(host) -i www
acl host_static hdr_beg(host) -i img. video. download. ftp.

# now use backend "static" for all static-only hosts, and for static urls
# of host "www". Use backend "www" for the rest.
use_backend static if host_static or host_www url_static
use_backend www    if host_www
```

###### 7.3. [Fetching samples](http://cbonte.github.io/haproxy-dconv/1.5/configuration.html#7.3)

```
- name
- name(arg1)
- name(arg1,arg2)
```

7.3.3. [Fetching samples at Layer 4](http://cbonte.github.io/haproxy-dconv/1.5/configuration.html#7.3.3)

7.3.6. [Fetching HTTP samples (Layer 7)](http://cbonte.github.io/haproxy-dconv/1.5/configuration.html#7.3.6)

###### 7.4. [Pre-defined ACLs](http://cbonte.github.io/haproxy-dconv/1.5/configuration.html#7.4)

ACL name | Equivalent to | Usage
---|---|---
FALSE | always_false | never match
HTTP | req_proto_http |	match if protocol is valid HTTP
HTTP_1.0 | req_ver 1.0 | match HTTP version 1.0
HTTP_1.1 | req_ver 1.1 | match HTTP version 1.1
HTTP_URL_ABS | url_reg ^[^/:]*:// | match absolute URL with scheme
HTTP_URL_SLASH | url_beg / | match URL beginning with "/"
HTTP_URL_STAR | url * | match URL equal to "*"
METH_GET | method GET HEAD | match HTTP GET or HEAD method
METH_POST |	method POST	| match HTTP POST method
TRUE | always_true | always match
