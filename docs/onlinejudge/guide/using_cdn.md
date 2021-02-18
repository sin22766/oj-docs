# Use Content Delivery Network (CDN)

OJ currently only supports CDN acceleration for Javascript and CSS, that is, APIs and dynamically loaded components will still be requested from the source host. The configuration is very simple. Just modify the `docker-compose.yml` file under OnlineJudgeDeploy and set `STATIC_CDN_HOST` For your own CDN domain name:

```
  -STATIC_CDN_HOST=cdn.oj.com
```

Save and run `docker-compose up -d`.
