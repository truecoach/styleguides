# Handlebars Style Guide

## Table Of Contents

* [Quotes](#quotes)
* [Multiline expressions](#multiline-expressions)

## Quotes
Prefer double quotes to single quotes
```hbs
{{! good }}
<button {{action "doThing"}}>Do Thing</button>

{{! bad }}
<button {{action 'doThing'}}>Do Thing</button>
```

## Multiline expressions

Multi-line expressions should specify attributes starting on the second
line, and should be indented one deeper than the start of the component
or helper name.
```hbs
{{! good }}
{{x-thing
    value=blah
    options=options
    label="thing"}}

{{! bad }}
{{x-thing
  value=blah
  options=options
  label="thing"}}

{{! this will be annoying to re-indent if you rename your component }}
{{x-thing value=blah
          options=options
          label="thing"}}

```

## Line Breaks and Whitespace

It can be helpful to have a multi-line expression be on a separate line its parent HTML—particularly when merging in `git`. When needed, you can optionally use [whitespace control](http://handlebarsjs.com/expressions.html#whitespace-control) to remove whitespace before or after an expression.

The following example removes whitespace before and after an expression:
```hbs
<section>
  {{~content~}}
</section>
```

This removes whitespace before and after the list, and between items:
```hbs
{{~#each item in model~}}
   {{item}}
{{~/each~}}
```
This leaves spaces before and after the list, but removes between items.

```hbs
{{#each item in model}}
   {{~item~}}
{{/each}}
```

## Comments
The most common HTML comment at DockYard is a `TODO`. `TODO`s are used to track items that need to be completed at a later date (such as incomplete placeholder links, images, and copy) or when engineering is needed to complete the work.

When writing a `TODO`, specify if it is for UXD or engineering. If it’s for UXD, make sure to communicate that `TODO` with a UXD team member via a tag in the PR, a ping in the project Slack, or both.

When you finish a `TODO`, delete the comment.

Example:
```html
{{! TODO UXD: Add `is-active` style on the button below when a user clicks on it }}
<button>Add Event</button>
```

## Resources
* [The Art of Comments](https://css-tricks.com/the-art-of-comments/)
* [Clear communication through HTML and GitHub](https://dockyard.com/blog/2015/09/02/clear-communication-through-html)
* [Handlebars Whitespace Control](https://handlebarsjs.com/expressions.html#whitespace-control)
* [Ember White Space Playground](https://emberjs.jsbin.com/nubup/1/edit?html,css,js,output)
* [VS Code TODO Highlighter](https://marketplace.visualstudio.com/items?itemName=wayou.vscode-todo-highlight)
