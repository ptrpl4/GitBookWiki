# 🐹 YAML

YAML Ain't Markup Language

Human-readable data serialization standard for all programming languages.

### Basic data types

* _integers_ like 15, 123
* _strings_ like "15", 'Hello, YAML!', which may be enclosed either in double or single quotation marks
* _floats_ like 15.033
* _booleans_ (true or false)
* _null type_ (null)

YAML auto-detects the type of data, but users can specify the type they need using `!!`.

`!!str yes`

#### comments

```yaml
# basic
# comments

published:
  year: 1889 # inline comments
  country: United Kingdom

```

### Maps

Mapping consists of key-value pairs. Pairs are called _scalars._

Indentation is always done with **spaces**, not tabs. Number of spaces doesn't matter, as long as the child node indentation is greater that its parent. Recommended indentation for YAML files is **two** spaces per level.

.yaml map example:

```yaml
--- # separator
object: Book # key: value

metadata: # value as a block of scalars (key: value)
  name: Three Men in a Boat
  author: Jerome K Jerome 
  genre: humorous account
  
published:
  year: 1889
  country: United Kingdom
 
# list - block style 
animals:
  - cat
  - dog
  - bird
  
animals: [cat, dog, bird] # list - flow style

# string that preserves newlines instead of a list |
saturday: |
  order cleaning
  order a pizza
  watch new series
```
