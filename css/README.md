This guide is deeply inspired by style guides from [GitHub](https://github.com/styleguide/css) and [@mdo](http://codeguide.co/#css).

## Contents

* [Syntax](#syntax)
* [Shorthand Declarations](#shorthand-declarations)
* [@import](#import)
* [Nesting](#nesting)
* [Sprite](#sprite)
* [SCSS Variables](#scss-variables)
* [Components](#components)
* [Classes, IDs, and Elements](#classes-ids-and-elements)
* [Naming](#naming)
  * [General to Specific](#general-to-specific)
  * [Modular CSS and SCSS Nesting](#modular-css-and-scss-nesting)
  * [Prefer Prefixed (Namespaced) Class Names Over Chaining](#prefer-prefixed-namespaced-class-names-over-chaining)
* [Document Specificity Overrides and Browser Hacks](#document-specificity-overrides-and-browser-hacks)
* [File Organization](#file-organization)
* [Features](#features)
* [Prevent Flickering](#prevent-flickering)
* [JS class names](#js-class-names)
* [Responsive: Media Queries](#responsive-media-queries)
  * [Breakpoints](#breakpoints)
  * [Viewport](#viewport)
* [Print](#print)
* [Refactoring](#refactoring)
* [Above and Below the Fold](#above-and-below-the-fold)
* [Additional Resources](#additional-resources)

All code samples below assume we're using SCSS.

## Syntax

* Use soft-tabs with a two-space indent. Spaces are the only way to guarantee that the code renders the same in everyone's environment.

* Put spaces after `:` in property declarations.

    ```scss
    // bad
    .footer {
      display:block;
    }

    // good
    .footer {
      display: block;
    }
    ```

* Put spaces before `{` in rule declarations.

    ```scss
    // bad
    .footer{
      display: block;
    }

    // good
    .footer {
      display: block;
    }

    ```

* Put line breaks between rulesets.

    ```scss
    // bad
    .header {
      width: 800px;
    }
    .footer {
      display: block;
    }

    // good
    .header {
      width: 800px;
    }

    .footer {
      display: block;
    }
    ```

* When grouping selectors, keep individual selectors to a single line.

    ```scss
    // bad
    .header, .footer {
      display: block;
    }

    // good
    .header,
    .footer {
      display: block;
    }
    ```

* Place closing braces of declaration blocks on a new line.

    ```scss
    // bad
    .header {
      display: block; }

    // good
    .header {
      display: block;
    }
    ```

* Each declaration should appear on its own line for more accurate error reporting. This is important for tracking
  changes in GitHub.

    ```scss
    // bad
    .header { display: block; }

    .footer { height: 100px; width: 800px; }

    // good
    .header {
      display: block;
    }

    .footer {
      height: 100px;
      width: 800px;
    }
    ```

* Use hex color codes like `#000` unless you use `rgba()`.

    ```scss
    // bad
    .error {
      color: red;
    }

    // good
    .error {
      color: #ff0000;
    }

    // good
    .error {
      color: rgba(255, 0, 0, 0.8);
    }
    ```

* Use `//` for comment blocks instead of `/* */`.

    ```scss
    // bad
    /*
    .error {
      color: #ff0000;
    }
    */

    // good
    // .error {
    //   color: #ff0000;
    // }
    ```

* Avoid specifying units for zero values.

    ```scss
    // bad
    .footer {
      margin: 0px;
    }

    // good
    .footer {
      margin: 0;
    }
    ```

* Quote attribute values in selectors.
  [They're only optional in some cases](https://mathiasbynens.be/notes/unquoted-attribute-values#css), and it's a good
  practice to stay consistent.

    ```scss
    // bad
    input[type=text] {
      font-size: 12px;
    }

    // good
    input[type="text"] {
      font-size: 12px;
    }
    ```

## Shorthand Declarations

* Limit use of shorthand declarations to instances where you must explicitly set all the available values. Overriding
  things you're not concerned with makes it difficult for other selectors to override or inherit.

  For example, if you only want to set the bottom margin, then use `margin-bottom` rather than the shorthand `margin` with 4 values:

    ```scss
    // bad
    .footer {
      margin: 0 0 20px 0;
    }

    .footer {
      margin-bottom: 20px;
    }
    ```

## @import

Use SCSS's `@import` to mix in separate files and components. All CSS will turn into one file.

```scss
// bad
h1 {
  margin: 0;
  font-size: 18px;
}
// ... many more rules to reset the default browser styles

.button {
  font-size: 12px;
}
// ... many more button rules

// good
@import "reset";
@import "components/buttons";
```

## Nesting

* Avoid unnecessary nesting. At most, aim for three or four levels. If you cannot help it, step back and rethink your
  overall strategy. It's possible you don't really need so much specificity needed. You may also need to rethink the
  layout of the nesting.

    ```scss
    // bad
    .header {
      .navigation-container {
        .navigation {
          .menu {
            .menu-item {
              height: 20px;
            }
          }
        }
      }
    }

    // better
    .navigation {
      .menu {
        .menu-item {
          height: 20px;
        }
      }
    }

    // best
    .navigation-menu-item {
      height: 20px;
    }

    ```

## Sprite

* Use one mixin. Don't worry about repeating; gzip will handle it. Give the mixin two arguments for `x` and `y`
  positions. Let both arguments default to `0`. This is preferred over adding a `.sprite` class across the HTML.

    ```scss
    // app/assets/stylesheets/_mixins.scss
    @mixin sprite ($left: 0, $top: 0) {
      background-image: url("sprite.png");
      background-repeat: no-repeat;
      background-position: $left $top;
    }

    // app/assets/stylesheets/_icons.scss
    .thumbs-up {
      @include sprite(50px, 50px);
    }
    ```

## SCSS Variables

SCSS variables give flexibility by reusing values. This is especially useful for things such as colors.

* Name variables for what they represent, rather than what they contain.
    ```scss
    // bad
    $blue-background-color: #0066cc;

    // good
    $cta-background-color: #0066cc;
    ```

  Usage:

    ```scss
    // app/assets/stylesheets/_variables.scss
    $cta-background-color: #0066cc;

    // app/assets/stylesheets/components/_buttons.scss
    @import "variables";

    .check-availability-button {
      background-color: $cta-background-color;
    }
    ```

* Prefer semantic class names whose ruleset uses variables over CSS selectors such as `.cta-background-color`.

    ```html
    <!-- bad -->
    <button class="cta-background-color">Check Availability</button>

    <!-- good -->
    <button class="check-availability-button">Check Availability</button>
    ```

## Components

Components consist one visual feature. For instance, a Check Availability button could be considered a component. A
generic button couldn't.

* Give each component its own SCSS file.

    ```
    // bad
    // This file contains all styling:
    app/assets/stylesheets/main.scss

    // good
    app/assets/stylesheets/components/_check_availability_button.scss
    app/assets/stylesheets/components/_srp_listings.scss
    ```

Name components based on what they are rather than what they do. For example, `.hover-menu` is named based on an
interaction. A simple `.menu` or `.header-menu` is preferred. Using (but not targeting) HTML5 elements can give a hint
as to what to name it, e.g., `<section class="right-rail">`. Nested classes include things like `.right-rail-item` and
`.right-rail-cta`.

## Classes, IDs, and Elements

Prefer class names over IDs and elements. Targeting IDs introduces heavy-handed rules that are difficult to override,
especially with [feature flipping](#features). Targeting elements introduces cascading rules that may have unintended
side effects.

### Bad

```html
<!-- app/views/site/_navigation.html.slim -->
<ul id="main-navigation">
  <li>
    <a href="/houses">Houses</a>
  </li>
  <li>
    <a href="/apartments">Apartments</a>
  </li>
</ul>
```

```scss
// app/assets/stylesheets/components/_navigation.scss
#main-navigation {
  height: 30px;

  li {
    width: 50px;
  }
}
```

### Good

```html
<!-- app/views/site/_navigation.html.slim -->
<ul class="main-navigation">
  <li class="main-navigation-item">
    <a href="/houses">Houses</a>
  </li>
  <li class="main-navigation-item">
    <a href="/apartments">Apartments</a>
  </li>
</ul>
```

```scss
// app/assets/stylesheets/components/_navigation.scss
.main-navigation {
  height: 30px;
}

.main-navigation-item {
  width: 50px;
}
```

## Naming

### General to Specific

Name classes in a general-to-specific way to communicate the context for the specific classes. Suppose you have a text
item inside a menu link:

```scss
// bad
.menu {}
.menu-link {}
.text-link-menu {}

// good
.menu {}
.menu-link {}
.menu-link-text {}
```

### Modular CSS and SCSS Nesting

Naming classes with a flat structure with [Modular CSS](http://thesassway.com/advanced/modular-css-an-example) is preferred, especially for new components. This has several advantages:

1. Explicit rules do not rely or introduce unintended cascading
2. Rules do not depend on the structure of the HTML (less fragile)

Why Modular CSS? See [Understanding Modular CSS](https://speakerdeck.com/jlong/understanding-modular-css)

#### Example

##### Before: Depends on HTML structure and nesting.

    ```html
    <nav>
      <a href="/">Link A</a>
      <a href="/">Link B</a>
      <a href="/">Link C</a>
    </nav>
    ```

    ```scss
    nav {
      background: #000;
      text-align: left;

      a {
        display: inline-block;
      }
    }
    ```

##### After: No dependency on structure. Flat.

```html
<nav class="menu">
  <a class="menu-item" href="/">Link A</a>
  <a class="menu-item" href="/">Link B</a>
  <a class="menu-item" href="/">Link C</a>
</nav>
```

```scss
.menu {
  background: #000;
  text-align: left;
}

.menu-item {
  display: inline-block;
}
```

### Prefer Prefixed (Namespaced) Class Names Over Chaining

```scss
// bad
.success.btn

// good
.success-button
```

#### Advantages
1. Less specificity weight.
2. Does not introduce generic classes like `.success`, which may conflict with other CSS.

[Further Reading](http://markdotto.com/2012/02/16/scope-css-classes-with-prefixes/)

### .js- class names

Never target `.js-` class names with CSS. They are used exclusively for JavaScript.


## Document Specificity Overrides and Browser Hacks

When a given class needs more specificity to define a style, it is preferred to document what the other selector is and
the workaround.

### Example

```scss
// 'a' is only used to be more specific than '.spotlight_apartment a'
a.hd-chicklet {
  color: #FFF799;
}
```

## File Organization

In general, the CSS file organization should follow something like this:

```
├── app
│   ├── assets
│   │   └── stylesheets
│   │       ├── _mixins.scss
│   │       ├── _variables.scss
│   │       ├── components
│   │          └── _my_component.scss
│   │       └── features
│   │          └── _feature_name.scss
```

## Features

Features relate to one pinball feature. Each should have its own SCSS file of the same name. Pinball automatically adds
an umbrella `.use-{feature-name}` class to `<html>` which can be used for targeting.

When building new components that are only visible for the feature, use the new convention with modular CSS outside of
the umbrella class.

For example, consider the "more_info" feature:

```scss
// app/assets/stylesheets/features/_more_info.scss
.use-more-info {
  // override styles only for this experiment.
  .nav {
    background: blue;
  }

  // show components
  .more-info-cta {
    display: block;
   }
}

// Define components only used within the feature
.more-info-cta {
  display: none; // Hide by default when feature is disabled
  background: #000;
  // remaining styles
}
```

Limit feature names to 2-3 words, preferably with a page context. For example, `pdp_more_info` rather than
`expanded_more_info_button`.

Prefer attaching umbrella classes to `<html>` rather than `<body>`. The former is available immediately to JavaScript,
whereas the latter requires waiting for the DOM to be ready.

## Prevent Flickering

Use CSS extensively to hide and show components, rather than relying on JavaScript. This prevents any flickering.
Flickering makes the site feel less stable.

Status classes can be added to parent elements to give more control to CSS.  For example, adding
`.listing-with-sms-phone-number` to the HTML allows CSS to control the display without JavaScript.

If needed, two exclusive classes can be added to explicitly indicate the status. For example,
`.listing-without-sms-phone-number` or `listing-with-sms-phone-number`.

## Responsive: Media Queries

* Include in each component.
* Build flexible components by not specifying dimensions. Let the container determine width.
* Add your own helper classes (e.g. `.visible-xs` and `hidden-xs`). Examples from
  [bootstrap](http://getbootstrap.com/css/#responsive-utilities) and its
  [implementation](https://github.com/twbs/bootstrap/blob/master/less/responsive-utilities.less).
* Use SCSS mixins for a cleaner syntax.
* Designing for mobile first is preferred, when available.
* [Read more](https://developers.google.com/web/fundamentals/layouts/rwd-fundamentals/use-media-queries?hl=en)

#### Example

```scss
@mixin xs-device {
  @media (max-width: 767px) {
    @content;
  }
}

// Usage:
.my-component {
  @include xs-device {
    // styles specific to small phones
  }
}
```

### Breakpoints

* x-small
* small
* medium
* large
* x-large

We'll need to define which devices and orientation fit into which category at some point.

### Viewport

* Always define this.
* Prefer to not disable scaling
* [Read more](https://developers.google.com/web/fundamentals/getting-started/your-first-multi-screen-site/responsive?hl=en)

## Print

Include print stylesheets with the main CSS using `@media print {}`. A separate stylesheet is slower because it
introduces an extra HTTP request.

## Refactoring

Follow this [guide](https://speakerdeck.com/jlong/refactoring-css-programming-principles-for-designers).

### Things to keep in mind

1. Take small steps.
2. Refactor existing CSS when there's a story so QA can test. Ask for a story if you need it.
3. When a feature becomes permanent, there's an opportunity to refactor.
4. Be cautious not to change autotagging when renaming classes and IDs. Clicks are tracked and will pick up element IDs,
   then the first class name.

## Above and Below the Fold

Future performance work will separate above and below the fold CSS. ATF will be inlined in `<head>` on a per page basis. The rest of the CSS will load async at a later time. Exact implementation is a work in progress.

## Additional Resources
* [GitHub CSS Styleguide](https://github.com/styleguide/css)
* [@mdo](http://codeguide.co/#css)
* [sass-guidelin.es](http://sass-guidelin.es/)
