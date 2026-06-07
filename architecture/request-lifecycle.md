# Request Lifecycle

When you use a tool every day, it pays to understand how it actually works. This page walks through what happens between a request hitting your application and a response going back out. Once you see the flow — kernels, providers, middleware, route, controller, response — the rest of Masonite stops feeling like magic and starts feeling like a well organized toolbox.

## Lifecycle Overview

### First Things: `wsgi.py`

The entry point for every Masonite application is the `wsgi.py` file in your project root. It builds the application object and registers the two kernels:

```python
from masonite.foundation import Application, Kernel
from masonite.utils.location import base_path

from app.Kernel import Kernel as ApplicationKernel
from config.providers import PROVIDERS

application = Application(base_path())

application.register_providers(Kernel, ApplicationKernel)

application.add_providers(*PROVIDERS)
```

This file runs **once**, when the WSGI server (the [development server](../digging-deeper/commands.md) or gunicorn in [production](../getting-started/deployment.md)) starts — not on every request. Everything registered here stays in memory for the life of the process.

### The Two Kernels

* The **framework kernel** (`masonite.foundation.Kernel`) loads your `.env` file, binds the core machinery into the [Service Container](service-container.md) — the router, the middleware capsule, the craft commands — and sets the response handler, the function that will process every incoming request.
* Your **application kernel** (`app/Kernel.py`) is yours to edit. It loads your configuration, declares your HTTP and route middleware stacks, and registers all the locations Masonite should know about: where your controllers, routes, views, models and migrations live.

### Building a Custom Kernel

Most applications never touch the framework kernel — the standard `Kernel` gives you everything, including the full suite of craft commands. But if you need a leaner boot (a queue worker, a slimmed-down service, or an embedded use that doesn't need the command line at all), you can build your own root kernel on top of `CoreKernel`:

```python
from masonite.foundation import CoreKernel


class Kernel(CoreKernel):
    def register(self) -> None:
        super().register()
        # register only the commands and services this application needs
```

`CoreKernel` registers just the essentials: it loads your environment, sets the response handler and storage path, and binds the router, the middleware capsule, the loader, and an empty command registry. The standard `Kernel` extends it to add the built-in craft commands on top, and — when running under the test runner — the test response helpers.

Both kernels are exported from `masonite.foundation`, and every import inside them is loaded lazily, so a custom kernel only pays for what it actually registers.

### Service Providers: `register()`

Next, every [service provider](service-providers.md) listed in `config/providers.py` has its `register()` method called, one by one, in order. Providers use `register()` to bind their services into the container: the view engine, the mail manager, the queue manager, the ORM and so on.

One provider worth knowing about: the `WhitenoiseProvider` wraps the response handler with [WhiteNoise](http://whitenoise.evans.io/en/stable/), which is how requests for [static files](../the-basics/static-files.md) are answered directly — a request for `/static/app.css` never reaches the router at all.

At this point the application is fully booted and waiting for traffic.

### A Request Arrives

For each request, the WSGI server calls the application with the request environment, and Masonite's response handler takes over:

1. The WSGI `environ` (path, method, headers, input) is bound into the container.
2. Every provider's `boot()` method is **resolved through the container**, in registration order. Where `register()` ran once at startup, `boot()` runs on every request.

Two boots do the heavy lifting:

* **`FrameworkProvider.boot()`** builds the [Request](../the-basics/request.md) and [Response](../the-basics/response.md) objects from the WSGI environ and binds them into the container.
* **`RouteProvider.boot()`** carries the request through the rest of its life:
    1. The router looks for a route matching the request path, HTTP method and subdomain. No match raises a 404 through the [exception handler](../the-basics/error-handling.md).
    2. The **HTTP middleware** stack (the `http_middleware` list in `app/Kernel.py`) runs its `before` methods — this is where cookies are decrypted and [maintenance mode](../getting-started/deployment.md#maintenance-mode) is enforced.
    3. Route parameters (like `@id`) are extracted, and the **route middleware** for the matched route (`web`, `auth`, `throttle`, ...) run their `before` methods — sessions start, the user is loaded, [CSRF is verified](../the-basics/csrf.md).
    4. The route calls your **[controller](../the-basics/controllers.md)**. The container inspects your method's parameters and injects whatever you asked for — the request, the view, the mail manager, anything bound in the container.
    5. Whatever your controller returns — a view, a dictionary, a string, a model — is set on the **response** object.
    6. Route middleware and then HTTP middleware run their `after` methods, in reverse order of the way in.

If an exception is raised anywhere along the way, it is handed to the exception handler, which logs it and renders the debug page (in [debug mode](../getting-started/environments.md#debug-mode)) or your error templates in production. See [Error Handling](../the-basics/error-handling.md).

### The Response Goes Out

Finally the response handler reads the status code, headers and cookies from the response object and hands the content back to the WSGI server, which sends it to the user's browser. Lifecycle complete.

## Where You Hook In

The lifecycle is deliberately easy to extend at every step:

* Bind or override services in a [service provider](service-providers.md) — your app ships with an `AppProvider` ready for this.
* Run code before or after every request (or specific routes) with [middleware](../the-basics/middleware.md).
* Listen for framework and application [events](../digging-deeper/events.md) to react without coupling.
* Customize how exceptions render with your own handlers in [Error Handling](../the-basics/error-handling.md).
