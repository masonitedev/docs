# Masonite 5.1

Masonite 5.1 makes creating a new application a delightful, guided experience — inspired by the best onboarding flows in modern web frameworks.

## The `masonite new` Wizard

Creating an application is now a single command:

```text title="terminal"
$ pip install masonite-framework
$ masonite new blog
```

The new interactive wizard walks you through every decision with arrow-key menus:

* **Application stack** — full-stack (server rendered views) or API only.
* **Frontend preset** — Tailwind CSS (default), Bootstrap, Vue 3, React or none.
* **Database** — SQLite (default), MySQL or Postgres. The wizard writes the matching `.env` values and adds the right driver to `requirements.txt` (`pymysql` or `psycopg2-binary`).

It then does all the setup work for you:

* creates a virtual environment (`.venv`) and installs the dependencies,
* writes your `.env` file and generates a fresh `APP_KEY`,
* initializes a git repository with an initial commit,
* and offers to start the development server right away.

Choosing **API only** publishes `config/api.py`, enables the `ApiProvider` and writes a `JWT_SECRET` to your `.env` so you can start building JSON APIs immediately.

Every question is also a flag for scripting and CI:

```text title="terminal"
$ masonite new blog --api --db=postgres --no-input
```

!!! info

    `project start` keeps working as an alias of `masonite new`, and a new `masonite` console command is available alongside `project`.

## Bundled Project Skeleton

The starter project now ships **inside the framework package** instead of being downloaded from GitHub. This means:

* project creation is **instant** and works **offline**,
* no more GitHub API rate limits when creating several projects,
* the starter files always match the framework version you installed.

The skeleton is fully up to date with Masonite 5: `config/logging.py`, environment-driven database configuration, Masonite 5 dependency pins and an API-ready providers file.

Custom templates are still supported through the legacy download flow:

```text title="terminal"
$ masonite new blog --repo=your-username/your-template --branch=main
```

## A New Welcome Page

New applications greet you with a redesigned welcome page following the Masonite brand — the Great Pyramid mark, violet palette and Sora typeface — with automatic light and dark mode. It is a single self-contained template with no CDN dependencies.

The 403, 404, 500 and maintenance pages were redesigned to match.

## Other Changes

* New dependency: [questionary](https://github.com/tmbo/questionary) powers the arrow-key prompts (with a plain-text fallback for non-interactive terminals, `NO_COLOR` and CI environments).
* The `craft` file in new projects is created executable, and `Ctrl+C` at any wizard question cancels cleanly without leaving files behind.
