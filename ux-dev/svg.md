# SVGs

Scalable Vector Graphics - they're retina-friendly by default and lighter than most other image formats. Use them whenever you can! (Here's how…)

## Best Practices

There are multiple ways to use an SVG in a webpage (but we stick to two):

* inline directly in page markup
* as the `src`/`srcset` for an `img` tag (or other media element, like `picture > source` - of course, this would just be for art direction and not responsive image, since SVGs are resolution-independent already)
* as the `data` attr in an `<object>` tag
* as a CSS `background-image` with `url('path/to/file.svg')`
* as an inlined CSS `background-image` with `url('data:image/svg+xml;utf8,<svg></svg>)`

By default, we prefer **inline SVG** (see [SVGJar](#SVGJar)), especially when CSS styling of element colors is necessary, or where the layout requires the SVG to define its own space.

For background decorations, we use **background-image SVGs**. [`narwin-pack`](https://github.com/DockYard/narwin-pack/blob/master/index.js) uses [`postcss-inline-svg`](https://github.com/TrySound/postcss-inline-svg) to convert those to inline data SVGs.

```postcss
.icon--background {
    background-image: svg-load('assets/icons/download.svg', fill=#283ae2, stroke=#fff);
}
```

You may have heard about icon fonts. [SVG icons are better](https://blog.github.com/2016-02-22-delivering-octicons-with-svg/).

## Accessibility

If you're using an SVG as an image's `src`, follow the normal rules for `alt` text and accessibility.

If the SVG is inline, add `role="presentation"` if it's purely decorative. However, if it needs descriptive text, add an `aria-label` to the SVG tag and consider putting that text in a `<title>` element as well. The `aria-label` will be announced to screen readers and the `<title>` will create a hover "tooltip" for sighted users on devices with pointers.

```xml
<svg aria-label="DockYard Logo">
  <title>DockYard Logo</title>
  <path />
</svg>
```

## Styling SVGs with CSS

SVG children element have lots of attributes that define their appearanc (`fill`, `stroke`, and all their sub-attributes). Many of those are available to CSS as well.

_This_

```xml
<svg class="icon">
  <path fill="#283ae2">
</svg>
```

_is equivalent to this:_

```postcss
.icon path {
  fill: #283ae2;
}
```

If all the inner elements will respond to a color property the same way, use the tag name to style them:

```postcss
.icon--blue circle {
  fill: blue;
}
```

If inner elements need the same property manipulated differently in CSS, use BEM classes on inner elements:

```xml
<svg class="icon">
  <path class="icon__path-1" />
  <path class="icon__path-2" />
</svg>
```

```postcss
.icon__path-1 {
  fill: #283ae2;
}

.icon__path-2 {
  fill: #3ae228;
}
```

If you need color changes on `:hover` or other pseudo-selector states, remember to put those on the right containing element:

_Not gonna do what you want:_

```postcss
.icon path {
  fill: #283ae2

  &:hover {
    fill: #3ae228;
  }
}
```

_You probably want this:_

```postcss
.icon path {
  fill: #283ae2;

  button:hover & {
    fill: #3ae228;
  }
}
```

_Reminder, `button:hover` might not be the best BEM-scoped selector. Refer to BEM docs for guidance there, not this shortened example._

Don't forget, `currentColor` is a native CSS variable that lets SVG icons inherit colors from their parent elements.

_This SVG_

```html
<a class="link">
  <svg class="link__icon">
    <path fill="currentColor">
  </svg>
</a>
```

_will inherit these colors:_

```postcss
.link {
  color: #283ae2;

  &:hover {
    color: #3ae228
  }
}
```

## Optimization

Start by taking your SVG (from Design handoff, or exported from a design app) over to [SVGOMG](https://jakearchibald.github.io/svgomg/). The default options will be just what you need most of the time. You can see both the visual result and code output by toggling "Image" and "Markup" in the top navigation. Occasionally, an optimization may break the visual rendering. In this case, toggle the settings in the "Features" list until it's no longer broken and optimize the image without that setting enabled. Don't hesitate to ask another UX Dev for help - SVG spec is complicated and none of us can memorize all the tiny gotchas that cause big problems. Rarely, you may need to go back to Design and pair to get an SVG built and exported in a way that can be optimized better.

SVGOMG is a PWA that will work offline. Also, it processes everything in your browser; it's safe to use on projects with "no 3rd party apps" NDAs.

### Keep `viewBox`

The `viewBox` attribute defines the SVG's internal dimensions/measurements and should be preserved. Whether you size the SVG with CSS properties or HTML attributes, keeping the `viewBox` ensures the SVG's inner elements will be scaled correctly across browsers.

_This is stable:_

```xml
<svg viewBox="0 0 160 90" width="32" height="18"></svg>
```

_So is this:_

```postcss
.icon {
  width: 32px
  height: 18px;
}
```
```xml
<svg class="icon" viewBox="0 0 160 90"></svg>
```

_This, not so much…_

```xml
<svg></svg>
```

### Remove `<style>` tags

Many image apps add `fill`, `stroke` or other visual properties to inner elements using classes and a `style` tag. When you're using multiple SVGs in a page, classes can overlap and cause unexpected style issues. Convert all `style` blocks to inline attributes on child elements.

_Change this:_

```xml
<svg>
  <style>
    .cls-1 {
      fill: #283ae2;
    }
  </style>
  <path class="cls-1" />
</svg>
```

_…into this:_
```xml
<svg>
  <path fill="#283ae2" />
</svg>
```

### Transforms

Occasionally, a design app will apply unnecessary transforms to inner element (and maybe even unnecessarily offset the `viewBox` too). SVGOMG doesn't always catch these, but with a little addition/subtraction, it's not too terrible to optimize them manually.

_Nope:_

```xml
<svg viewBox="-15 -15 33 33" xmlns="http://www.w3.org/2000/svg">
  <path d="..." transform="translate(-15 -15)"/>
</svg>
```

_Yep:_

```xml
<svg viewBox="0 0 33 33" xmlns="http://www.w3.org/2000/svg">
  <path d="..." />
</svg>
```

Sub-par exports could affect SVG child element positioning too:

_Still nope:_

```xml
<svg viewBox="0 0 32 32" xmlns="http://www.w3.org/2000/svg">
  <circle r="12" cx="24" cy="24" transform="translate(-8 -8)" />
</svg>
```

_Yes, please:_

```xml
<svg viewBox="0 0 32 32" xmlns="http://www.w3.org/2000/svg">
  <circle r="12" cx="16" cy="16" />
</svg>
```

These manual optimizations can get much trickier inside a `path` values list, however. (If there are capital letters scattered throughout the values, those point to absolute coordinates, so each following x/y pair needs to be adjusted to offset a stray `transform` or `viewBox` offset.) Typically, SVGOMG will fix those once non-`path` elements have been cleaned up from unnecessary transforms. But if not, and you're stuck, holler at your UXD pals for help - either we'll figure it out, or we might just stop optimizing if it's too tricky.

## SVGJar

Many DY projects use [ember-svg-jar](https://www.npmjs.com/package/ember-svg-jar). If you have a question about it, start with its [docs](https://github.com/ivanvotti/ember-svg-jar#installation) and [issues](https://github.com/ivanvotti/ember-svg-jar/issues)

### Adding SVGs

Optimize new SVGs, the drop them in your project's `/public/assets/` directory. If your project has more specific rules (like a `/svgs/` dir in `/assets/`), follow those rules. (SVGJar will find SVGs anywhere in `/public/` and can be configured to look in other directories if a project requires it.)

### Using SVGs in Templates

In your template file, use the SVGJar component:

```hbs
{{svg-jar "path/to/file-name"}}
```

This component also handles several other useful properties, including `class`, `height`, `width`, and more:

```hbs
{{svg-jar "icons/logo" class="header__logo" height="70" width="140"}}
```

### Previewing Available SVGs

To preview all the SVGs available to SVGJar, go to `https://localhost:4200/ember-svg-jar/index.html`. (Try `http` if your local dev site isn't set to run over `https`.)

This previewer page will also let you quickly copy the Ember component code needed for any available SVG.

## Resources

* [MDN](https://developer.mozilla.org/en-US/docs/Glossary/SVG)
* [Jenkov's Tutorials](http://tutorials.jenkov.com/svg/index.html)
* Sara Soueidan: [her blog](https://www.sarasoueidan.com/blog/), [her CSS-Tricks posts](https://css-tricks.com/author/sarasoueidan/), [anything](https://duckduckgo.com/?q=sara+soueidan+svg)
* [Sarah Drasner](https://sarahdrasnerdesign.com/)
* [Una Kravetz](https://una.im/archive/)