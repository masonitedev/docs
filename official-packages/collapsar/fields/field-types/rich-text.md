# RichText

## Overview
Use `RichText` create a WYSIWYG component based on [TipTap](https://tiptap.dev/).


```python
from collapsar.fields.RichText import RichText

RichText('Name')
```

### Make field

Collapsar will assume the field if you pass only pass 1 parameter. You can define the label and field name this way:

```python
from collapsar.fields.TextInput import TextInput

RichText('Content', 'article_content')
```

### Computed values

You may use provide a function if you want to render a custom value.

```python
from collapsar.fields.RichText import RichText

RichText('Body', lambda model: f'<strong>Name:</strong>: {model.name}')
```

!!! info
    Computed values cannot be edited and won't be shown on the creation form.
