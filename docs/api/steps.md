# Steps API

Steps are the building blocks of SpiderChef recipes. This reference documents the core step classes and the built-in step types available in SpiderChef.

## Core Step Classes

### Base Step Class

::: spiderchef.steps.base.BaseStep
    handler: python
    options:
      show_source: true
      show_bases: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3

### Synchronous Step

::: spiderchef.steps.base.SyncStep
    handler: python
    options:
      show_source: true
      show_bases: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3

### Asynchronous Step

::: spiderchef.steps.base.AsyncStep
    handler: python
    options:
      show_source: true
      show_bases: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3

## HTTP Steps

### Fetch Step

::: spiderchef.steps.asynchronous.FetchStep
    handler: python
    options:
      show_source: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3

### Sleep Step

::: spiderchef.steps.asynchronous.SleepStep
    handler: python
    options:
      show_source: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3

## Extraction Steps

### Regex Step

::: spiderchef.steps.extract.RegexStep
    handler: python
    options:
      show_source: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3

### Regex First Step

::: spiderchef.steps.extract.RegexFirstStep
    handler: python
    options:
      show_source: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3

### XPath Step

::: spiderchef.steps.extract.XpathStep
    handler: python
    options:
      show_source: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3

### XPath First Step

::: spiderchef.steps.extract.XpathFirstStep
    handler: python
    options:
      show_source: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3

### Get (Pydash) Step

::: spiderchef.steps.extract.GetStep
    handler: python
    options:
      show_source: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3

### Extract Items Step

::: spiderchef.steps.extract.ExtractItemsStep
    handler: python
    options:
      show_source: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3

## Transformation Steps

### Join Base URL Step

::: spiderchef.steps.format.JoinBaseUrl
    handler: python
    options:
      show_source: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3

### From JSON Step

::: spiderchef.steps.format.FromJson
    handler: python
    options:
      show_source: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3

### To Int Step

::: spiderchef.steps.format.ToInt
    handler: python
    options:
      show_source: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3

### To String Step

::: spiderchef.steps.format.ToStr
    handler: python
    options:
      show_source: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3

### To Float Step

::: spiderchef.steps.format.ToFloat
    handler: python
    options:
      show_source: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3

### To Money Step

::: spiderchef.steps.format.ToMoneyStep
    handler: python
    options:
      show_source: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3

### Remove Extra Whitespace Step

::: spiderchef.steps.format.RemoveExtraWhitespace
    handler: python
    options:
      show_source: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3

### Remove HTML Tags Step

::: spiderchef.steps.format.RemoveHTMLTags
    handler: python
    options:
      show_source: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3

## Flow Control Steps

### Compare Step

::: spiderchef.steps.conditional.CompareStep
    handler: python
    options:
      show_source: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3

## Data Management Steps

### Save Step

::: spiderchef.steps.base.SaveStep
    handler: python
    options:
      show_source: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3

## Error Handling Steps

### Try Catch Step

::: spiderchef.steps.error.TryCatchStep
    handler: python
    options:
      show_source: true
      show_root_heading: true
      show_object_full_path: false
      heading_level: 3
