# Introduction

Masonite ORM is a beautiful, easy to use Active Record style ORM. It was built for the [Masonite Web Framework](https://github.com/masonitedev/masonite) **but is built to work in any Python project**.

It is heavily inspired by the Orator Python ORM and is designed to be a drop in replacement for Orator. Orator was in turn inspired by Laravel's Eloquent ORM, so if you are coming from a framework like Laravel you should see plenty of similarities between this project and Eloquent.

Masonite ORM currently supports MySQL, MariaDB, Postgres and SQLite 3+ databases.

## What's Included

Masonite ORM is a complete database toolkit:

* **[Models](models.md)** — expressive Active Record models with attribute casting, accessors, scopes, relationships and eager loading.
* **[Query Builder](query-builder.md)** — a fluent interface for building any query, with or without models.
* **[Migrations](schema-and-migrations.md)** — version your database schema in plain Python and roll it forward or back with simple commands.
* **[Seeding](seeding.md)** — fill your database with test or starter data.
* **[Collections](collections.md)** — query results come back as supercharged lists with dozens of helpful methods.
* **[Commands](commands.md)** — scaffold models, migrations, seeds and observers from the command line.

A taste of what working with Masonite ORM looks like:

```python
from masoniteorm.models import Model


class Post(Model):
    __fillable__ = ["title", "body"]


post = Post.create({"title": "Hello World", "body": "My first post"})

recent = Post.where("created_at", ">", "2026-01-01").order_by("created_at", "desc").get()
```

## Installation

If you are using the Masonite framework, the ORM is already installed and configured — every new application depends on `masonite-framework-orm` and ships with a ready `config/database.py`. For standalone usage in any other Python project, head over to the [Installation](installation.md) page.

## Commands

It is important that commands here are based on the standalone version of the package. If you are using Masonite, you can substitute `masonite-orm` with `craft`. If you are not using Masonite, you will just need to use `masonite-orm`:

Standalone:

```
$ masonite-orm model User
```

With Masonite:

```
$ python craft model User
```

Either one of these commands will work with Masonite but to standardize the commands you should use `craft`.
