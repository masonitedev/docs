# Boolean

## Overview
With `Boolean` you can easily render booleans with a toggle component.


```python
from collapsar.fields.Boolean import Boolean

Boolean('Active')
```

### Make field

Collapsar will assume the field if you pass only pass 1 parameter. You can define the label and field name this way:

```python
from collapsar.fields.TextInput import TextInput

Boolean('Admin', 'is_admin')
```

### Computed values

You may use provide a function if you want to render a custom value.

```python
from collapsar.fields.Boolean import Boolean

Boolean('Has name', lambda model: model.name != "")
```

!!! info
    Computed values cannot be edited and won't be shown on the creation form.
