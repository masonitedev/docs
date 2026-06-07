Form requests are dedicated classes that authorize and validate an incoming request before your controller runs. Instead of validating inside every controller method, you move the rules — and the authorization check — into a small, reusable class and simply type-hint it in your controller. If the request is not authorized or the data is invalid, the controller body never executes.

# Creating a Form Request

Generate one with the `request` command:

```text title="terminal"
$ python craft request StorePostRequest
```

This creates `app/requests/StorePostRequest.py`:

```python title="app/requests/StorePostRequest.py"
from masonite.request import FormRequest
from masonite.validation import required


class StorePostRequest(FormRequest):
    def authorize(self) -> bool:
        return True

    def rules(self) -> list:
        return [
            required(["example"]),
        ]

    def messages(self) -> dict:
        return {}
```

# Using a Form Request

Type-hint the form request on a controller method. The container builds it, runs `authorize()` and then validates the input against `rules()` — all before your method body runs:

```python
from masonite.validation import required, email


class StorePostRequest(FormRequest):
    def rules(self):
        return [
            required(["title", "author_email"]),
            email("author_email"),
        ]


class PostController(Controller):
    def store(self, request: StorePostRequest):
        # Only reached when authorization and validation both pass.
        data = request.validated()
        ...
```

## Rules

`rules()` returns a list of [validation rules](validation.md#available-rules), exactly like the rules you would pass to `request.validate()`. Custom messages are returned from `messages()`, keyed by `field_rule`:

```python
def messages(self):
    return {"title_required": "A title is required"}
```

## Authorization

`authorize()` returns a boolean. Return `False` to block the request — the framework raises an authorization error and renders a `403` response, so the controller never runs. Use it to check the authenticated user or a [gate or policy](../security/authorization.md):

```python
def authorize(self):
    return self.user() is not None
```

By default `authorize()` returns `True`.

## Accessing input

A form request exposes convenience accessors that delegate to the underlying request:

| Method | Description |
| --- | --- |
| `request.all()` | All incoming input. |
| `request.input("key")` | A single input value. |
| `request.user()` | The authenticated user. |
| `request.validated()` | Only the inputs that were covered by a rule. |

`validated()` is the safe payload to hand to a model or query — it contains just the fields you declared rules for, with everything else stripped out.

# Handling Failure

When validation fails, the framework chooses the response based on what the client accepts:

- **Standard requests** are redirected back to the previous page with the errors flashed to the session (and the old input preserved), so your templates can display them through the [`errors` message bag](validation.md#displaying-errors-in-views) — the same flow as `response.back().with_errors()`.
- **Requests that accept JSON** receive a `422 Unprocessable Entity` response with the error bag:

```json
{
    "message": "The given data was invalid.",
    "errors": {
        "title": ["The title field is required."],
        "author_email": ["The author_email must be a valid email address."]
    }
}
```

No `if errors:` branching is needed in your controller — reaching the method body means the request already passed.
