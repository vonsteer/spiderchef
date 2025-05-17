# Recipe API

The `Recipe` class is the central component of SpiderChef, representing a complete web scraping workflow.

## Core Recipe Class

::: spiderchef.recipe.Recipe
    handler: python
    options:
      show_source: true
      show_bases: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 2

## Creating Recipes

### From YAML

The most common way to create a recipe is from a YAML file:

```python
from spiderchef import Recipe

# Load a recipe from a YAML file
recipe = Recipe.from_yaml("my_recipe.yaml")
```

### Programmatically

You can also create recipes directly in code:

```python
from spiderchef import Recipe
from spiderchef.steps import FetchStep, XPathStep

recipe = Recipe(
    name="ProductScraper",
    base_url="https://example.com",
    variables={
        "category": "electronics"
    },
    steps=[
        FetchStep(
            name="fetch_page",
            path="/products",
            params={"category": "${category}"}
        ),
        XPathStep(
            name="extract_titles",
            expression="//h2[@class='product-title']/text()"
        )
    ]
)
```

## Running Recipes

To run a recipe, use the `cook` method:

```python
import asyncio

# Run with default variables
result = asyncio.run(recipe.cook())

# Run with custom variables
result = asyncio.run(recipe.cook(category="books", max_pages=3))
```

## Recipe Variables

Recipe variables allow you to parameterize your recipes:

```python
# Define variables when creating the recipe
recipe = Recipe(
    name="Search",
    base_url="https://example.com",
    variables={
        "query": "default search",
        "limit": 10
    },
    # ... steps ...
)

# Override variables when running the recipe
result = asyncio.run(recipe.cook(query="new search", limit=20))
```

## Step Registry

SpiderChef uses a step registry to map step types (as used in YAML) to their corresponding Python classes:

```python
from spiderchef import STEP_REGISTRY, Recipe

# View all available step types
print(STEP_REGISTRY)

# Set a custom step registry for a specific recipe
recipe = Recipe(
    # ... recipe parameters ...
)
recipe.step_registry = custom_registry

# Set a custom step registry for all recipes
Recipe.step_registry = custom_registry
```

## Recipe Context

During execution, a recipe maintains a context that includes:

- Variables defined in the recipe
- Variables overridden during execution
- Variables saved during execution using save steps

You can access this context in custom steps:

```python
class CustomStep(SyncStep):
    def _execute(self, recipe: Recipe, previous_output=None):
        # Access a variable from the recipe context
        category = recipe.variables.get("category")
        return f"Category: {category}"
```

## Example Workflows

### Basic Scraping

```python
from spiderchef import Recipe

recipe = Recipe.from_yaml("basic_scraper.yaml")
result = asyncio.run(recipe.cook())
```

### Parameterized Scraping

```python
from spiderchef import Recipe

recipe = Recipe.from_yaml("product_scraper.yaml")

# Scrape electronics products
electronics = asyncio.run(recipe.cook(category="electronics"))

# Scrape book products
books = asyncio.run(recipe.cook(category="books"))
```

### Dynamic Configuration

```python
from spiderchef import Recipe

recipe = Recipe.from_yaml("search_scraper.yaml")

# Configure search parameters
search_params = {
    "query": "python programming",
    "sort": "relevance",
    "limit": 20
}

# Run with dynamic configuration
result = asyncio.run(recipe.cook(**search_params))
```
