# NGINX 301 list of redirects SEO - Server Fault
I have about 250 URLS that need to be redirected from an old site to new. In the server block I set this statement:

```
include /etc/nginx/301s/somesite.conf;

```

Then in the included file I have a list of the urls (shown are four of them):

```
location = /programs-services/ { return 301 https://www.somesite.com/ }
location = /meeting-planner/ { return 301 https://www.somesite.com/speaker-kit/ }
location = /media/ { return 301 https://www.somesite.com/ }
location = /research-fund/ { return 301 https://www.somesite.com/about/ }

```

However, Nginx won't restart and complains:

```
unexpected "}" at the end of the first line.

```

Is there a better / correct way to implement this?

asked Jan 4, 2018 at 16:58

\[

![](https://www.gravatar.com/avatar/f43eba489b9bffa85af0755be3462479?s=64&d=identicon&r=PG)

]([https://serverfault.com/users/450702/gigaboy](https://serverfault.com/users/450702/gigaboy))

I don't know of a better way, although I too would like to, but your configuration is broken due to missing `;` after return directive.

```
location = /programs-services/ { return 301 https://www.somesite.com/; }
location = /meeting-planner/ { return 301 https://www.somesite.com/speaker-kit/; }

```

answered Jan 4, 2018 at 17:53

\[

![](https://i.stack.imgur.com/km5EG.png?s=64&g=1)

]([https://serverfault.com/users/405852/virullius](https://serverfault.com/users/405852/virullius))

[virullius](https://serverfault.com/users/405852/virullius)virullius

9788 silver badges22 bronze badges

3

Not the answer you're looking for? Browse other questions tagged [nginx](https://serverfault.com/questions/tagged/nginx "show questions tagged 'nginx'") [301-redirect](https://serverfault.com/questions/tagged/301-redirect "show questions tagged '301-redirect'") or [ask your own question](https://serverfault.com/questions/ask).

* * *

 [https://serverfault.com/questions/890736/nginx-301-list-of-redirects-seo](https://serverfault.com/questions/890736/nginx-301-list-of-redirects-seo)
