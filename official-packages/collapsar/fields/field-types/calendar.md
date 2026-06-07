# Calendar

## Overview
Use `Calendar` to control date fields.


```python
from collapsar.fields.Calendar import Calendar

Calendar('Name')
```

### Make field

Collapsar will assume the field if you pass only pass 1 parameter. You can define the label and field name this way:

```python
from collapsar.fields.Calendar import Calendar

Calendar('Updated at', 'updated_at')
```

### Fill method

Use `fill` to control the way the field will be assigned to the model. For example, to save it on another format.

```python
def fill(self, request, model: "Model"):
    """Fill the field"""
    setattr(
        model,
        self.attribute,
        datetime.strptime(request.input(self.attribute), "%Y-%m-%dT%H:%M:%S.%fZ"),
```

### Format date

Use `format_value` to display the date on a different format.

```python
def format_value(self, value: "datetime"):
    """Format the value for display."""
    return value.strftime("%Y-%m-%d") if value else None
```
