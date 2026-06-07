# Validation

Every Field has their own rules implementation to manage how should be treated in the front, you can see more on each [Field](fields/overview.md) page

### Rules

To set rules, use the `rules` method.

```python
TextField("Name", "name").rules("required", "max:40")
```

### Create rules

To set rules that will appear on `create`, use the `create_rules` method.

```python
Password("Password").create_rules("required", "min:8", "max:40")
```

### Update rules

To set rules that will appear on `update`, use the `update_rules` method.

```python
Password("Password", "password").update_rules("nullable", "min:8", "max:40")
```
