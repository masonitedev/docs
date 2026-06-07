# Masonite 5.3

Masonite 5.3 ships a long-requested Redis queue driver, moves new projects to Vite + Tailwind CSS 4, and clears **every known vulnerability** in the dependency tree — `pip-audit` reports zero findings.

## Redis Queue Driver

Queues can now run on Redis with the same API as every other driver:

```python title="config/queue.py"
DRIVERS = {
    "default": "redis",
    "redis": {
        "host": env("REDIS_HOST", "127.0.0.1"),
        "port": env("REDIS_PORT", "6379"),
        "password": env("REDIS_PASSWORD", ""),
        "failed_table": "failed_jobs",
        "attempts": 3,
        "poll": 1,
    },
}
```

```python
application.make("queue").push(SendWelcomeEmail(), driver="redis")
```

```text title="terminal"
$ python craft queue:work --driver redis
```

Delayed jobs are held in a Redis sorted set and promoted to the queue exactly when they come due, failed jobs respect `attempts` and land in the failed-jobs table, `queue:retry` re-enqueues them, and a payload that cannot be deserialized is discarded with a warning instead of bringing the worker down.

Also fixed along the way: `queue.push(job, delay="10 minutes")` now works through the public API for **every** driver — the `delay` option used to be silently dropped.

Thanks to ReS4 for the original driver implementation.

## Vite and Tailwind CSS 4

New projects build their frontend with [Vite](https://vite.dev) and [Tailwind CSS 4](https://tailwindcss.com), replacing the legacy `laravel-mix` webpack pipeline:

```text title="terminal"
$ npm install
$ npm run watch   # or: npm run production
```

`npm audit` on a freshly generated project goes from 12 findings to **zero**. All four frontend presets — Tailwind, Vue 3, React and Bootstrap — were ported to Vite. Existing Mix-based projects keep working and the `mix()` template helper remains available. See [Compiling Assets](../the-basics/compiling-assets.md).

## A Clean Security Audit

Masonite 5.3 closes out a full dependency audit:

* **craft runs on cleo 2** — the CLI framework jumped three major versions, removing exposure to CVE-2022-42966. Your custom commands need **no changes**: the docstring signature format is parsed by Masonite itself now.
* **cryptography 46+** — unblocks the fixes for three CVEs.
* **pytest is no longer a production dependency** — install it with `pip install masonite-framework[test]`.
* **`strong(breach=True)` works out of the box** — the validation rule calls the [Have I Been Pwned](https://haveibeenpwned.com/Passwords) k-anonymity API directly with `requests`; only the first 5 characters of the SHA-1 hash ever leave your server. See [Validation](../the-basics/validation.md#strong).
* **Unmaintained packages removed** — `hashids` (superseded by Sqids), `hfilesize` (vendored) and `pwnedapi` (replaced) are gone from the dependency tree, and the skeleton's `axios` moved to the 1.x line.

## Upgrading from 5.2

Masonite 5.3 requires `masonite-framework-orm >= 3.1`, which is installed automatically.

There are three behavior changes to review:

* **Hash IDs are generated with [Sqids](https://sqids.org)** — the successor of Hashids. Generated values differ from previous versions. Hashed IDs are ephemeral by design (encoded into a response, decoded on the next request), so this only affects hashes you persisted across the upgrade. See [Hash ID's](../digging-deeper/hashid.md).
* **The Vonage notification driver uses the v4 SDK** — run `pip install "vonage>=4"` if you send SMS notifications. Your `to_vonage` notifications are unchanged.
* **`masonite.tests.TestCase` requires pytest explicitly** — it ships with the `test` extra: `pip install masonite-framework[test]`.
