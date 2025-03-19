# eXtensible Markup Language

consists of tags and elements

example

```xml
<?xml version="1.0" encoding="UTF-8"?>
<library>
  <book>
    <title>The Three-Body Problem</title>
    <author>Liu Cixin</author>
  </book>
  <book>
    <title>Modern Operating Systems</title>
    <author>Andrew S. Tanenbaum</author>
  </book>
</library>
```

```xml
<library>               <!-- Tag -->
    <book id="1" name="Three-Body">      <!-- Tag with Attributes -->
        <title>...</title>  <!-- Tag -->
        <author>...</author> <!-- Tag -->
        <year>...</year>     <!-- Tag -->
        <genre>...</genre>   <!-- Tag -->
    </book>
</library>
```
