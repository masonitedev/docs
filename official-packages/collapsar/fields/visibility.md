# Visibility

By default, a `Field` will be added to index, show, create and update views.

### hide_from_index()

Use this method to hide the field from `index` view.

```python
Password("Password", "password").hide_from_index(),
```

### hide_from_show()

Use this method to hide the field from `show` view.

```python
Password("Password", "password").hide_from_show(),
```

### hide_from_create()

Use this method to hide the field from `create` view.

```python
Id("Id").hide_from_create(),
```

### hide_from_update()

Use this method to hide the field from `update` view.

```python
Calendar("updated_at").hide_from_update(),
```
