# Spider Chef 🕷️👨‍🍳
[![PyPI](https://img.shields.io/pypi/v/spiderchef)](https://pypi.org/project/spiderchef/)
[![Python Versions](https://img.shields.io/pypi/pyversions/spiderchef)](https://pypi.org/project/spiderchef/)
[![Ruff](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/ruff/main/assets/badge/v2.json)](https://github.com/astral-sh/ruff)
[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white)](https://github.com/pre-commit/pre-commit)
[![Coverage Status](./coverage-badge.svg?dummy=8484744)](./coverage.xml)
[![Documentation Status](https://readthedocs.org/projects/spiderchef/badge/?version=latest)](https://spiderchef.readthedocs.io/en/latest/?badge=latest)
```
                   /\
                  /  \
                 |  _ \                   _
                 | / \ \   .--,--        / \
                 |/   \ \  `.  ,.'      /   \
                 /     \ |  |___|  /\  /     \
                /|      \|  ~  ~  /  \/       \
        _______/_|_______\ (o)(o)/___/\_____   \
       /      /  |        (______)     \    \   \_
      /      /   |                      \    \
     /      /    |                       \    \
    /      /     |                        \    \
   /     _/      |                         \    \
  /             _|                          \    \_
_/                                           \_                                               
```

SpiderChef is a powerful, recipe-based web scraping tool that makes data extraction systematic and reproducible. By defining scraping procedures as "recipes" with sequential "steps," SpiderChef allows you to craft elegant, maintainable data extraction workflows.

## Features

- Recipe-Based Architecture: Define extraction workflows as YAML recipes
- Modular Step System: Build complex scraping logic from reusable components
- Async Support: Handle both synchronous and asynchronous extraction steps
- Type Safety: Fully typed for better development experience
- Extensible Design: Easily create custom steps for specialized extraction needs

## Installation

```bash
# If you want to use the cli
pip install spiderchef[cli]

# If you just want the library usage
pip install spiderchef
```

## CLI Usage

```bash
# Run a recipe
spiderchef cook recipes/example.yaml

# Create a new recipe template
spiderchef recipe new my_extraction
```


## Library Usage

### Basic Usage
The basic usage of this library involves just pulling a local recipe and "cooking" it to get the output data:
```python
import asyncio
from spiderchef import Recipe

# Imports a recipe from a yaml file locally
recipe = Recipe.from_yaml('recipe_example.yaml')
# Run a recipe
asyncio.run(recipe.cook())
```
## Example Recipe

```yaml
base_url: https://example.com
name: ProductExtractor
steps:
  - type: fetch
    name: fetch_product_page
    page_type: text
    path: /products
    params:
      category: electronics
      sort: price_asc
  
  - type: regex
    name: extract_product_urls
    expression: '"(\/product\/[^"]+)"'
  
  - type: join_base_url
    name: format_urls
```

### Custom Usage
Let's say you want to extend the steps available even more with your own custom ones, you can do it like so:
```python
import asyncio
from typing import Any

from spiderchef import STEP_REGISTRY, AsyncStep, Recipe, SyncStep


# You can define your own custom steps like so:
class HelloStep(SyncStep):
    # .name is a reserved keyword for steps
    person_name: str

    def _execute(self, recipe: Recipe, previous_output: Any = None) -> str:
        return f"Hello There {self.person_name}"


# Sync or Async is possible.
class SleepStep(AsyncStep):
    sleep_time: int = 5

    async def _execute(self, recipe: Recipe, previous_output: Any = None) -> Any:
        await asyncio.sleep(self.sleep_time)
        return previous_output


CUSTOM_STEP_REGISTRY = {**STEP_REGISTRY, "hello": HelloStep, "sleep": SleepStep}

# Overrides the global step registry with your own
Recipe.step_registry = CUSTOM_STEP_REGISTRY

# You can manually initialise a recipe like so, or just use the yaml recipe.
recipe = Recipe(
    base_url="https://example.com",
    name="Example",
    steps=[
        HelloStep(name="Saying Hello", person_name="George"),
        SleepStep(
            name="Sleeping",
        ),
    ],
)

# Run a recipe
asyncio.run(recipe.cook())

"""Output:
2025-05-07 16:33:01 [info     ] 🥣🥄🔥 Cooking 'Example' recipe!
2025-05-07 16:33:01 [info     ] ➡️  1. Saying Hello...         step_class=HelloStep
2025-05-07 16:33:01 [info     ] ➡️  2. Sleeping...             step_class=SleepStep
2025-05-07 16:33:06 [info     ] 🍞 'Example' recipe finished output='Hello There George'
"""
```


## Variable Replacement
SpiderChef supports variable replacement in your steps using the `${variable}` syntax. Variables can be defined in the Recipe and will be automatically replaced when the step is executed:

```python
recipe = Recipe(
    name="Variable Example",
    base_url="https://example.com"
    # Default variables
    variables={
        "sort_order": "price_asc",
        "category": "smartphones"
    },
    steps=[
        # Variables are replaced automatically before execution
        FetchStep(
            name="Search Products",
            path="/products"
            params={
              "category":"${category}"
              "sort":"${sort_order}"
            }
        )
    ]
)
# Uses default variables
await recipe.cook()

# Replace a specific variable, making any recipe extendable
await recipe.cook(category="books")
```
In YAML recipes, you can use the same syntax:
```yaml
name: ProductExtractor
base_url: https://example.com
variables: # these are defaults
  category: electronics
  sort_order: price_asc
steps:
  - type: fetch
    name: fetch_product_page
    page_type: text
    path: /products
    params:
      category: ${category}
      sort: ${sort_order}
```
You can even save variables within the recipe to be used later using the save step.
```yaml
name: ProductExtractor
base_url: https://example.com
variables:
  category: electronics
  sort_order: price_asc
steps:
  - type: fetch
    name: fetch_product_page
    path: /products
    params:
      category: ${category}
      sort: ${sort_order}
  - type: xpath
    name: extract_title
    expression: //h1
  - type: save
    variable: title
```

## Why SpiderChef?
Traditional web scraping often involves writing complex, difficult-to-maintain code that mixes HTTP requests, parsing, and business logic. SpiderChef separates these concerns by:

- Breaking extraction into discrete, reusable steps
- Defining workflows as declarative recipes
- Handling common extraction patterns with built-in steps
- Making scraping procedures reproducible and maintainable

Whether you're scraping product data, monitoring prices, or extracting research information, SpiderChef helps you build structured, reliable data extraction pipelines.


## Documentation
For full documentation, visit [spiderchef.readthedocs.io](https://spiderchef.readthedocs.io).

The documentation includes:
- Getting started guide
- User guides for basic and advanced usage
- API reference
- Tutorials and examples
- Contributing guidelines

To build the documentation locally:
```bash
make docs
```

## License

[MIT License](LICENSE)

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.