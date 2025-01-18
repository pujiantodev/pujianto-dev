---
author: Pujianto DEV
pubDatetime: 2025-01-18T09:37:42.681Z
modDatetime: 2025-01-18T09:37:42.681Z
title: Deploy Laravel
slug: deploy-laravel
featured: true
draft: false
tags:
  - docs
  - laravel
description: Panduan deploy Laravel di server VPS Linux. Pada panduan ini saya menggunakan Droplet di vendor Digital Ocean.
---

Panduan deploy Laravel di server VPS Linux. Pada panduan ini saya menggunakan Droplet di vendor Digital Ocean.

## Table of contents

## Update via Git

### Back End

- down app

```
sudo php artisan down
```

- git pull

```
sudo git pull
```

- run composer install

```shel
sudo composer install --optimize-autoloader --no-dev
```

- run artisan migrate when needed

```
sudo php artisan migrate
```

### Front End

#### PNPM

- pnpm install

```
sudo pnpm install
```

- pnpm build

```
sudo pnpm build
```

- remove node_modules (optional)

```
sudo rm -rf node_modules
```

### Optimize

- run

```
sudo php artisan optimize
```

- up app

```
sudo php artisan up
```

ini optional sesuai kebutuhan.

## Supervisor

ini digunakan jika menggunakan fitur job dan queue

- Status

```
sudo supervisorctl status
```

- Restart All

```
sudo supervisorctl restart all
```
