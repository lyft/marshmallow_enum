# marshmallow-enum

Enum field for use with Marshmallow.

## Installation

```bash
pip install --user marshmallow_enum
```

If you're on a version before 3.4, you'll also need to install `enum34`.

## Using The Field

To make use of the field, you must have an existing Enum:

```python
from enum import Enum


class StopLight(Enum):
    green = 1
    yellow = 2
    red = 3
```

Then, declare it as a field in a schema:

```python
from marshmallow import Schema
from marshmallow_enum import EnumField


class TrafficStop(Schema):
    light_color = EnumField(StopLight)
```

By default, the field will load and dump based on the _name_ given to an enum value.

```python
schema = TrafficStop()
schema.dump({'light_color': EnumField.red}).data
# {'light_color': 'red'}

schema.load({'light_color': 'red'}).data
# {'light_color': StopLight.red}
```

### Customizing loading and dumping behavior

To customize how an enum is serialized or deserialized, there are three options:

-   Setting `by_value=True`. This will cause both dumping and loading to use the value of the enum.
-   Setting `load_by=EnumField.VALUE`. This will cause loading to use the value of the enum.
-   Setting `dump_by=EnumField.VALUE`. This will cause dumping to use the value of the enum.

If either `load_by` or `dump_by` are unset, they will follow from `by_value`.

Additionally, there is `EnumField.NAME` to be explicit about the load and dump behavior, this
is the same as leaving both `by_value` and either `load_by` and/or `dump_by` unset.

### Custom Error Message

A custom error message can be provided via the `error` keyword argument. It can accept three
format values:

-   `{input}`: The value provided to the schema field
-   `{names}`: The names of the individual enum members
-   `{values}`: The values of the individual enum members

Previously, the following inputs were also available but are deprecated and will be removed in 1.6:

-   `{name}`
-   `{value}`
-   `{choices}`
