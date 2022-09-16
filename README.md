# GlitchTip on Dokku

This repository contains all the information needed to self-host a GlitchTip v2.0.5 instance on Dokku.

If you are looking to self-host to Heroku or DigitalOcean App Platform, please refer to [GlitchTip's own meta repo and documentation](https://gitlab.com/glitchtip/glitchtip). This repository is specifically for Dokku only.

## Requirements

- Dokku 0.28.1 (cannot guarantee this works on older versions of Dokku)

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

Dokku by default does not deploy the GlitchTip Celery worker.

On your Dokku host, run the Celery worker by scaling the `worker` service up to `1`.

```bash
dokku ps:scale glitchtip worker=1
```

### Configure HTTPS/SSL

Set up HTTPS/SSL with your preferred method. Dokku has a [LetsEncrypt plugin](https://github.com/dokku/dokku-letsencrypt) you can use to get certificates.

### Configure ports

GlitchTip listens on port 8080

If you have HTTPS/SSL set up, you can configure Dokku to forward HTTPS requests from port 443 to GlitchTip

```bash
dokku proxy:ports-set glitchtip https:443:8080
```

If you prefer to use HTTP, though it is highly discouraged to do, you can configure Dokku to forward HTTP requrest from port 80 to GlitchTip

```bash
dokku proxy:ports-set glitchtip http:80:8080
```

### Configure GlitchTip

Set your domain

```bash
# Replace `glitchtip.dokku.me` with your domain
dokku config:set glitchtip GLITCHTIP_DOMAIN=https://glitchtip.dokku.me
```

Set a secret key. You can use a third-party service like [Djcrety](https://djecrety.ir/) to generate one.

```bash
dokku config:set glitchtip SECRET_KEY=your_secret_key
```

Create your GlitchTip superuser

```bash
dokku run glitchtip ./manage.py createsuperuser
```

Refer to the [GlitchTip Configuration documentation](https://glitchtip.com/documentation/install#configuration) for other variables you can set such as for e-mail notifications, etc.

### Start using GlitchTip!

You're now fully set up to use GlitchTip! Go to the address you've set for your GlitchTip app and log in with your superuser to get started!

## References

- [GlitchTip's Configuration Documentation](https://glitchtip.com/documentation/install#configuration)
- [GlitchTip's meta repo on self-hosting on Heroku and DigitalOcean App Platform](https://gitlab.com/glitchtip/glitchtip)
