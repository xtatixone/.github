# 2. Example Demo

## Input

- The following is fragmented preview of input to output compilation.

```html
<!doctype html>
<html lang="en">

<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <!-- style -->
</head>
```
- `<!-- style -->`, a reserved comment tag, which will be replaced by compiled stylesheet.

```html

<body 
data-sveltekit-preload-data="hover" 
class="=bg$pattern-checkerboard =$custom-pattern" 
_$custom-pattern="
  --pattern-checker-bg1: #456734;
  --pattern-checker-bg2: #2bb43d;
  --pattern-checker-size: var(---delta-block-lg);
"
{@media (min-width:512px)}&="
  --pattern-checker-bg1: var(---primary-100);
  --pattern-checker-bg2: var(---secondary-900);
">
```

- You can compose classes with in html tags with attribute representing **symbolic classes (symclasses)** `bg$pattern`, and attribute which ends with `&` is considered **wrappers** for the symbolic class.
- `@--assign` / `=` can be used for initial compose of a symbolic class using **symclasses from libraries**. These will be hoisted to block scope and any explicit properties will easily override them. 
- Use `&` attribute to write comment, which can be used multiple times in the same tag. 
- To use a symbolic class use `={sym-class}` with in attributes. 

```html
  <stitch amorphous$--container>
    <svg xmlns="http://www.w3.org/2000/svg">
      <defs>
        <filter id="#glass-distortion" x="0%" y="0%" width="100%" height="100%">
          <feTurbulence type="fractalNoise" baseFrequency="0.008 0.008" numOctaves="2" seed="92"
            result="noise" />
          <feGaussianBlur in="noise" stdDeviation="2" result="blurred" />
          <feDisplacementMap in="SourceGraphic" in2="blurred" scale="77" xChannelSelector="R"
            yChannelSelector="G" />
        </filter>
      </defs>
    </svg>
  </stitch>
```

-  `<stitch ... > ... </stitch>` a special tag used to create a **dependency to text-block with a symclass**. The content with-in the tag will be only deployed if the corresponding symclass is used compiled CSS.

```html
  <sketch style="
    background-size: 18px 18px;
    background-image: linear-gradient(#ffffff 0.9px, transparent 0.9px), 
      linear-gradient(to right, oklab(100% 0 -0.00011) 0.9px, #cacaca 1px);
  " data-amorphous-type="liquid" amorphous$$$container="
    ~ amorphous$--container ;
    = p-12 m-0 border-0 d-flex align-center justify-center position-fixed;
    = tx$decoration-none isolate an$transition-all an$animation-delay-500;
    animation: .5s fade-in forwards;
    &:hover {
      = tf$scale-105;
    }
    &::after {
      = position-absolute inset-0 layer-neg-2 radius-16 tx$content-clear;
      filter: url(#\#glass-distortion);
    }
    &::before {
      = position-absolute inset-0 layer-neg-1 radius-16 tx$content-clear;
      box-shadow: inset 0 0 15px -5px #ffffffec;
    }
    &[data-glass-type=]& {
      &['liquid'] {
        &::after { backdrop-filter: blur(.5px); }
        &::before { background-color: #e7fffa73; }
      }
      &['frosted'] {
        &::after { backdrop-filter: blur(1px); }
        &::before { background-color: lab(93.8 1 -5.7 / 0.713); }
      }
    }
  ">
    Template
  </sketch>
```
- `<sketch ... > ... </sketch>`, a special tag which lets you create portable template for component level **symclasses**, which can be used for preview in a live sand-boxed environment while using language server. 
- `@--attach` / `~` can be used to add a dependency attachment of a symbolic class. These will be used for dependency tracking.

```html
  <div data-amorphous-type='liquid' class="~amorphous$$$container"> Content </div>
```
- Symbolic classes can  defined anywhere and used where-ever within the provided scope.

```html
  <!-- stitch -->
</body>
</html>
```
- `<!-- stitch -->` a reserved tag which will be replaced with stitch content of tracked dependencies.

## Output

```html
<!doctype html>
<html lang="en">

<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />

  <style>
    
    ._8h {
      --pattern-checker-bg1: var(---tertiary-300, #e0e0e0);
      --pattern-checker-bg2: transparent;
      --pattern-checker-size: 40px;
      background: linear-gradient(45deg, var(--pattern-checker-bg1) 25%, var(--pattern-checker-bg2) 25%, var(--pattern-checker-bg2) 75%, var(--pattern-checker-bg1) 75%, var(--pattern-checker-bg1)), linear-gradient(45deg, var(--pattern-checker-bg1) 25%, var(--pattern-checker-bg2) 25%, var(--pattern-checker-bg2) 75%, var(--pattern-checker-bg1) 75%, var(--pattern-checker-bg1));
      background-size: var(--pattern-checker-size) var(--pattern-checker-size);
      background-position: 0 0, calc(var(--pattern-checker-size) / 2) calc(var(--pattern-checker-size) / 2);
      display: flex;
      justify-content: center;
      align-items: center;
      min-width: 100vw;
      min-height: 100vh;
    }

    ._8i {
      padding: 6rem;
      margin: 0;
      border-width: 0;
      border-radius: 4rem;
      display: flex;
      align-items: center;
      justify-content: center;
      position: fixed;
      text-decoration: none;
      cursor: pointer;
      background: none;
      font-size: var(---font-size-h1);
      isolation: isolate;
      transition: all 300ms ease;
      box-shadow: 0px 6px 12px -6px #77777777;
    }

    ._8i.glass-type[data-amorphous-type='frosted']::after {
      backdrop-filter: blur(1px);
    }

    ._8i.glass-type[data-amorphous-type='frosted']::before {
      background-color: rgba(255, 255, 255, 0.6);
    }

    ._8i.glass-type[data-amorphous-type='liquid']::after {
      backdrop-filter: blur(.5px);
    }

    ._8i.glass-type[data-amorphous-type='liquid']::before {
      background-color: rgba(255, 255, 255, 0.25);
    }

    ._8i:hover {
      transform: scale(1.25);
    }

    ._8i::after {
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      z-index: -2;
      border-radius: 4rem;
      content: "";
      filter: url(#glass-distortion);
    }

    ._8i::before {
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      z-index: -1;
      border-radius: 4rem;
      content: "";
      box-shadow: inset 0 0 15px -5px #00000044;
    }

    @media (min-width:512px) {
      ._8h {
        --pattern-checker-bg1: var(---primary-100);
        --pattern-checker-bg2: var(---secondary-900);
      }
    }
  </style>
</head>

<body data-sveltekit-preload-data="hover" class="_8h">

  <div class="_8i" data-amorphous-type='liquid'>
    Content
  </div>

  <div>
    <svg xmlns="http://www.w3.org/2000/svg" style="display: none;">
      <defs>
        <filter id="glass-distortion" x="0%" y="0%" width="100%" height="100%">
          <feTurbulence type="fractalNoise" baseFrequency="0.008 0.008" numOctaves="2" seed="92"
            result="noise" />
          <feGaussianBlur in="noise" stdDeviation="2" result="blurred" />
          <feDisplacementMap in="SourceGraphic" in2="blurred" scale="77" xChannelSelector="R"
            yChannelSelector="G" />
        </filter>
      </defs>
    </svg>
  </div>
</body>

</html>
```
