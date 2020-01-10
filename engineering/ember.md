# Ember.js Style Guide

## Table Of Contents

* [General](#general)
* [Organizing your modules](#organizing-your-modules)
* [Models](#models)
* [Controllers](#controllers)
* [Templates](#templates)
* [Routing](#routing)
* [Ember data](#ember-data)
* [Fetching data](#fetching-data)


## General

### Import what you use, do not use globals

For Ember Data, we should import `Model`, `attr` and other ember-data modules
from `ember-data`, and then destructure our desired modules.
For Ember, we should only import those modules that will be used.
Ember's imports can be found in the [JavaScript Module API RFC](https://github.com/emberjs/rfcs/blob/master/text/0176-javascript-module-api.md) and in the [Ember API docs](https://emberjs.com/api/ember/).
```javascript
// Good
import Model from 'ember-data/model';
import attr from 'ember-data/attr';
import { computed } from '@ember/object';
import { alias } from '@ember/object/computed';

export default Model.extend({
  firstName: attr('string'),
  lastName: attr('string'),

  surname: alias('lastName'),

  fullName: computed('firstName', 'lastName', function() {
    // Code
  })
});

// Bad
import Ember from 'ember';
import DS from 'ember-data';

export default DS.Model.extend({
  firstName: DS.attr('string'),
  lastName: DS.attr('string'),

  surname: Ember.computed.alias('lastName'),

  fullName: Ember.computed('firstName', 'lastName', {
    get() {
      // Code
    },

    set() {
      // Code
    }
  })
});
```

### Don't use Ember's Date prototype extensions

Avoid Ember's `Date` prototype extensions. Prefer the
corresponding functions from the `Ember` object.

Preferably turn the prototype extensions off by updating the
`EmberENV.EXTEND_PROTOTYPES` setting in your `config/environment` file.

```javascript
module.exports = function(environment) {
  var ENV = {
    EmberENV: {
      EXTEND_PROTOTYPES: {
        Date: false
      }
    }
```

### Use `this.get` and `this.set`

Calling `someObj.get('prop')` couples your code to the fact that
`someObj` is an Ember Object. This is informative for developers. 
It prevents you from passing in a POJO, which is sometimes preferable in testing, but then we are ensure whether we're dealing with a POJO or Ember Object.

When defining a method in a controller, component, etc. you
can be fairly sure `this` is an Ember Object, for consistency with the
above, we still use `get`/`set`.

```js
// Good

this.set('isSelected', true);
this.get('isSelected');

// Bad
import { get, set } from '@ember/object';

set(this, 'isSelected', true);
get(this, 'isSelected');
```

### Use brace expansion

This allows much less redundancy and is easier to read.

Note that **the dependent keys must be together (without space)** for the brace expansion to work.

```js
// Good
fullName: computed('user.{firstName,lastName}', {
  // Code
})

// Bad
fullName: computed('user.firstName', 'user.lastName', {
  // Code
})
```

## Organizing your modules

Ordering a module's properties in a predictable manner will make it easier to
scan.

1. __Injected Services__

   Define services that configure the module's behavior.

2. __Plain properties__

   Define properties that configure the module's behavior. Examples are
   `tagName` and `classNames` on components and `queryParams` on controllers and
   routes. Followed by any other simple properties, like default values for properties.
   
3. __Lifecycle hooks__

   The hooks should be chronologically ordered by the order they are invoked in.

4. __Single line computed property macros__

   E.g. `alias`, `sort` and other macros. Start with service injections. If the
   module is a model, then `attr` properties should be first, followed by
   `belongsTo` and `hasMany`.

5. __Multi line computed property functions__

6. __Functions__

   Public functions first, internal functions after.

7. __Actions__

```js
import { inject as service } from '@ember/service';

export default Component.extend({
  // Injected Services
  store: service(),
  
  // Plain properties
  tagName: 'span',
  
  // Lifecycle hooks
  init() {
    this._super(...arguments);
  },
  
  didReceiveAttrs() {
    this._super(...arguments);
    // code
  },

  // Single line CP
  post: alias('myPost'),

  // Multiline CP
  authorName: computed('author.{firstName,lastName}', function() {
    // code
  }),

  // Functions
  someFunction() {
    // code
  },
  

  actions: {
    someAction() {
      // Code
    }
  }
});
```

### Override init

Rather than using the object's `init` hook via `on`, override init and
call `_super` with `...arguments`. This allows you to control execution
order. [Don't Don't Override Init](https://dockyard.com/blog/2015/10/19/2015-dont-dont-override-init)

## Models

### Organization

Models should be grouped as follows:

* Associations
* Attributes
* Computed Properties

Within each section, the attributes should be ordered alphabetically.

```js
// Good
import Model from 'ember-data/model';
import attr from 'ember-data/attr';
import { computed } from '@ember/object';

export default Model.extend({
  // Associations
  children: hasMany('child'),
  
  // Attributes
  firstName: attr('string'),
  lastName: attr('string'),

  // Computed Properties
  fullName: computed('firstName', 'lastName', function() {
    // Code
  })
});

// Bad
import Model from 'ember-data/model';
import attr from 'ember-data/attr';
import { computed } from '@ember/object';
import { hasMany } from 'ember-data/relationships';

export default Model.extend({
  firstName: attr('string'),
  lastName: attr('string'),
  children: hasMany('child'),

  fullName: computed('firstName', 'lastName', function() {
    // Code
  })
});

```

## Controllers

Controllers should be used when defining query params. For all other use-cases reach for components. 

### Define query params first

For consistency and ease of discover, list your query params first in
your controller. These should be listed above default values.

### Alias your model

It provides a cleaner code to name your model `user` if it is a user. It
is more maintainable, and will fall in line with future routable
components

```javascript
export default Controller.extend({
  user: alias('model')
});
```

If your Route does not require a Controller, you can still name your model `user` in the `setupController` hook of the Route. 

```javascript
export default Route.extend({
  model() {
    // return promise;
  },
  
  setupController(controller, model) {
    this._super(...arguments);
    controller.set('user', model);
  }
});
```

## Templates

### Don't yield `this`

Use the hash helper to yield what you need instead.

```hbs
{{! Good }}
{{yield (hash thing=thing action=(action "action"))}}

{{! Bad }}
{{yield this}}
```

### Use components in `{{#each}}` blocks

Contents of your each blocks should be a single line, use components
when more than one line is needed. This will allow you to test the
contents in isolation via unit and integration tests, as your loop will likely contain
more complex logic in this case.

```hbs
{{! Good }}
{{#each posts as |post|}}
  {{post-summary post=post}}
{{/each}}

{{! Bad }}
{{#each posts as |post|}}
  <article>
    <img src={{post.image}} />
    <h1>{{post.title}}</h2>
    <p>{{post.summar}}</p>
  </article>
{{/each}}
```

### Always use the `action` keyword to pass actions.

Although it's not strictly needed to use the `action` keyword to pass on
actions that have already been passed with the `action` keyword once,
it's recommended to always use the `action` keyword when passing an action
to another component. This will prevent some potential bugs that can happen
and also make it more clear that you are passing an action.

```hbs
{{! Good }}
{{edit-post post=post deletePost=(action deletePost)}}

{{! Bad }}
{{edit-post post=post deletePost=deletePost}}
```

### Ordering static attributes, dynamic attributes, and action helpers for HTML elements

Ultimately, we should make it easier for other developers to read templates.
Ordering attributes and then action helpers will provide clarity.

```hbs
{{! Bad }}

<button disabled={{isDisabled}} data-auto-id="click-me" {{action (action click)}} name="wonderful-button" class="wonderful-button">Click me</button>
```

```hbs
{{! Good }}

<button class="wonderful-button"
  data-auto-id="click-me"
  name="wonderful-button"
  disabled={{isDisabled}}
  onclick={{action click}}>
    Click me
</button>
```

## Routing

### Route naming
Dynamic segments should be underscored. This will allow Ember to resolve
promises without extra serialization
work.

```js
// good

this.route('foo', { path: ':foo_id' });

// bad

this.route('foo', { path: ':fooId' });
```

[Example with broken
links](https://ember-twiddle.com/0fea52795863b88214cb?numColumns=3).

## Ember data

### Be explicit with Ember Data attribute types

Even though Ember Data can be used without using explicit types in
`attr`, always supply an attribute type to ensure the right data
transform is used.

```javascript
// Good

export default Model.extend({
  firstName: attr('string'),
  jerseyNumber: attr('number')
});

// Bad

export default Model.extend({
  firstName: attr(),
  jerseyNumber: attr()
});
```

## Fetching data

### Fetch the main model(s) for a url in Route `model` hooks

The model hooks are async hooks, and will wait for any promises returned
to resolve. An example of this would be viewing a trainer on a trainer's show page. 
A counter example would be viewing the clients for a trainer on a trainer's show page. The
clients should be fetched along side the trainer model, but should not block
your page from loading if the required model is there.

```javascript
// Good

// routes/trainer/show.js
export default Route.extend({
  model({ trainer_id }) {
    return this.store.findRecord('trainer', trainer_id);
  }
});

// Bad

// routes/trainer/show.js
export default Route.extend({
  model({ trainer_id }) {
    return this.store.findRecord('trainer', trainer_id);
  },
  
  afterModel(trainer) {
    return trainer.get('clients'); // async hasMany association
  }
});
```

### Fetch supporting model(s) in Component
By fetcing supporting models for a page outside of a Route's model hook, we enable support for skeleton screen loading. See why [Skeleton Screen Loading in Ember](https://emberway.io/skeleton-screen-loading-in-ember-js-2f7ac2384d63) can be useful. We have developed patterns that allow us to support empty state, loading state, and the resolved state of asynchronous fetches. 


#### Fetch supporting model(s) in `init` hooks

We can fetch supporting models for a page in the `init` hook of the component. This is convininent when we need to make a request via the ember-data `store`. Ex: `this.get('store').findAll('client');`

```hbs
{{!-- templates/trainer/show.hbs --}}
{{clients-list trainer=trainer}}
```

```javascript
// components/clients-list.js
export default Component.extend({
  store: service(),
  
  init() {
    this._super(...arguments);

    this.set('clients', []);
    this.get('fetchClients').perform();
  }
  
  isLoading: readOnly('fetch.isRunning'),
  
  fetchClients: task(function * (trainer) {
    // fetch all clients for a trainer
    let clients = yield this.get('store').query('client', { trainer_id: trainer.id });
    this.set('clients', clients);
  }),
});
```

```hbs
{{!-- template/components/clients-list.hbs --}}
{{#each clients as |client|}}
  {{!-- resolved state --}}
  {{client-list-item client=client}}
  
{{else unless isLoading}}
  {{!-- empty state --}} 
 
{{else each (repeat 10)}}
  {{!-- loading state --}}
  {{client-list-item/skeleton}}
{{/each}}
```


#### Fetch supporting model(s) using `await-promise` component

We can fetch supporting models for a page in the component's template. This is convininent when we need to make a request via an ember-data records' `belongsTo` or `hasMany` association. Ex: `trainer.get('clients');`

The `await-promise` component takes a promise as the first argument. It yields the original `promise` and resolved value as the first and second argument respectively. If the promise is not yet resolved it will yield `null` as the second argument.

```hbs
{{!-- templates/trainer/show.hbs --}}
{{clients-list trainer=trainer}}
```

```hbs
{{!-- template/components/clients-list.hbs --}}
{{#await-promise trainer.clients as |promise clients|}}
  {{#each clients as |client|}}
    {{!-- resolved state --}}
    {{client-list-item client=client}}
  
  {{else unless (is-pending promise)}}
    {{!-- empty state --}} 
    
  {{else each (repeat 10)}}
    {{!-- loading state --}}
    {{client-list-item/skeleton}}
  {{/each}}
{{/await-promise}}
```
