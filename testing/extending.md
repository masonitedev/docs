# Extending

As your test suite grows you will find yourself repeating the same setup steps and assertions across many tests. The cleanest way to remove that duplication is to extend your project's base test case with your own helpers and assertions.

## Your Project's TestCase

Every new Masonite application ships with a project-level test case in `tests/TestCase.py`:

```python
from masonite.tests import TestCase as MasoniteTestCase


class TestCase(MasoniteTestCase):
    def setUp(self):
        super().setUp()
```

All of your tests should inherit from this class instead of importing Masonite's `TestCase` directly. That way, anything you add here — helpers, assertions, default state — is instantly available to your whole suite:

```python
from tests.TestCase import TestCase


class TestWelcome(TestCase):
    def test_welcome_page(self):
        self.get("/").assertOk().assertContains("Masonite")
```

## Adding Helpers

A helper is just a method on your test case. A very common one is logging in a specific kind of user:

```python
from masonite.tests import TestCase as MasoniteTestCase

from app.models.User import User


class TestCase(MasoniteTestCase):
    def actingAsAdmin(self):
        admin = User.where("is_admin", 1).first()
        return self.actingAs(admin)
```

Then in any test:

```python
def test_admin_can_see_dashboard(self):
    self.actingAsAdmin()
    self.get("/admin").assertOk()
```

## Adding Custom Assertions

Masonite's assertions are chainable because each one returns the test case (or the HTTP test response) itself. Follow the same convention in your own assertions — perform the check, then `return self`:

```python
class TestCase(MasoniteTestCase):
    def assertDatabaseHasUser(self, email):
        from app.models.User import User

        self.assertIsNotNone(
            User.where("email", email).first(),
            f"No user found with email {email}",
        )
        return self
```

Because the base class is `unittest.TestCase` under the hood, you can build on every standard `unittest` assertion (`assertEqual`, `assertIn`, `assertRaises`, ...) inside your custom ones.

## Default Setup For The Whole Suite

Use `setUp()` in your project test case to apply state to every test — for example registering extra [test routes](getting-started.md) or default [headers](http-tests.md):

```python
from masonite.routes import Route
from masonite.tests import TestCase as MasoniteTestCase


class TestCase(MasoniteTestCase):
    def setUp(self):
        super().setUp()
        self.withHeaders({"X-Requested-With": "XMLHttpRequest"})
        self.addRoutes(
            Route.get("/testing-only", "WelcomeController@show"),
        )
```

!!! info

    Always call `super().setUp()` first — it boots the application and prepares the container before your customizations run.

If you need several specialized base classes (for example one that prepares an [authenticated user](../security/authentication.md) and one for [database-heavy tests](database-tests.md)), create them as subclasses of your project `TestCase` so they all share the same foundation.
