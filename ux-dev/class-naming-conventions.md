
# TrueCoach CSS / Sass Styleguide

*Most definitely a work in progress*

## Table of Contents

1. [Intro](#intro)
1. [Terminology](#terminology)
    - [Rule Declaration](#rule-declaration)
    - [Selectors](#selectors)
    - [Properties](#properties)
1. [CSS](#css)
    - [Formatting](#formatting)
    - [Comments](#comments)
    - [OOCSS and BEM](#oocss-and-bem)
    - [ID Selectors](#id-selectors)
    - [JavaScript hooks](#javascript-hooks)
    - [Border](#border)
1. [Sass](#sass)
    - [Syntax](#syntax)
    - [Ordering](#ordering-of-property-declarations)
    - [Variables](#variables)
    - [Mixins](#mixins)
    - [Extend directive](#extend-directive)
    - [Nested selectors](#nested-selectors)
1. [Translation](#translation)
1. [License](#license)

## Intro

### Styleguide driven design and development

Every time new CSS is added, it increases our CSS bloat, CSS maintenance, and can add to inconsistencies in the TrueCoach user experience. If we follow a practice of designing with styles in the styleguide first and try to implement our designs with only styles in the style guide first, we reduce the risk of deviating away from these styles.

If new styles are needed:

* Use global variables where appropriate, such as spacing, typography, and color variables.
* Write styles in a way that can be folded back into the global style guide should it become a common pattern, i.e. following our principles for naming conventions, components, objects, and utilities as listed below.

## Terminology

### Rule declaration

A “rule declaration” is the name given to a selector (or a group of selectors) with an accompanying group of properties. Here's an example:

```sass
.banner
  font-size: 18px
  line-height: 1.2
```

### Selectors

In a rule declaration, “selectors” are the bits that determine which elements in the DOM tree will be styled by the defined properties. Selectors can match HTML elements, as well as an element's class, ID, or any of its attributes. Here are some examples of selectors:

```sass
.banner
  /* ... */

[aria-hidden]
  /* ... */
```

### Properties

Finally, properties are what give the selected elements of a rule declaration their style. Properties are key-value pairs, and a rule declaration can contain one or more property declarations. Property declarations look like this:

```sass
/* some selector */ 
  background: #f1f1f1
  color: #333
```

**[⬆ back to top](#table-of-contents)**

## CSS

* Choose verbose over clever. A little duplication is worthwhile if it adds clarity.
* Don't prioritize being DRY if it means it's hard to read and understand, creates dependencies, or hides what the code is really doing.
* Avoid overusing pre-processor features that make the code less approachable.

### Formatting

* Use soft tabs (2 spaces) for indentation.
* Prefer camelCasing in class names (see [OOCSS and BEM](#oocss-and-bem) below).
* Do not use ID selectors.
* When using multiple selectors in a rule declaration, give each selector its own line.
* In properties, put a space after, but not before, the `:` character.
* Put blank lines between rule declarations.

**Bad**

```sass
.avatar
    border-radius:50%
    border:2px solid white
.no, .nope, .not_good
  // ...
#lol-no
  // ...
```

**Good**

```sass
.avatar
  border-radius: 50%
  border: 2px solid white

.one,
.selector,
.per-line
  // ...
```

### Comments

* Prefer line comments `//` to block comments.
* Prefer comments on their own line. Avoid end-of-line comments.
* Write detailed comments for code that isn't self-documenting:
  - Uses of z-index
  - Compatibility or browser-specific hacks

### OOCSS and BEM

We encourage some combination of OOCSS and BEM for these reasons:

  * It helps create clear, strict relationships between CSS and HTML
  * It helps us create reusable, composable components
  * It allows for less nesting and lower specificity
  * It helps in building scalable stylesheets

**OOCSS**, or “Object Oriented CSS”, is an approach for writing CSS that encourages you to think about your stylesheets as a collection of “objects”: reusable, repeatable snippets that can be used independently throughout a website.

  * Nicole Sullivan's [OOCSS wiki](https://github.com/stubbornella/oocss/wiki)
  * Smashing Magazine's [Introduction to OOCSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/)

**BEM**, or “Block-Element-Modifier”, is a _naming convention_ for classes in HTML and CSS. It was originally developed by Yandex with large codebases and scalability in mind, and can serve as a solid set of guidelines for implementing OOCSS.

  * CSS Trick's [BEM 101](https://css-tricks.com/bem-101/)
  * Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

We recommend a variant of BEM with camel-cased “blocks”. Dashes and double-dashes are still used for modifiers (`block--modifier`) and children (`block-child`).

**Example**

```hbs
<div class="banner banner--warning">

  <div class="banner-icon">
    {{fa-icon 'exclamation-circle'}}
  </div>

  <p class="banner-message">
    Something might be wrong.
    <a class="banner-link">
      Let's try to fix it.
    </a>
  </p>

</div>
```

```scss

// block
.banner {...}

// elements
.banner-icon {...}
.banner-message {...}
.banner-link {...}

// modifiers
.banner--danger {...}
.banner--info {...}
.banner--warning {...}
```

  * `.banner` is the “block” and represents the higher-level component
  * `.banner-icon` is an “element” and represents a descendant of `.banner` that helps compose the block as a whole.
  * `.banner--warning` is a “modifier” and represents a different state or variation on the `.banner` block.

### Utilities

Utilities provide the building blocks for layout and handle a range common use cases that help us avoid writing custom styles. When we need to add some margin or padding, rather than adding a new selector specific to that use case, we can use utilities. This helps us reduce the number of unique property-value pairs, and helps us keep more consistent styles across the site.

* Utilities should do one job well and one job only, are immutable, and on occasion are an acceptable approach to overriding component styles.
* Utility class-names should be transparent and clearly describe the job they do.

Examples:

```sass
.bg-gray-light 
  background-color: #ddd

.d-inline-block 
  display: inline-block

.margin-right 
  margin-right: $space

.text-uppercase 
  text-transform: uppercase

.text-white 
  color: #fff
```

### ID selectors

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.

### JavaScript hooks

Avoid binding to the same class in both your CSS and JavaScript. Conflating the two often leads to, at a minimum, time wasted during refactoring when a developer must cross-reference each class they are changing, and at its worst, developers being afraid to make changes for fear of breaking functionality.

We recommend creating JavaScript-specific classes to bind to, prefixed with `.js-`:

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>
```

### Border

Use `0` instead of `none` to specify that a style has no border.

**Bad**

```css
.foo {
  border: none;
}
```

**Good**

```css
.foo {
  border: 0;
}
```
**[⬆ back to top](#table-of-contents)**

## Sass

### Syntax

* Use the `.sass` syntax rather than `.scss` (no curly braces or semicolons).

### Ordering of property declarations

_This section needs to be revisited and completed_

1. Property declarations

    List all standard property declarations, anything that isn't an `@include` or a nested selector.

    ```sass
    .btn-green
      background: green
      font-weight: bold
      // ...
    ```

2. `@include` declarations

    Grouping `@include`s at the end makes it easier to read the entire selector.

    ```sass
    .btn-green
      background: green
      font-weight: bold
      @include transition(background 0.5s ease)
      // ...
    ```

3. Nested selectors

    Nested selectors, _if necessary_, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.

### Variables

_This section needs to be revisited and completed_

Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`).

### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions. Mixins that accept no arguments can be useful for this, but note that if you are not compressing your payload (e.g. gzip), this may contribute to unnecessary code duplication in the resulting styles.

### Extend directive

`@extend` should be avoided because it has unintuitive and potentially dangerous behavior, especially when used with nested selectors. Even extending top-level placeholder selectors can cause problems if the order of selectors ends up changing later (e.g. if they are in other files and the order the files are loaded shifts). Gzipping should handle most of the savings you would have gained by using `@extend`, and you can DRY up your stylesheets nicely with mixins.

### Nested selectors

**Do not nest selectors more than three levels deep!**

```sass
.page-container 
  .content 
    .profile 
      // STOP!
```

When selectors become this long, you're likely writing CSS that is:

* Strongly coupled to the HTML (fragile) *—OR—*
* Overly specific (powerful) *—OR—*
* Not reusable

Again: **never nest ID selectors!**

### Example module

```sass

/* _button.sass */
// *************************************
//
//   Button
//   -> Description of button module (block)
//
// *************************************

.button
  border-radius: 4px
  cursor: pointer
  padding: 16px

// -------------------------------------
//   Modifiers
// -------------------------------------

.button--warning
  background-color: red

// -------------------------------------
//   States
// -------------------------------------

.button.is-disabled
  cursor: default
  opacity: .5

// -------------------------------------
//   Scaffolding (elements)
// -------------------------------------

.button-icon
  margin-right: 12px
```

**[⬆ back to top](#table-of-contents)**
