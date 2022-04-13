# GlitchTip on Dokku

This repository contains all the information needed to self-host a GlitchTip v1.11.1 instance on Dokku.

If you are looking to self-host to Heroku or DigitalOcean App Platform, please refer to [GlitchTip's own meta repo and documentation](https://gitlab.com/glitchtip/glitchtip). This repository is specifically for Dokku only.

## Requirements

- Dokku 0.27.0 (cannot guarantee this works on older versions of Dokku)

## Installation

### Create the Dokku app

On your Dokku host, create an app for GlitchTip

```bash
dokku apps:create glitchtip
```

### Set up Postgres and Redis

Ensure that the Postgres and Redis plugins are installed

```bash
sudo dokku plugin:install https://github.com/dokku/dokku-postgres.git postgres
sudo dokku plugin:install https://github.com/dokku/dokku-redis.git redis
```

Create a Postgres service and link it to your GlitchTip app

```bash
dokku postgres:create glitchtip-postgres
dokku postgres:link glitchtip-postgres glitchtip
```

Create a Redis service and link it to your GlitchTip app

```bash
dokku redis:create glitchtip-redis
dokku redis:link glitchtip-redis glitchtip
```

### Deploy GlitchTip to Dokku

Clone this repository to your local machine

```bash
git clone https://github.com/b-ggs/dokku-glitchtip.git
cd dokku-glitchtip
```

Add a new remote to the git repository pointing to your Dokku host

```bash
# Replace `dokku.me` with your Dokku host's domain or IP address
git remote add dokku dokku@dokku.me:glitchtip
```

Push to your new `dokku` remote to start deploying GlitchTip

```bash
git push dokku main
```



## References

- [GlitchTip's meta repo on self-hosting on Heroku and DigitalOcean App Platform](https://gitlab.com/glitchtip/glitchtip)
