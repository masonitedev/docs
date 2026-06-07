# Directory Structure

A fresh Masonite application ships with a clear, convention-driven structure. Every directory has a single purpose, and Masonite knows where to find your controllers, models, views and configuration without you wiring anything up.

This is what a new project created with `masonite new` looks like:

```text
blog/
├── app/
│   ├── controllers/
│   │   └── WelcomeController.py
│   ├── middlewares/
│   │   ├── AuthenticationMiddleware.py
│   │   └── VerifyCsrfToken.py
│   ├── models/
│   │   └── User.py
│   ├── providers/
│   │   └── AppProvider.py
│   └── Kernel.py
├── config/
├── databases/
│   ├── migrations/
│   └── seeds/
├── resources/
│   ├── css/
│   └── js/
├── routes/
│   ├── api.py
│   └── web.py
├── storage/
├── templates/
├── tests/
├── .env
├── craft
├── package.json
├── requirements.txt
├── webpack.mix.js
└── wsgi.py
```

## The Root Directory

### The `app` Directory

The `app` directory contains the core code of your application: your controllers, models, middleware and providers. Almost all of the code you write will live here.

* **`app/controllers`** — your [controllers](../the-basics/controllers.md). Classes generated with `python craft make:controller` are placed here, and routes find them in this location automatically.
* **`app/middlewares`** — your [middleware](../the-basics/middleware.md) classes. New applications ship with an `AuthenticationMiddleware` for protecting routes and a `VerifyCsrfToken` middleware for [CSRF protection](../the-basics/csrf.md).
* **`app/models`** — your [Masonite ORM models](../orm/models.md). A `User` model is included out of the box for [authentication](../security/authentication.md).
* **`app/providers`** — your application's [service providers](../architecture/service-providers.md). The included `AppProvider` is the recommended place to register your own container bindings.
* **`app/Kernel.py`** — the application kernel. It registers your HTTP and route middleware stacks and tells Masonite where everything lives (controllers, views, routes, migrations and so on). You will mostly touch it to [register middleware](../the-basics/middleware.md).

As your application grows you will add more directories here — `app/jobs` for [queue jobs](../digging-deeper/queues.md), `app/mailables` for [mail](../digging-deeper/mail.md), `app/events` and `app/listeners` for [events](../digging-deeper/events.md), `app/policies` for [authorization policies](../security/authorization.md), `app/tasks` for [scheduled tasks](../digging-deeper/scheduling.md) and `app/commands` for [custom commands](../digging-deeper/commands.md). The `craft make:*` commands create these directories for you the first time you need them.

### The `config` Directory

All of your application's [configuration](configuration.md) files: `application.py`, `auth.py`, `broadcast.py`, `cache.py`, `database.py`, `exceptions.py`, `filesystem.py`, `logging.py`, `mail.py`, `notification.py`, `providers.py`, `queue.py`, `security.py` and `session.py`. Each file is plain Python, reads its secrets from [environment variables](environments.md), and is documented inline.

`config/providers.py` deserves a special mention: it holds the `PROVIDERS` list of [service providers](../architecture/service-providers.md) that are registered when your application boots.

### The `databases` Directory

Your database [migrations](../orm/schema-and-migrations.md) and [seeds](../orm/seeding.md). When using SQLite, the database file itself is typically created in the project root.

### The `resources` Directory

The uncompiled sources of your frontend assets — CSS and JavaScript — which are built by Laravel Mix via `webpack.mix.js` and `package.json`. See [Compiling Assets](../the-basics/compiling-assets.md).

### The `routes` Directory

Your route files. `routes/web.py` holds the `ROUTES` list for your web routes (with the `web` middleware group applied), and `routes/api.py` holds API routes which are registered under the `/api` prefix when the `ApiProvider` is enabled. See [Routing](../the-basics/routing.md) and [API Development](../digging-deeper/api.md).

### The `storage` Directory

Compiled assets, framework cache files, uploads and public files such as `favicon.ico` and `robots.txt`. See [Static Files](../the-basics/static-files.md) and [File Storage](../digging-deeper/filesystem.md):

* **`storage/public`** — files served from the root of your site.
* **`storage/static`** and **`storage/compiled`** — static and compiled assets, served under the `static/` alias.
* **`storage/uploads`** — user uploaded files.
* **`storage/framework`** — internal framework files such as the cache and session data.

### The `templates` Directory

Your [views](../the-basics/views.md) — Jinja2 templates rendered by your controllers. New applications include a `base.html` layout, the `welcome.html` page, a `maintenance.html` page and styled error pages in `templates/errors/`.

### The `tests` Directory

Your [tests](../testing/getting-started.md). It ships with a project-level `TestCase` you can extend with your own helpers, and a first `test_welcome.py` you can run immediately with `python -m pytest`.

## Key Root Files

| File | Purpose |
| :--- | :--- |
| `.env` | Your local [environment variables](environments.md). Never commit this file. |
| `.env-example` | A template of the environment variables your app needs, safe to commit. |
| `.env.testing` | Environment variables loaded when running your [test suite](../testing/getting-started.md). |
| `craft` | The entry point for the [Craft console](../digging-deeper/commands.md): `python craft serve`, `python craft make:controller`, `python craft migrate`, ... |
| `wsgi.py` | The WSGI entry point that creates and boots your application. Production servers like gunicorn point at it — see [Deployment](deployment.md). |
| `requirements.txt` | Your Python dependencies, pinned to `masonite-framework>=5.0,<6` and `masonite-framework-orm>=3.0,<4`. |
| `package.json` / `webpack.mix.js` | The Node toolchain for [compiling assets](../the-basics/compiling-assets.md). Only needed if your app has a frontend build. |
