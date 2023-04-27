### Uses for host multiple websites (in docker conteiners) with HTTPS on a single server.

This runs nginx proxy docker container that listen to the host's 80 and 443 ports and points the requests and reponses to the appropiate web application running in other docker containers that automatically detected by the nginx-proxy.I addition it runs another container that automatically creates and renews SSL certificates.

Follow this steps:
-   Put all your web apps into one directory, for example 'web_apps'. 
-  clone this repository next to the app's folders in the web_apps:`git clone https://github.com/prikid/nginx-proxy-template.git nginx-proxy`
- so now you web_apps folder looks like this: 
    app1 app2 nginx-proxy
- for each app provide the env variable VIRTUAL_HOST with the domain name of the app and expose the http port of the app (5000 or 8000 or another) and add the external network named `nginx-proxy`:
```yaml
services:
  app1:
    environment:
      - VIRTUAL_HOST=mylottodata.io
      # - VIRTUAL_PORT=8000 # not required. If not provided, the exposed port will be used.
    expose:
      - 8000
    networks:
      - nginx-proxy
      # - another-network
networks:
  nginx-proxy:
    external: true
  # another-network:

```
- Thats all. Now you can run the nginx-proxy in it's folder: `docker compose up -d`
- And then run each app. The nginx-proxy will automaticallyb create nginx conf file and SSL sertificates for all apps. 


For more information read
this [article](https://francoisromain.medium.com/host-multiple-websites-with-https-inside-docker-containers-on-a-single-server-18467484ab95)
and [this one](https://hub.docker.com/r/nginxproxy/acme-companion)
and [this](https://github.com/nginx-proxy/nginx-proxy)


