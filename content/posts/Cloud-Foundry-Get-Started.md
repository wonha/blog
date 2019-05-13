---
title: "Cloud Foundry Get Started"
date: 2018-12-22T22:26:58+09:00
draft: false
tags: [PaaS, Clound Foundry]
---

# Basic Commands

## Create Organization & Space
```
cf create-rpaas-org -demo wonha-demo-org 28
cf target -o wonha-demo-org
cf create-space wonha-demo-dev
cf target -s wonha-dev
```

## Deploy Application
```
echo '<?php echo "Hello World!\n"; ?>' > index.php
cf push wonha-test-app -d dev.wonha.net
cf apps
curl -v wonha-test-app.dev.wonha.net
cf set-space-isolation-segment wonha-dev dev
cf push hello-world-app -d dev.wonha.net -i 2 -m 64M --random-route -k 256M
cf apps
curl -v hello-world-app.dev.wonha.net
```

## Create and Bind Service
```
cf marketplace
cf services
cf env
cf env hello-world-app
cf create-service proxy free hello-wonha-proxy
cf cups SERVICE_INSTANCE -p '{"name":"admin","password":"1234"}'
cf create-user-provided-service SERVICE_NAME -r https://route
```

## Blue Green Deployment
```
cf map-route --help
cf map-route hello-world2 hello-wonha.dev.wonha.net -n hello-world
```

## Deploy Docker Container
```
cf push my-docer-app -d dev.wonha.net --docker-image httpd
```

## General
```
cf restart APP_NAME
cf logs APP_NAME --recent
cf app APP_NAME
cf bind --help
cf service --help
cf bind-service --help
cf bind-service APP_NAME SERVICE_NAME
cf restage APP_NAME
```

