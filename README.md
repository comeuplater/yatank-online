# Attention please!

This plugin is not the prefered way to build reports currently. Use Yandex [Overload](https://overload.yandex.net/mainpage/guide#install) instead.

This doc is for new version of a plugin (currently work-in-progress). The old one (code and doc) is here: https://github.com/yandex-load/yatank-online/tree/1.7_branch

# yatank-online

Yandex.Tank OnlineReport plugin. Shows you some charts while you're shooting and saves a report in the end.

## How to enable
Install with ```pip install yatank-online``` and specify these lines in your Tank configuration:
```
[tank]
plugin_web=yandextank.plugins.Report
```
Then start your shooting and go to ```http://localhost:8001/```

## WebSockets and Nginx configuration

If you want to run online interface behind nginx, then this would be useful:

    http {

        <...>

        map $http_upgrade $connection_upgrade {
            default upgrade;
            '' close;
        }

        upstream backend {
          server 127.0.0.1:8001;
        }

        server {

            <...>

            location / {

                proxy_pass http://backend;

                proxy_http_version 1.1;

                proxy_set_header Host $host;
                proxy_set_header Origin $upstream_addr;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection $connection_upgrade;

            }

            <...>

        }
    }
