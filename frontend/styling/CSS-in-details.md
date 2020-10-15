## CSS tools

- [any shape using css: Picture](https://bennettfeely.com/clippy/)

## Considerations: Component vs Utility-Based CSS Design Systems

## What is a Utility-Based Framework?

Utility-based means that when implementing designs, you use predefined classes to set font sizes, padding, margins, colors, etc on each individual item to create a custom element from the available options.

### Existing Utility-Based Frameworks

Existing frameworks may speed up the process and cost less, but will often include excess code that may impact load times and complicate certain design decisions due to opinionated markup.
Being a popular approach, there are several options available such as Bootstrap, Foundation, Semantic UI, and Skeleton.

Using Bootstrap as an example, here’s some basic markup to show what an element’s class structure might look like when using an existing utility-based framework. This will generate a padded div with a light gray background, black text, a dark gray border, and rounded corners.

```html
<div class="p-3 bg-light text-dark border-secondary rounded">
  Lorem Ipsum Dolor Sit Amet
</div>
```

### Custom Utility-Based Frameworks

If your design is looking to do something more adventurous, needs strict limitations due to branding constraints, or needs to focus heavily on performance due to high traffic, a custom design system might be preferable. 
A custom utility framework can define custom class structures that make more sense to your team, or match your branding guidelines more accurately. Here’s an example of a custom framework we’ve generated to do the same as the panel example above.

```html
<div class="padding-all-3 background-gray-light gray-dark border-all-1 border-dark-gray border-radius-3">
  Lorem Ipsum Dolor Sit Amet
</div>
```

### Component-Based Systems

While utility-based options can function in any type of project, sometimes repetitive layouts can be best implemented with components: reusable, segmented blocks of predefined elements. Each element can have predefined styles and variations which limit how branding can be implemented and favors a single class per element rather than a series of utility classes to style it.
An example of the clarity and cleanliness in component based frameworks can be showcased with this minimal markup representing the same panel as before.

```html
<div class="panel">
  Lorem Ipsum Dolor Sit Amet
</div>
```

Variations can be added using methods like BEM to separate the versions of a component if needed.

An example of a self contained component would be used in a method like this:

```js
<panel>
  Lorem Ipsum Dolor Sit Amet
</panel>
```

This component could contain all of the logic, styles, and markup required to display the elements exactly as intended each time, ensuring consistency.

## TL:DR

- Existing utility-based frameworks are best for quick, cheap development such as rapid prototyping or MVPs (Minimum Viable Product).
- Custom utility-based frameworks require a little more budget, but can give better performance and can allow a site to represent your brand with custom designed utility classes.
- Component-based systems have cleaner and more customized markup and benefit greatly in single page apps.

## [CSS Utility Classes and "Separation of Concerns"](https://adamwathan.me/css-utility-classes-and-separation-of-concerns/)
