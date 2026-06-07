# Masonite 5.4

Masonite 5.4 splits the framework kernel into two layers, giving you a bare-metal `CoreKernel` to build leaner custom boots on top of, and makes importing the foundation package noticeably lighter.

## A Minimal Kernel with `CoreKernel`

The standard `Kernel` has always registered everything a full application needs, including the complete suite of craft commands. That is the right default — but it is more than a queue worker, a slimmed-down service, or an embedded use of the framework actually needs.

Masonite 5.4 introduces `CoreKernel`: a minimal root kernel that registers only the essentials — it loads your environment, sets the response handler and storage path, and binds the router, the middleware capsule, the loader, and an empty command registry. No built-in commands, no testing helpers.

Build your own root kernel on top of it and register only what you need:

```python title="app/Kernel.py"
from masonite.foundation import CoreKernel


class Kernel(CoreKernel):
    def register(self) -> None:
        super().register()
        # register only the commands and services this application needs
```

The standard `Kernel` now extends `CoreKernel`, adding the built-in craft commands — and, only when running under the test runner, the test response capsule. Both kernels are exported from `masonite.foundation`. See [Request Lifecycle](../architecture/request-lifecycle.md#building-a-custom-kernel).

## Lighter Imports

Every command and testing import inside the kernels is now loaded lazily. Importing `masonite.foundation` no longer pulls in cleo, the full command suite, or the testing helpers — a custom kernel only pays for what it actually registers, and production boots never import the testing helpers at all.

## Upgrading from 5.3

Masonite 5.4 is a drop-in upgrade — the standard `Kernel` your project ships with behaves exactly as before, and no changes are required.

One behavior change is worth knowing about:

* **The test response capsule is registered lazily.** The `tests.response` binding is now wired up only when the application is running under the test runner, instead of on every boot. Your tests are unaffected. If you depended on that binding outside of a test run, register it yourself in a service provider.

## Patch Releases

### 5.4.1

* **Package providers resolve their root reliably.** A package whose name appeared in the project's filesystem path — an app created in a directory like `myapp-collapsar`, or an editable install from a repository folder named after the package — could crash on boot with `views() first argument must be a folder containing all package views.` `PackageProvider.root()` now resolves the package directory through Python's import system instead of matching on the file path, so the location is found correctly regardless of where the project lives.
