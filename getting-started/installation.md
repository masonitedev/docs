# Installation

Masonite excels at being simple to install and get going. This page walks you through creating your first Masonite application, from system requirements to a running development server.

## Requirements

In order to use Masonite, you'll need:

* Python 3.10 - 3.13
* Latest version of OpenSSL
* Pip3

!!! warning

    All commands of python and pip in this documentation is assuming they are pointing to the correct Python 3 versions. For example, anywhere you see the `python` command ran it is assuming that is a Python 3.10+ Python installation. If you are having issues with any installation steps just be sure the commands are for Python 3.10+ and not 2.7 or below.

### Linux

If you are running on a Linux flavor, you'll need the Python dev package and the libssl package. You can download these packages by running:

#### Debian and Ubuntu based Linux distributions

```text title="terminal"
$ sudo apt install python3-dev python3-pip libssl-dev build-essential python3-venv
```

Or you may need to specify your `python3.x-dev` version:

```text title="terminal"
$ sudo apt-get install python3.10-dev python3-pip libssl-dev build-essential python3-venv
```

#### Enterprise Linux based distributions \(Fedora, CentOS, RHEL, ...\)

```text title="terminal"
# dnf install python-devel openssl-devel
```

## Creating Your First Application

!!! success

    Be sure to visit the [Masonite repository](https://github.com/masonitedev/masonite) for help or guidance.

First install the Masonite package:

```text title="terminal"
$ pip install masonite-framework
```

Then create a new application:

```text title="terminal"
$ masonite new blog
```

This starts an interactive wizard that guides you through everything with arrow-key menus:

* the **application stack** — full-stack (server rendered views) or API only,
* the **frontend preset** — Tailwind CSS (default), Bootstrap, Vue 3, React or none,
* the **database** — SQLite (default), MySQL or Postgres,
* whether to create a **virtual environment** and install the dependencies (recommended),
* whether to initialize a **git repository**.

The wizard then crafts your project, writes your `.env` file, generates your application key and offers to start the development server right away. Open `http://localhost:8000` and you will see your new application's welcome page.

!!! info

    `masonite new .` crafts the application in the current directory (it must be empty), and `project start` keeps working as an alias of `masonite new` if you are used to previous versions of Masonite.

## Scripting the Installation \(optional\)

Every wizard question is also a flag, so you can skip the questions entirely — useful in scripts, CI or when you already know what you want:

```text title="terminal"
$ masonite new blog --api --db=postgres --no-input
```

| Flag | Description |
| :--- | :--- |
| `--api` | Scaffold an API-ready project \(publishes `config/api.py` and enables the `ApiProvider`\) |
| `--preset=` | Frontend preset: `tailwind`, `bootstrap`, `vue`, `react` or `none` |
| `--db=` | Database driver: `sqlite`, `mysql` or `postgres` |
| `--no-venv` | Do not create a virtual environment or install dependencies |
| `--no-git` | Do not initialize a git repository |
| `--no-serve` | Do not offer to start the development server |
| `--no-input` | Ask no questions and use the defaults |
| `--force` | Craft into a non-empty directory |

If you skipped the virtual environment during creation you can always set one up manually inside your project:

```text title="terminal"
$ cd blog
$ python -m venv .venv
$ source .venv/bin/activate   # Windows: .venv\Scripts\activate
$ pip install -r requirements.txt
```

## Running the Development Server

Inside your project \(with the virtual environment activated\) run the migrations and start the server:

```text title="terminal"
$ python craft migrate
$ python craft serve
```

Congratulations! You've setup your first Masonite project!

## Next Steps

* Get familiar with the [Directory Structure](directory-structure.md) of your new application.
* Learn how [Configuration](configuration.md) and [Environments](environments.md) work.
* Build something real by following the [blog tutorial](create-a-blog.md).
