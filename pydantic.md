# Pydantic Tricks

## Constrained Lists

- Add type checking with additional constraints
- Enforce min or max number of items in list
- validate list is not empty and list are of a specific type

```python
from typing import Annotated
from pydantic import BaseModel, Field

class MyModel(BaseModel):
    numbers: Annotated[list[int], Field(min_length=1, max_length=5)]
```

## Custom Validator

- API or Data Processing needs to validate if incoming data meets certain criteria

| Mode      | When to Use |
|-----------|-------------|
| "after"   | When you want to validate or modify data after Pydantic has done its initial parsing and type conversion. |
| "before"  | When you need to preprocess raw input data before Pydantic's validation. |
| "wrap"    | When you need the most control over the validation process, allowing you to intervene before and after Pydantic's validation. |
| "plain"   | When you want to completely replace Pydantic's validation with your own logic. |

```python
    @model_validator(mode="after")
    def validate_input_fields(self) -> Self:
        if not any([self.resolution, self.root_cause, self.diagnostic_steps]):
            raise ValueError("Field 'resolution', 'root_cause' or 'diagnostic_steps' is required.")
        return self
```
