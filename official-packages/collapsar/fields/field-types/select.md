# Select

## Overview
Use `Select` to predefine a list of values.


```python
from collapsar.fields.Select import Select

Select('Language').options([
    'English',
    'Spanish',
])
```

### Make field

Collapsar will assume the field if you pass only pass 1 parameter. You can define the label and field this way:

```python
from collapsar.fields.Select import Select

Select('Role', 'role_type').options([
    'user',
    'mod',
    'admin'
])
```

## Defining keys

If you want to set a custom label for your option, you can set it using a list of dict.

```python
from collapsar.fields.Select import Select

SelectField("Department", "department")
.options(
    [
        {"label": "IT", "value": 1},
        {"label": "Marketing", "value": 2},
        {"label": "Sales", "value": 3},
    ]
)
```
