# Introduction and Installation

Stop using old frameworks with just a few confusing features. Masonite is the developer focused dev tool with all the features you need for the rapid development you deserve. Masonite is perfect for beginners getting their first web app deployed or advanced developers and businesses that need to reach for the full fleet of features available.

Masonite works hard to be fast and easy from install to deployment so developers can go from concept to creation in as quick and efficiently as possible. Use it for your next SaaS! Try it once and you’ll fall in love.

## Some Notable Features Shipped With Masonite

* Mail support for sending emails quickly.
* Queue support to speed your application up by sending jobs to run on a queue or asynchronously.
* Notifications for sending notifications to your users simply and effectively.
* Task scheduling to run your jobs on a schedule (like everyday at midnight) so you can set and forget your tasks.
* Events you can listen for to execute listeners that perform your tasks when certain events happen in your app.
* A BEAUTIFUL Active Record style ORM called Masonite ORM. Amazingness at your fingertips.
* Many more features you need which you can find in the docs!

These, among many other features, are all shipped out of the box and ready to go. Use what you need when you need it.

## Requirements

In order to use Masonite, you’ll need:

* Python 3.10 - 3.13
* Latest version of OpenSSL
* Pip3

!!! warning

    All commands of python and pip in this documentation is assuming they are pointing to the correct Python 3 versions. For example, anywhere you see the `python` command ran it is assuming that is a Python 3.10+ Python installation. If you are having issues with any installation steps just be sure the commands are for Python 3.10+ and not 2.7 or below.

### Linux

If you are running on a Linux flavor, you’ll need the Python dev package and the libssl package. You can download these packages by running:

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

## Installation

!!! success

    Be sure to visit the [Masonite repository](https://github.com/masonitedev/masonite) for help or guidance.

Masonite excels at being simple to install and get going. First install the Masonite package:

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

Congratulations! You’ve setup your first Masonite project! Keep going to learn more about how to use Masonite to build your applications.
