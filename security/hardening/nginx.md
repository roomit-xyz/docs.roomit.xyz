---
description: nginx hardening
---

# Nginx

### NGINX Hardening

* Disable Token SIgn Version
* Adjusting Buffer Client
* Keepalive
* Header Restriction
* Request Retriction
* Direction Domain Unused

nginx.conf

```
##### HARDENING
# Disable Token Sign Version
server_tokens off;

# Adjust Buffer Client
client_body_buffer_size  1K;
client_header_buffer_size 1k;
client_max_body_size 1k;
large_client_header_buffers 2 1k;
client_body_timeout   10;
client_header_timeout 10;

# Keepalive
keepalive_timeout     5 5;
send_timeout          10;

# Header
add_header X-XSS-Protection "1; mode=block";
add_header X-Frame-Options SAMEORIGIN;
proxy_hide_header X-powered-by;
proxy_hide_header X-Runtime;
add_header X-Frame-Options "deny";
add_header X-Content-Type-Options "nosniff";


######
```

request-hardening.conf

```
###### Page Error
error_page 403 404 405  /fuck.html;
location = /fuck.html {
      root   /opt/www/error;
      allow all;
}

###### Request Denied
# Disable Unwanted HTTP Method
if ($request_method !~ ^(GET|HEAD|POST)$ )
{
    return 404;
}

# Block download agents
if ($http_user_agent ~* LWP::Simple|BBBike|wget) {
    return 404;
}

# Deny certain Referers
if ( $http_referer ~* (babes|forsale|girl|jewelry|love|nudit|organic|poker|porn|sex|teen) )
{
    return 404;
}



### Location Denied
location /wp-admin/
{
    deny all;
    return 404;
}

location  /wp/
{
    deny all;
    return 404;
}

location /.git/
{
    deny all;
    return 404;
}

location /docs/
{
    allow   10.66.66.0/24;
    deny    all;
    return 404;
}
```

direct.conf

```
server {
   listen 80;

   server_name gravity.roomit.xyz nym.roomit.xyz mysterium.roomit.xyz celestia.roomit.xyz gitopia.roomit.xyz;

   set $validation 0;

   if ($host = 'gravity.roomit.xyz') {
      set $validation 1;
   }

   if ($host = 'nym.roomit.xyz') {
     set $validation 1;
   }

   if ($host = 'mysterium.roomit.xyz') {
     set $validation 1;
   }

   if ($host = 'celestia.roomit.xyz') {
     set $validation 1;
   }

   if ($host = 'gitopia.roomit.xyz') {
     set $validation 1;
   }


   if ($validation = 1) {
     rewrite ^/(.*)$ https://roomit.xyz/$1 permanent;
   }

}

```
