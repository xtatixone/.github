# 7. Custom Tags

## Declaration Tags (Paired Tags)

- If a symbolic-class is found as attribute with in special tags, the content between tags is considered bound to it.
- Paired tags are collapsed, and self closing tags are replaced with proper value while compiling.

```html 
<style local$-class>
  ...
</style>
<stitch local$--class>
  ...
</stitch>
<sketch
  attribute-1="attr-value-1"
  attribute-2="attr-value-2"
  attribute-3="attr-value-3"
  local$class="
    ~ local$-class local$--class;
    = $class-1 $$class-2 $class-3;
    property-1: value-1;
    property-2: value-2;
    property-3: value-3;
  "
>
  Template
</sketch>
```

### `<style> ... </style>` 
- This tag is a special case, as it’s the standard HTML tag for writing CSS content inside markup.
- If a sym-class is found in the opening tag, the content between tags is considered a dependent snippet of that corresponding sym-class.
- Declared using a sym-class where - immediately follows the final $ (e.g., style$-class-name).

### `<stitch> ... </stitch>`
- Snippets are imported in a minified form but remain unprocessed.
- Useful for direct association without transformation or validation.
- Declared using a sym-class where -- immediately follows the final $ (e.g., stitch$--class-name).

### `<sketch> ... </sketch>`
- Used to declare component-level styles and generate corresponding style templates.
- The snippet inside these tags is used for live preview of the given classes.
- Style attributes are passed to the sandbox body, while other attributes are passed directly to the preview sandbox.

## Replacement Placeholders (Self-Closing Tags && Reserved HTML Comments)

```html
<!doctype html>
<html lang="en">

<head>
  <meta charset="utf-8" />
  <meta 
    name="viewport"
    content="width=device-width, initial-scale=1" 
  />
  <!-- style -->
</head>

<body 
  data-sveltekit-preload-data="hover" 
  class="=bg$pattern-checkerboard =$custom-pattern" 
  _$custom-pattern="
    --pattern-checker-bg1: #456734;
    --pattern-checker-bg2: #2bb43d;
    --pattern-checker-size: var(---delta-block-lg);
  "
>

  <stitch amorphous$--container>
    <svg xmlns="http://www.w3.org/2000/svg">
      <defs> ... </defs>
    </svg>
  </stitch>


  <sketch 
    data-glass-type="liquid" 
    amorphous$$$container="
      ~ amorphous$--container;
      &::after {
        filter: url(#\#glass-distortion);
      }
    "
  >
    Template
  </sketch>

  <div 
    id="#scoped-id" 
    data-glass-type='liquid' 
    class="~amorphous$$$container"
  > Content </div>
  <!-- stitch -->
</body>

</html>
```

### `<sketch />` /  `<!-- sketch -->` 

- Used as a placeholder for deploying `stylesheet` and `stitch-snippets` together in the compiled output.
- Intended for rapid prototyping, not recommended for production use.

### `<style />` /  `<!-- style -->`

- Embed stylesheet using these tags with in any targeted files. 

### `<stitch />` / `<!-- stitch -->`

- Acts as a placeholder for injecting attached `stitch-snippets` into the compiled output.
