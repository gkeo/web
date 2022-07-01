# rewrite - maintaining redirects in nginx from an external source - Server Fault
There is no built in way to properly isolate the rewrite configuration like that. There are three approaches you could take.

### Map module include

The [map module](http://wiki.nginx.org/HttpMapModule) allows you to include mappings from a separate file. Nginx still has to be reloaded after the file is changed, and the mapping file must be syntactically correct, but it does limit what can be done.

`nginx.conf`:

```
map $uri $new {
    include /etc/nginx/marketing.map;
}

server {
    ...
    if ($new) {
        rewrite ^ $new redirect;
    }
    ...
}

```

`marketing.map`:

```
/about  /company/about-us;
~^/people/(?<person>.*)$    /company/people/$person;

```

### Pre-process configuration

The first is to write a script which transform the redirects from some format which you define into nginx configuration. For example, given a list of space separated redirects:

```
/foo/(.*) /bar/$1

```

and a script:

```null
#!/bin/sh
while read SOURCE DEST; do
    echo "rewrite $SOURCE $DEST permanent;"
done < redirects.txt > redirects.conf
```

to form the following configuration:

```
rewrite /foo/(.*) /bar/$1 permanent;

```

You'd then want to run `nginx -t` on the entire configuration to check that it's valid before reloading.

### On-the-fly processing

The second option is to use [ngx_lua](http://wiki.nginx.org/HttpLuaModule), [ngx_perl](http://wiki.nginx.org/HttpPerlModule) or [ngx_js](https://github.com/kung-fu-tzu/ngx_http_js_module#readme) to implement reading and processing your redirect configuration in nginx itself. For example, the [`rewrite_by_lua`](http://wiki.nginx.org/HttpLuaModule#rewrite_by_lua) directive allows you to execute [Lua](http://www.lua.org/) code to construct a rewrite. You need to be careful about preformance however since you'll be interpreting code for every request. 
 [https://serverfault.com/questions/441235/maintaining-redirects-in-nginx-from-an-external-source/441517#441517](https://serverfault.com/questions/441235/maintaining-redirects-in-nginx-from-an-external-source/441517#441517)
