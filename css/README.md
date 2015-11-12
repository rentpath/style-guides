This guide is deeply inspired by styleguides from [GitHub](https://github.com/styleguide/css) and [@mdo](http://codeguide.co/#css)

**Contents**

- [Syntax](#syntax)
	- [Examples](#examples)
- [Limit use of shorthand declarations to instances where you must explicitly set all the available values](#limit-use-of-shorthand-declarations-to-instances-where-you-must-explicitly-set-all-the-available-values)
- [@import](#import)
- [Nesting](#nesting)
- [Sprite](#sprite)
- [SCSS Variables](#scss-variables)
- [Components](#components)
- [Prefer Class Names over IDs and Elements](#prefer-class-names-over-ids-and-elements)
- [General to Specific Naming](#general-to-specific-naming)
- [Modular CSS and SCSS Nesting](#modular-css-and-scss-nesting)
- [Prefer Prefixed (namespaced) Class names over chaining](#prefer-prefixed-namespaced-class-names-over-chaining)
- [Document Specificity Overrides and Browser Hacks](#document-specificity-overrides-and-browser-hacks)
- [File Organization](#file-organization)
- [Features](#features)
- [Prevent Flickering](#prevent-flickering)
- [JS class names](#js-class-names)
- [Responsive: Media Queries](#responsive-media-queries)
- [Breakpoints](#breakpoints)
- [Viewport](#viewport)
- [Responsive: IE8](#responsive-ie8)
- [Print](#print)
- [Refactoring](#refactoring)
- [Above and Below the Fold](#above-and-below-the-fold)
- [Additional Resources](#additional-resources)


### Syntax

- Use soft-tabs with a two space indent. Spaces are the only way to guarantee code renders the same in any person's environment.
- Put spaces after `:` in property declarations.
- Put spaces before `{` in rule declarations. e.g. `.selector {}`
- Put line breaks between rulesets.
- When grouping selectors, keep individual selectors to a single line.
- Place closing braces of declaration blocks on a new line.
- Each declaration should appear on its own line for more accurate error reporting. This is important for tracking changes in GitHub.
- Use hex color codes `#000` unless using `rgba()`.
- Use `//` for comment blocks (instead of `/* */`).
- Avoid specifying units for zero values, e.g., `margin: 0;` instead of `margin: 0px;`.
- Quote attribute values in selectors, e.g., `input[type="text"]`. [They’re only optional in some cases](https://mathiasbynens.be/notes/unquoted-attribute-values#css), and it’s a good practice for consistency.

#### Examples

Here are some good examples that apply the above guidelines:

```css
// Example of good basic formatting practices
.styleguide-format {
  color: #000;
  background-color: rgba(0, 0, 0, .5);
  border: 1px solid #0f0;
  @include sprite(10px, 10px);
}
```

```css
// Example of individual selectors getting their own lines
.multiple,
.classes,
.get-new-lines {
  display: block;
}
```

### Limit use of shorthand declarations to instances where you must explicitly set all the available values

**Rationale:** Overriding things you're not concerned with makes it difficult for other selectors to override or inherit.

For example, if you only want to set the bottom margin, then use `margin-bottom` rather than the shorthand `margin` (with 4 values).

```css
.overly-resets-other-margins {
  margin: 0 0 20px 0;
}

.only-sets-intentional-bottom-margin {
  margin-bottom: 20px;
}
```

### `@import`

Use SCSS's `@import` to separate files and components. All CSS will turn into one file.

### Nesting

As a rule of thumb, avoid unnecessary nesting in SCSS. At most, aim for three (maybe 4) levels. If you cannot help it, step back and rethink your overall strategy (either the specificity needed, or the layout of the nesting).

### Sprite

One mixin. Don't worry about repeating (gzip will handle it). Has two arguments for x and y position (defaults to 0, 0).

This is preferred rather than adding a `.sprite` class across the HTML.

**Example**
```scss
// app/assets/stylesheets/_mixins.scss
@mixin sprite ($left: 0, $top: 0) {
  background-image: url("sprite.png");
  background-repeat: no-repeat;
  background-position: $left $top;
}

// Usage:
.my-icon {
  @include sprite(50px, 50px);
}
```

### SCSS Variables

SCSS variables give flexibility by reusing values. This is especially useful for things such as colors. Variables should be named for what they represent, rather than what they contain.

For example, `$cta-background-color` rather than `$blue-background-color`.

```scss
// app/stylesheets/_variables.scss
$cta-background-color: #00f;

// Usage
@import "variables";

.check-availability-button {
  background-color: $cta-background-color;
}
```

Variables are preferred over CSS selectors such as `.cta-background-color` which is added to the DOM.

### Components

Components consist one visual feature. For instance, a Check Availability button could be considered a component, but not a generic button. Each component should have its own SCSS file like `app/stylesheets/components/_component.scss`.

Components should be named based on what they are rather than what they do. For example, `.hover-menu` is named based on interaction. A simple `.menu` or `.header-menu` is preferred. Using (but not targeting) HTML5 elements can give a hint as to what to name it e.g. `<section class="right-rail">`. Nested classes include things like `.right-rail-item` and `.right-rail-cta`.

### Prefer Class Names over IDs and Elements

Targeting IDs introduce heavy handed rules that are difficult to override (especially with [feature flipping](#features)). Targeting elements (e.g. `a`, `div`) introduce cascading rules that may have unintended side effects.

### General to Specific Naming

Naming classes in a general to specific way communicates the context for the class. For instance, if you have a text item inside a menu link —

**Specific to general (not preferred)**:
```scss
.menu {}
.menu-link {}
.text-link-menu {}
```

**General to Specific (preferred)**:
```scss
.menu {}
.menu-link {}
.menu-link-text {}
```

### Modular CSS and SCSS Nesting

Naming classes with a flat structure with [Modular CSS](http://thesassway.com/advanced/modular-css-an-example) is preferred, especially for new components. This has several advantages —

1. Explicit rules do not rely or introduce unintended cascading
2. Rules do not depend on the structure of the HTML (less fragile)

Why Modular CSS? See [Understanding Modular CSS](https://speakerdeck.com/jlong/understanding-modular-css)

#### Example

**Before: Depends on HTML structure and nesting**
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

**After: No dependency on structure. Flat**
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

### Prefer Prefixed (namespaced) Class names over chaining

`.success-button {}` > `.success.btn`

1. Less specificity weight.
2. Does not introduce generic classes (e.g. `.success`) which may conflict with other CSS.

[Further Reading](http://markdotto.com/2012/02/16/scope-css-classes-with-prefixes/)

### Document Specificity Overrides and Browser Hacks

When a given class needs more specificity to define a style, it is preferred to document what the other selector is and the workaround.

**Example**
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

### Features

Features relate to one pinball feature. Each should have its own SCSS file of the same name. Pinball automatically adds an umbrella `.use-{feature-name}` class to `<html>` which can be used for targeting.

When building new components that are only visible for the feature, use the new convention with modular CSS outside of the umbrella class.

For example, the feature `more_info` —

```scss
// app/assets/stylesheets/features/_more_info.scss
.use-more-info {
  // override styles only for this experiment.
  .nav { background: blue; }

  // show components
  .more-info-cta { display: block; }
}

// Define components only used within the feature
.more-info-cta {
  display: none; // Hide by default when feature is disabled
  background: #000;
  // remaining styles
}
```

Limit feature names to 2-3 words, preferably with a page context. For example, `pdp_more_info` rather than `expanded_more_info_button`.

Prefer attaching umbrella classes to `<html>` rather than `<body>`. The former is available immediately to JavaScript where the latter requires waiting for the DOM to be ready.

### Prevent Flickering

Use CSS extensively to hide and show components rather than relying on JavaScript. This prevents any form of flickering which makes the site feel less stable.

Status classes can be added to parent elements to give more control to CSS.  For example, adding `.listing-with-sms-phone-number` to the HTML allows CSS to control the display without JavaScript.

If needed, two exclusive classes can be added to explicitly indicate the status. For example, adding `.listing-without-sms-phone-number` or `listing-with-sms-phone-number`.

### JS class names

Never target `.js-` class names with CSS. They are used exclusively for JS.

### Responsive: Media Queries

TODO: How-to docs for building responsive sites. e.g. tech talk

- Include in each component.
- Build flexible components by not specifying dimensions. Let the container determine width.
- Add your own helper classes (e.g. `.visible-xs` and `hidden-xs`). Examples from [bootstrap](http://getbootstrap.com/css/#responsive-utilities) and its [implementation](https://github.com/twbs/bootstrap/blob/master/less/responsive-utilities.less).
- Use SCSS mixins for a cleaner syntax.
- Designing for mobile first is preferred when available.
- [Read more](https://developers.google.com/web/fundamentals/layouts/rwd-fundamentals/use-media-queries?hl=en)

**Example**
```scss
@mixin xs-device {
  @media (max-width: 767px) { @content; }
}

// Usage:
.my-component {
  @include xs-device {
    // specific to small phones
  }
}
```

### Breakpoints

x-small
small
medium
large
x-large

**TODO: Define which devices (and orientation) fit into which category**

### Viewport
Always define.
Prefer to not disable scaling
See https://developers.google.com/web/fundamentals/getting-started/your-first-multi-screen-site/responsive?hl=en


orientation change bugs: https://github.com/primedia/rentals/blob/dev/app/assets/javascripts/main.js#L186


### Responsive: IE8

In order to support IE8, the desktop version must be the default (with no media queries).

If supporting IE8 is required, building the site for desktop first without media queries. Then add media queries for smaller devices.

**Investigate generating a new stylesheet for IE8 - takes desktop sizes.**

Document mobile first (and do not support IE8).

### Print

Include print stylesheets with the main CSS using `@media print {}`. A separate stylesheet is slower (it introduces an extra HTTP request).

### Refactoring

Follow this [Guide](http://environmentsforhumans.com/2015/javascript-summit/presentations/Long-JSsummit2015-RefactoringCSS.pdf)

**Things to keep in mind**

1. Small steps
2. Refactor existing CSS when there's a story so QA can test. Ask for a story if you need it.
3. When a feature becomes permanent, there's an opportunity to refactor.
4. Be cautious not to change autotagging when renaming classes and IDs. Clicks are tracked and will pick up element ids, then the first class name. Ask a dev to help debug if you have concerns.

### Above and Below the Fold

Future performance work will separate above and below the fold CSS. ATF will be inlined in `<head>` on a per page basis. The rest of the CSS will load async at a later time. Exact implementation is a work in progress.

## Additional Resources
- [GitHub CSS Styleguide](https://github.com/styleguide/css)
- [@mdo](http://codeguide.co/#css)
- [sass-guidelin.es](http://sass-guidelin.es/)
