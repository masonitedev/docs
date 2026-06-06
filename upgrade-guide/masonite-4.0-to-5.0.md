Masonite 5 marks the move of the framework to the [masonitedev](https://github.com/masonitedev) organization. The legacy `MasoniteFramework/masonite` repository and the old `masonite` PyPI package are deprecated and will no longer receive updates or security fixes.

Masonite 5 is mostly compatible with Masonite 4 application code: imports, project structure and APIs are unchanged. The upgrade is mainly about the new package names, supported Python versions and modernized dependencies.

It is highly recommended that you are on the latest Masonite 4 release before upgrading to Masonite 5.

# Python Version

Masonite 5 supports Python 3.10 through 3.13. Python 3.8 and 3.9 were dropped as they reached end of life. Upgrade your interpreter before upgrading Masonite.

# Install Masonite 5

The PyPI package has been renamed from `masonite` to `masonite-framework`. Uninstall the legacy package and install the new one:

```
$ pip uninstall masonite
$ pip install "masonite-framework>=5,<6"
```

Your imports do not change — you still `import masonite` / `from masonite import ...`, and the `project` and `craft` commands work the same way.

# Masonite ORM

The ORM package has been renamed from `masonite-orm` to `masonite-framework-orm`. Masonite 5 depends on it automatically, but if your `requirements.txt` pins the ORM directly, update it:

```
$ pip uninstall masonite-orm
$ pip install "masonite-framework-orm>=3,<4"
```

Imports do not change — you still `from masoniteorm.models import Model`.

# Pendulum 3

Masonite 5 and Masonite ORM 3 use Pendulum 3. If your application uses Pendulum directly:

* Parsing is stricter in Pendulum 3 — review any direct `pendulum.parse()` calls.
* `pendulum.set_test_now()` was removed. In your tests, replace it with `pendulum.travel_to()` or simply use Masonite's built-in `self.fakeTime()` / `self.restoreTime()` test helpers which handle this for you.

# New: Logging

Masonite 5 ships with a complete logging system: 6 drivers (`terminal`, `single`, `daily`, `stack`, `syslog`, `slack`), 8 alert levels and automatic exception logging with full tracebacks.

Add the provider and a `config/logging.py` file to enable it — see the [Logging documentation](../features/logging.md).

# Repository and Documentation

* The repository now lives at [https://github.com/masonitedev/masonite](https://github.com/masonitedev/masonite) — update your git remotes, bookmarks and issue links:

```
$ git remote set-url origin https://github.com/masonitedev/masonite.git
```

* The documentation now lives at [https://docs.masonite.dev](https://docs.masonite.dev).
