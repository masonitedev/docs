# Quickstart

Collapsar is a package that will let you save time creating a dashboard for your app. You won't need to worry anymore about creating CRUD's.

!!! danger
    Collapsar is in early development stage and it isn't ready for production environments.

**Main features**

- **Resources**: Create multiple resources to your dashboard using your Masonite Models.
- **Use fields**: Choose between a set of **Fields** components to create your dashboard.
- **Add validations**: You can configure rules to validate user input directly from your **Resource** definition.

## Prerequisites

Before we start, make sure you have the following installed:

- Masonite 4 or later.
- Python3.
- Python3 Pip.

## Install Collapsar

Go to your project folder and run the following [pip](https://pip.pypa.io/en/stable/installation/) command.

```bash
pip install collapsar
```

Add CollapsarProvider to your project in `config/providers.py`:

```python
# config/providers.py
# ...
from collapsar import CollapsarProvider

# ...
PROVIDERS = [
    # ...
    # Third Party Providers
    CollapsarProvider,
    # ...
]
```

## Install Inertia

Collapsar depends on `inertia-masonite` package. If you have it installed, skip this step.

To install the Inertia adapter follow the instructions [provided here.](https://github.com/eaguad1337/masonite-inertia)

## Creating a user

To enter your dashboard, you should visit `/collapsar` and a login page will prompt. Create an user to login or use an existing one. Collapsar uses the model "User" to authenticate you.

If you need to create an user:

```python
python craft collapsar:user
```

## Configure the gate

You may give access to certain kind of users to your app. To do so, you can specify what is the Authentication model and also define a callback to determine if the user that will be authenticated can have access to your dashboard.

Edit your `config/collapsar.py` file in order to modify USER_MODEL or GATE function.

Pzzt: the RESOURCES variable will indicate which Resources will be available on your sidebar. It receives a list of Resources.

```python
"""Collapsar Settings"""
from app.models.User import User

ROUTE_PREFIX = "collapsar"

RESOURCES = [
    # Add your resources
]

USER_MODEL = User

def gate(user):
     return user.email in []

GATE = gate
```

## Create a resource

Use the following command to create your resources, where `MyModel` is the class name of your [Masonite Model](../../orm/models.md).

```bash
python craft resource MyModel
```

Your new resource will be placed in `app/collapsar/resources/MyModelResource.py` with a base configuration.

```python
from collapsar import Resource
from collapsar.IdField import IdField

from app.models.MyModel import MyModel

class MyModelResource(Resource):
    """MyModel Resource."""

    title = "MyModel"
    model = MyModel

    @classmethod
    def fields(cls):
        """Return the fields of the resource."""

        return [
            IdField("Id", "id").readonly().sortable(),
        ]
```

After you created your model, add it to your sidebar:

```python hl_lines="3 8"
"""Collapsar Settings"""
from app.models.User import User
from app.collapsar.MyModelResource import MyModelResource # import the class

ROUTE_PREFIX = "collapsar"

RESOURCES = [
    MyModelResource, # Add the class here
]

USER_MODEL = User

def gate(user):
     return user.email in []

GATE = gate
```

Great! From now on, you can add [Fields](fields/overview.md), [Validations](validation.md) or define your Field [Visibility](fields/visibility.md)

To see your dashboard just run your Masonite Server with `python craft serve` and visit [https://localhost:8000/collapsar](https://localhost:8000/collapsar)
