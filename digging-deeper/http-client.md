Masonite ships a small, fluent HTTP client for talking to other services. It wraps the `requests` library with an expressive API, sensible defaults, and a built-in way to fake outbound calls in your tests.

# Making Requests

Resolve the client through the `Http` facade and call one of the verb methods. Each returns a response object:

```python
from masonite.facades import Http

response = Http.get("https://example.com/api/users")

response.json()      # decoded JSON body
response.text()      # raw body as a string
response.status()    # status code
response.ok()        # True for 2xx
```

The available verbs are `get`, `post`, `put`, `patch` and `delete`:

```python
Http.post("https://example.com/api/users", json={"name": "Sam"})
Http.get("https://example.com/api/users", query={"page": 2})
Http.delete("https://example.com/api/users/1")
```

`json=` sends a JSON body, `data=` sends form-encoded data, and `query=` adds query-string parameters.

# Configuring the Request

The fluent methods return a pending request you can chain before sending:

```python
Http.with_headers({"X-Trace": "abc"}).get(url)

# Bearer token (the prefix is configurable)
Http.with_token("my-token").get(url)

# Basic auth
Http.with_basic_auth("user", "secret").get(url)

# Timeout in seconds (defaults to 30)
Http.timeout(5).get(url)

# Retry on connection errors, optionally sleeping between attempts
Http.retry(3, sleep=1).get(url)
```

These can be combined:

```python
Http.with_token("my-token").timeout(10).retry(2).post(url, json=payload)
```

# Inspecting the Response

```python
response = Http.get(url)

response.status_code        # 200
response.ok()               # True for 2xx (alias: successful())
response.failed()           # True for non-2xx
response.client_error()     # True for 4xx
response.server_error()     # True for 5xx
response.header("Content-Type")
response.json()
response.text()
response.body()             # raw bytes
```

# Testing

Call `Http.fake()` to stop real requests from going out. Map a URL substring (or `"*"` as a catch-all) to a stub response built with `Http.response()`:

```python
from masonite.facades import Http


def test_fetches_users(self):
    Http.fake(
        {
            "example.com/api/users": Http.response({"data": []}, status=200),
            "*": Http.response("not found", status=404),
        }
    )

    # ... code under test calls Http.get("https://example.com/api/users") ...

    Http.assert_sent("example.com/api/users")
    Http.assert_sent_count(1)
```

`Http.response(body, status=200, headers=None)` accepts a dict or list (encoded as JSON), a string, or bytes. When faking, every call is recorded so you can assert what was sent:

| Method | Description |
| --- | --- |
| `Http.assert_sent(url)` | A request was sent to a URL containing this substring. |
| `Http.assert_sent_count(n)` | Exactly `n` requests were sent. |
| `Http.assert_nothing_sent()` | No requests were sent. |
| `Http.recorded()` | The list of recorded requests (method, url, params, data, json, headers). |
| `Http.stop_faking()` | Turn faking back off. |
