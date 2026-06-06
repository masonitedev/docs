# Masonite 5.0

Masonite 5.0 is the first release published from the [masonitedev](https://github.com/masonitedev) organization, and it is dedicated to the memory of Joseph "Joe" Mancuso, the creator of Masonite, who sadly passed away in November 2025. The project continues in his memory.

## New Home and Package Names

* The framework moved to [github.com/masonitedev/masonite](https://github.com/masonitedev/masonite) and is now published on PyPI as **`masonite-framework`**.
* Masonite ORM moved to [github.com/masonitedev/orm](https://github.com/masonitedev/orm) and is now published as **`masonite-framework-orm`**.
* Imports are unchanged in both packages.
* The legacy repositories and PyPI packages (`masonite`, `masonite-orm`) are deprecated and will no longer receive updates.

## Logging

Masonite 5 ships a complete logging system, configured in `config/logging.py` and used through the `Log` facade:

```python
from masonite.facades import Log

Log.info("hello!")
Log.channel("daily").warning("Some message")
Log.stack("console", "slack").critical("Some message")
```

* 6 drivers: `terminal` (colored console output), `single` (one file), `daily` (rotating files), `stack` (multiple channels at once), `syslog` and `slack`.
* 8 alert levels: `debug`, `info`, `notice`, `warning`, `error`, `critical`, `alert`, `emergency`.
* Per-channel options: `level`, `timezone`, `format`, `date_format` and `propagate`.
* On-demand channels with `Log.build(driver, options)`.
* **Unhandled exceptions are logged automatically** with their type, message and full traceback.

Read more in the [Logging documentation](../features/logging.md).

## Python 3.10–3.13

Masonite 5 supports Python 3.10, 3.11, 3.12 and 3.13. Python 3.8 and 3.9 were dropped as they reached end of life.

## Modernized Foundation

* **Pendulum 3** and **Masonite ORM 3** under the hood.
* Werkzeug 3, Jinja2 3.1+, bcrypt 4+, modern `cryptography`, and the `multipart` 1.x parser.
* Modern PEP 621 packaging and PyPI Trusted Publishing (OIDC) — releases are built and published automatically from GitHub Actions without long-lived credentials.

## Upgrading

See the [Masonite 4.0 to 5.0 upgrade guide](../upgrade-guide/masonite-4.0-to-5.0.md). For most applications the upgrade is just swapping the packages — application code, imports and project structure are unchanged.
