# 6. Composition

## Symbolic-Class 

``` 
{cluster}{scope}{identifier}
```

- **`cluster`**: Collection of classes, or use '-' to delegate to open cluster. 
  - Available characters: `A-Z`, `a-z`, `0-9`, and `-`.
  - `-` or `_`  is only for delegating open cluster at declaration. It will be hidden in other cases.
- **`scope`**: Scope of access of declared styles
  - `$` | **Local:** with in the declared file.
  - `$$` | **Global:** across all valid files in target folders.
- **`identifier`**: Specific identifier within the cluster.
  - Available characters: `A-Z` `a-z` `0-9` and `-`.

- While composing styles, you can use any of the following for external grouping:
  - **\` ... \`**
  - **\[ ... \]**
  - **\{ ... \}**
  - **\( ... \)**
  - **\' ... \'**
  - **\" ... \"**
- Be careful not to use the same internal quotes as the external grouping to avoid conflicts.

### Example
```html
<div -$button=" ... ">
<div _$button-2={ ... }>
<div animate$$fade-in=' ... '>
```

## Wrapper Attributes

- Each wrapper-attribute generates a corresponding wrapper element around the class, with selectors derived directly from the attribute name.
- Wrapper-attributes enable highly flexible conditional logic—styles can be scoped, toggled, or layered based on attribute presence or value.
- Responsive design breakpoints are implemented using wrapper-attributes, allowing layout and style shifts based on contextual constraints.

### Rule Specification
- Must terminate with an `&` token
- `hashrules` (`#{rule}`) are valid within these attributes.
- use `{...}` brackets for raw string formatting for not breaking at spaces.
- Within `identifier@{ ... }`, shorthand expressions map to style constraints:
  - `width>=` : `min-width:`
  - `width<=` : `max-width:`
  - `height>=` : `min-height:`
  - `height<=` : `max-height:` 
  
### Example
```html
<!-- Assume Hashrule `#{Load}` == "body[data-loading]" from hashrules.jsonc  -->
<div
  _$class="..."
  #{Load}&="..." 
  {@supports not (backdrop-filter: blur(1px))}&="..."
  container@{(max-width: 320px)}&="...">
  {Placeholder}
</div>
```
Gets structurally gets transformed into:
```css
.$class { ... }
body[data-loading] .$class { ... }
@supports not (backdrop-filter: blur(1px)) { 
  .$class { ... } 
}
@container (max-width: 320px) { 
  .$class { ... } 
}
```

## Compiled Classnames

- Each compilation command uses different stratagies for cascading and create classes.

### `debug`

- Unoptimized cascading order, and verbose classnames representing source-data.
- **Scattered Classes:**
  - Format: `{Type}|{Definition-Source}_{Symbolic-Class}`
  - Example: `PUBLIC|xrc/content/demo.html:30:2_glass$$$container`
- **Ordered Classes:**
  - Format: `TAG|{Import-Source}__{Type}|{Definition-Source}_{Symbolic-Class}`
  - Example: `TAG|xrc/content/demo.html:16:58__PUBLIC|xrc/content/demo.html:30:2_glass$$$container`
- **Final Classes:**
  - Format: `{Type}|{Definition-Source}_{Symbolic-Class}_Final`
  - Example: `PUBLIC|xrc/content/demo.html:30:2_glass$$$container_Final`

### `preview`

- Unoptimized cascading order, and respective classnames.
- Classname is enumered hash followed by cascade position index.
- **Scattered Classes:**
  - Format: `~{classname}_{hash}`
  - Example: `~scatter-class_g3` 
- **Ordered Classes:**
  - Format: `!{classname}_{hash}-{cascade-counter}`
  - Example: `!ordered-class_g3-134` 
- **Final Classes:**
  - Format: `={classname}_{hash}`
  - Example: `=final-class_g3` 

### `publish`

- Unoptimized cascading order, and respective classnames.
- Classname is enumered hash followed by cascade position index.
- **Scattered Classes:**
  - Format: `_{hash}`
  - Example: `__k9`, `__8i` 
- **Ordered Classes:**
  - Format: `__{hash}-{cascade-counter}`
  - Example: `__H9`, `__8h` 
- **Final Classes:**
  - Format: `___{hash}`
  - Example: `___k9`, `___8i`
