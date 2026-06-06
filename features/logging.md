Logging is a crucial part of any application. Logging allows you to see errors your application throws as well as log your own messages in several different alert levels. Masonite ships with a full logging system with multiple drivers, and it automatically logs unhandled exceptions with their full traceback.

# Configuration

To enable logging in your application, ensure the `LoggingProvider` is added to your application providers:

```python
# config/providers.py
from masonite.providers import LoggingProvider

PROVIDERS = [
    # ...
    LoggingProvider,
]
```

Logging configuration is done in `config/logging.py` through channels. Each channel uses a driver. Available drivers are `terminal`, `single`, `daily`, `stack`, `syslog` and `slack`:

```python
# config/logging.py
CHANNELS = {
    "default": {
        "driver": "console",
        # these settings apply to every channel unless overridden per channel
        "level": "info",
        "timezone": "UTC",
        "format": "{timestamp} - {levelname}: {message}",
        "date_format": "YYYY-MM-DD HH:mm:ss",
        "propagate": False,
    },
    "console": {
        "driver": "terminal",
    },
    "single": {
        "driver": "single",
        "path": "logs/masonite.log",
    },
    "daily": {
        "driver": "daily",
        "path": "logs",
        "days": 7,
        "keep": 10,
    },
    "all": {
        "driver": "stack",
        "channels": ["single", "daily", "console"],
    },
    "syslog": {
        "driver": "syslog",
        "address": "/var/log/system.log",
    },
    "papertrail": {
        "driver": "syslog",
        "host": "logs.papertrailapp.com",
        "port": None,  # specify the port as an integer
    },
    "slack": {
        "driver": "slack",
        "webhook_url": "",
    },
}
```

The `default` channel defines which channel is used when none is specified, and its options (`level`, `timezone`, `format`, `date_format`, `propagate`) act as fallbacks for every other channel.

## Channel Options

| Option | Description |
| --- | --- |
| `driver` | The driver used by this channel: `terminal`, `single`, `daily`, `stack`, `syslog` or `slack`. |
| `level` | The minimum level a message must have to be logged: `debug`, `info`, `notice`, `warning`, `error`, `critical`, `alert` or `emergency`. |
| `timezone` | Timezone used to render the `{timestamp}` of the log message. |
| `format` | The log line format. Available placeholders are `{timestamp}`, `{levelname}` and `{message}`. |
| `date_format` | Format of the `{timestamp}` placeholder (Pendulum tokens, e.g. `YYYY-MM-DD HH:mm:ss`). |
| `propagate` | Whether messages should also propagate to ancestor (root) Python loggers. Defaults to `False` to avoid duplicated log lines. |

## Drivers

* **terminal**: outputs colored log lines to the console.
* **single**: appends every message to a single log file (`path`).
* **daily**: writes to a rotating daily file. `days` controls the rotation interval and `keep` the number of backup files kept.
* **stack**: forwards the message to a list of other `channels` at once.
* **syslog**: sends messages to a syslog daemon. Use `address` for a local socket (e.g. `/var/log/system.log`) or `host` and `port` for a remote collector such as Papertrail.
* **slack**: posts messages to a Slack incoming webhook (`webhook_url`).

# Logging Messages

Use the `Log` facade to log a message to the default configured channel in any of the 8 alert levels:

```python
from masonite.facades import Log

Log.debug("hello!")
Log.info("hello!")
Log.notice("hello!")
Log.warning("hello!")
Log.error("hello!")
Log.critical("hello!")
Log.alert("hello!")
Log.emergency("hello!")
```

Messages below the configured minimum `level` of the channel are ignored.

## Choosing the Channel on the Fly

You can log to a specific channel with `channel()`:

```python
Log.channel("daily").info("Some message")
```

You can also stack several channels for a single message:

```python
Log.stack("console", "daily", "slack").critical("Some message")
```

## On-demand Channels

You can build a channel on the fly without declaring it in the configuration by using `build()` with a driver and its options:

```python
Log.build("daily", {"path": "logs/audit.log", "format": "{message}"}).info("On demand!")
```

# Exception Logging

When the `LoggingProvider` is registered, Masonite listens to the `masonite.exception.*` events fired by the exception handler. Every unhandled exception is automatically logged as an `error` with its type, message and **full traceback**:

```
2026-01-01 10:00:00 - ERROR: ZeroDivisionError: division by zero
Traceback (most recent call last):
  File "app/controllers/WelcomeController.py", line 12, in show
    1 / 0
ZeroDivisionError: division by zero
```

You can silence exception logging (for example in a specific environment) by raising the default channel level above `error`:

```python
# config/logging.py
CHANNELS = {
    "default": {
        # ...
        "level": "critical",
    },
}
```
