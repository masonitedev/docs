# Password

## Overview
Use `Password` to control secret values like passwords or keys.


```python
from collapsar.fields.Password import Password

Password('Password')
```

### Make field

Collapsar will assume the field if you pass only pass 1 parameter. You can define the label and field name this way:

```python
from collapsar.fields.TextInput import TextInput

Password('Secret', 'secret_key')
```

### Hashing

Password fields automatically Hashes the value on create or update. You can customize this behaviour overriding the `fill` method.

```python
# collapsar.fields.Password.py

from masonite.facades.Hash import Hash

def fill(self, request, model: "Model"):
    """Fill the field"""
    setattr(model, self.attribute, Hash.make(request.input(self.attribute)))

    return None
```

!!! info
    For advanced validations see [Validations](../../validation.md).
