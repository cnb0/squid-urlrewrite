## Squid Url Rewriter

This program is for `url_rewrite_program`, and concurrency support.

Require: `squid-3.4+`

### Install


```
go get github.com/rchunping/squid-urlrewrite
```

### squid config:

```
url_rewrite_program /path/to/squid-urlrewrite
url_rewrite_children 20 startup=1 idle=1 concurrency=10000
```

### rewrite config file location:
```
search in this order:
/<program_dir>/squid-urlrewrite.conf
/usr/local/etc/squid-urlrewrite.conf
/etc/squid-urlrewrite.conf
```

### squid-urlrewrite.conf example:
```
# example

# loglevel
# info: default
# debug: more detail info
# log messages are write to syslog
loglevel info

# rewrite <regexp> <target>

rewrite ^https?://webserver.domain.com/file/(\d+)/  http://192.168.1.3:1234/backend/file/read?file_id=$1

```

### reload configure
```
kill -HUP <pid of squid-urlrewrite>
```
or
```
squid -k reconfigure
```


### Test
```bash
$ ./squid-urlrewrite
1 http://webserver.domain.com/file/123/test.png  // <-- input
1 OK rewrite-url="http://192.168.1.3:1234/backend/file/read?file_id=123" // <-- response
2 http://github.com
2 ERR // <-- It's ok, means no rewrite rule matched.

```

### About squid redirector

[http://wiki.squid-cache.org/Features/Redirectors](http://wiki.squid-cache.org/Features/Redirectors)
