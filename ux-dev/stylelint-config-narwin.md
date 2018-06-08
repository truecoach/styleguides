# stylelint-config-narwin

A unified set of rules that help push the UXD team in the same direction for CSS best practices. Below will be some short examples of some linter rules and for the complete ruleset visit [stylelint-config-narwin](https://github.com/DockYard/stylelint-config-narwin).

## Installation
[stylelint-config-narwin#installation](https://github.com/DockYard/stylelint-config-narwin#installation)

## Usage
[stylelint-config-narwin#usage](https://github.com/DockYard/stylelint-config-narwin#usage)

## Examples
In order to see all the rules in [stylelint-config-narwin](https://github.com/DockYard/stylelint-config-narwin) look at the `index.js` file at [https://github.com/DockYard/stylelint-config-narwin/blob/master/index.js](https://github.com/DockYard/stylelint-config-narwin/blob/master/index.js).

Further you can find documentation on the available Stylelint rules at [stylelint.io](https://stylelint.io/user-guide/rules/).

### Remove empty comments:
The rule `"comment-no-empty": true,` will not allow any type of empty comments.

Will throw linter error:
```css
.block {
  position: relative; /* */
```

Fixed and no error:
```css
.block {
  position: relative;
}
```

### String quotes:
The rule `"string-quotes": "double"` will only allow double quotes in strings.

Will throw linter error:
```css
.block {
  &:after {
    content: '';
  }
}
```

Fixed and no error:
```css
.block {
  &:after {
    content: "";
  }
}
```

### Property order

Property order is loosely enforced by property "type", according to the rules
[here](https://github.com/dockyard/styleguides/blob/master/ux-dev/css.md#declaration-order). E.g., "box model" properties (position, float, width, etc.) must come before "typography" properties (font, line-height, color, etc.), which must come before "visual" properties (border, background, etc.)

The rules are strict enough that you aren't allowed to put `color` before `float`, for instance, but you are allowed to put `float` and `position` in whichever order you like (as long as they are grouped with similar properties).

Declaration types must also appear in the following order:

- Custom properties (`--color: red;`)
- Dollar variables (`$var: 10px;`)
- Standard declarations (`float: none;`)
- @ rules (`@include`, `@extend`, etc.)
- Media queries

For flexibility, nested selectors may go anywhere within this order.

Invalid:
```scss
.block {
  @include mixin(10px);
  --color: red;
  border: 1px solid var(--color);
  font-size: 12px;
  position: absolute;
}
```

Valid:
```scss
.block {
  --color: red;
  position: absolute;
  font-size: 12px;
  border: 1px solid var(--color);

  @include mixin(10px);
}
```

## Adding more stylelint rules
If you have a stylelint rule you think should be included in [stylelint-config-narwin](https://github.com/DockYard/stylelint-config-narwin) the best practice is to open a PR in the [stylelint-config-narwin](https://github.com/DockYard/stylelint-config-narwin) repository. Include why the rule should be included and ask for input with other UXD team members.
