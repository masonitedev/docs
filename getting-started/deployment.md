# Deployment

When you are ready to take your Masonite application to production, there are a few important things you should do to make sure your application runs as securely and efficiently as possible.

## The WSGI Entry Point

Every Masonite project ships with a `wsgi.py` file in the project root which creates and boots the application and exposes it as a standard WSGI callable named `application`. Any WSGI-compliant application server can serve it.

!!! warning

    `python craft serve` runs the Werkzeug development server. It is perfect for local development but is **not** designed for production traffic — always use a production WSGI server.

### Gunicorn

[Gunicorn](https://gunicorn.org) is the most common choice on Linux:

```text title="terminal"
$ pip install gunicorn
$ gunicorn --workers 4 --bind 0.0.0.0:8000 wsgi:application
```

A common rule of thumb for the worker count is `(2 x CPU cores) + 1`.

### Waitress

[Waitress](https://docs.pylonsproject.org/projects/waitress/) is a pure-Python alternative that also works on Windows:

```text title="terminal"
$ pip install waitress
$ waitress-serve --port=8000 wsgi:application
```

## Production Checklist

### Turn Off Debug Mode

Never deploy an application with debug mode enabled. With `APP_DEBUG=True`, unhandled exceptions render a full debug page that can expose environment variables, configuration values and source code to the world. In your production `.env`:

```text
APP_ENV=production
APP_DEBUG=False
APP_URL=https://example.com
```

### Set Your Application Key

Masonite uses the `APP_KEY` environment variable to encrypt cookies, sign data and more. Generate a fresh key for production — do not reuse the one from development:

```text title="terminal"
$ python craft key
```

This writes the new key into the `.env` file on the machine where it runs (use `python craft key --dont-store` to only print it). Keep it secret: anyone holding the key can decrypt and forge your cookies.

### Configure Your Environment

Your `.env` file should never be committed to version control. On your server, either create the `.env` file by hand or provide the variables through your platform's environment configuration. See [Environments](environments.md) for everything Masonite reads from the environment, and remember to configure your production [database](../orm/installation.md), [mail driver](../digging-deeper/mail.md), [queue driver](../digging-deeper/queues.md) and [logging channels](../the-basics/logging.md).

### Run Your Migrations

```text title="terminal"
$ python craft migrate
```

### Compile Your Assets

If your application uses a frontend preset, build the production assets and make sure the compiled files end up in your storage directories:

```text title="terminal"
$ npm install
$ npm run prod
```

## Serving Static Files

Masonite serves static and uploaded files itself through [WhiteNoise](http://whitenoise.evans.io/en/stable/), which is built into every application via the `WhitenoiseProvider`. This means a plain gunicorn deployment already serves your CSS, JavaScript and uploads correctly with proper caching headers — no extra configuration needed.

For high-traffic applications you can additionally put a reverse proxy such as nginx in front of your application server:

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

## Maintenance Mode

When deploying a new version you can put the application into maintenance mode so visitors see a friendly maintenance page (rendered from `templates/maintenance.html`) instead of a half-deployed application:

```text title="terminal"
$ python craft down
# ... deploy, migrate, build assets ...
$ python craft up
```

## Queue Workers and the Scheduler

If your application dispatches [jobs](../digging-deeper/queues.md) to a queue, remember that workers are separate, long-running processes that you must keep alive with a process manager such as systemd or supervisor:

```text title="terminal"
$ python craft queue:work
```

If you use [task scheduling](../digging-deeper/scheduling.md), add the schedule runner to your server's cron so it runs every minute:

```text
* * * * * cd /home/your-app && /home/your-app/.venv/bin/python craft schedule:run >> /dev/null 2>&1
```
