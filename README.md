# Welcome to Masonite

Masonite is a modern, developer-centric, "batteries included" Python web framework. It gives you everything you need to build and ship a complete web application — routing, an expressive Active Record ORM, mail, queues, notifications, task scheduling, events, authentication, validation and much more — out of the box, with very little configuration.

If you have used frameworks like Laravel or Ruby on Rails, Masonite will feel instantly familiar: it follows the MVC pattern, favors convention over configuration, and works hard so you can go from concept to creation as quickly and efficiently as possible. If Masonite is your first web framework, even better — it is designed to be easy to pick up for beginners while still scaling to the needs of advanced developers and businesses.

A taste of what building with Masonite looks like:

```python
# routes/web.py
from masonite.routes import Route

ROUTES = [
    Route.get("/", "WelcomeController@show"),
    Route.get("/posts/@id", "PostController@show"),
]
```

```python
# app/controllers/PostController.py
from masonite.controllers import Controller
from masonite.request import Request
from masonite.views import View

from app.models.Post import Post


class PostController(Controller):
    def show(self, view: View, request: Request):
        post = Post.find(request.param("id"))
        return view.render("posts.show", {"post": post})
```

## Why Masonite?

* **Batteries included.** Mail, queues, notifications, broadcasting, caching, task scheduling, events, file storage, authentication and authorization are all first-party features — no plugin hunting required. Use what you need when you need it.
* **A beautiful Active Record ORM.** [Masonite ORM](orm/introduction.md) lets you work with your database using expressive, elegant models, migrations, relationships and collections.
* **Delightful developer experience.** A single `masonite new` command scaffolds a complete project with an interactive wizard. The `craft` command line tool generates controllers, models, jobs, mailables and more.
* **Simple from install to deployment.** Sensible defaults, environment-based configuration and a clear [directory structure](getting-started/directory-structure.md) mean you spend your time building features, not wiring infrastructure.

## The Story Behind Masonite

Masonite was created in 2017 by [Joseph "Joe" Mancuso](https://github.com/josephmancuso). Joe wanted Python to have the kind of framework he loved working with in other ecosystems — one obsessed with developer experience, where everything you need ships out of the box and just works. He built Masonite from the ground up and poured years of work, care and enthusiasm into the framework, its packages and its community, taking it from a one-person project to a framework used in production around the world.

Joe sadly passed away in November 2025. Everything you see here exists because of him.

The project continues in his memory under the [masonitedev](https://github.com/masonitedev) organization, maintained by the community he built. [Masonite 5](whats-new/masonite-5.0.md), the first release published from the new organization, is dedicated to him — and the developer-first spirit he gave the framework continues to guide every release.

Thank you for everything, Joe. ❤️

## How the Documentation Is Organized

This documentation is designed to be read from less to more — but every page also stands on its own when you need a reference:

* **[Getting Started](getting-started/installation.md)** — install Masonite, create your first application, understand configuration and the directory structure, and deploy to production.
* **[Architecture Concepts](architecture/request-lifecycle.md)** — how Masonite works under the hood: the request lifecycle, the service container, service providers and facades.
* **The Basics** — the features you will use in every application: [routing](the-basics/routing.md), [controllers](the-basics/controllers.md), [requests](the-basics/request.md), [responses](the-basics/response.md), [views](the-basics/views.md), [validation](the-basics/validation.md) and friends.
* **Digging Deeper** — the rest of the toolbox: [mail](digging-deeper/mail.md), [queues](digging-deeper/queues.md), [events](digging-deeper/events.md), [caching](digging-deeper/cache.md), [task scheduling](digging-deeper/scheduling.md), [API development](digging-deeper/api.md) and more.
* **Security** — [authentication](security/authentication.md), [authorization](security/authorization.md), [CORS](security/cors.md) and [hashing](security/hashing.md).
* **[Masonite ORM](orm/introduction.md)** — models, query builder, migrations, seeding and collections.
* **[Testing](testing/getting-started.md)** — everything you need to test your application with confidence.

## First Steps

Ready to build something?

1. Head over to the [Installation](getting-started/installation.md) guide and create your first application — it takes a single command.
2. Follow the [Creating a Blog](getting-started/create-a-blog.md) tutorial to build your first real application, from routes to database to views.
3. Check out [what's new in Masonite 5](whats-new/masonite-5.0.md) if you are coming from an earlier version.

Try it once and you'll fall in love.
