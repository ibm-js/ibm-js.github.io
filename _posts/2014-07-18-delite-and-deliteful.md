---
layout: post
title: Introducing the Delite & Deliteful Projects
author: cjolif
---

During the last months (nearly a year actually!), in addition to our <a href="http://dojotoolkit.org/">Dojo Toolkit</a>
contributions, we have been working on two [Web Components](http://www.w3.org/TR/components-intro/) related open source
projects. Now you might say, "well, Web Components are just standards, [Polymer](http://www.polymer-project.org/) is already
providing polyfill implementations for those standards, so why should I bother looking into some other projects in that area?"

This post is here to try answer that question by laying out the foundations of our projects and how they relate and 
differentiate from other projects out there. I hope this will answer questions you might have and raise your interest! 

## Delite

The first project, [delite](/delite/index.html), is all about UI core infrastructure. The idea behind it is that 
Web Components are definitely the way to go to build UI for the Web and that for this reason they must be adopted
for delite, but that there is also room for innovation around or on top of the Web Components themselves and that care
must be taken on how the standards must be adopted.

<!--more-->

### Performance and Browser support

Some of the Web Components specifications are not easy to implement in the browser while both keeping high 
standards for performance and providing support for a relatively wide range of browsers (think Android stock browser or IE9 for 
example that have not yet disappeared and are thus important to take into account). Some projects like Polymer have decided
to implement the entire standard set at all cost. On our side we decided to concentrate on what we consider the two most 
important pieces of Web Components, namely, the custom elements and template specifications. 

For these reasons, at least for now, we do not leverage specifications like Shadow DOM for which the trade-off between 
implementation and runtime cost vs. benefit is not obvious. In the long run, when Shadow DOM will be natively implemented 
we will probably leverage it, however in the short term we figured out we would get better results without it.

That said, even though we do provide our own small custom element shim for best performance/code size vs. feature ratio, 
when Polymer or native implementation is available we do rely on them to provide the feature and for this reason you 
will get full interoperability.

### JavaScript vs HTML centric ecosystem

While Web Components through the HTML import specification favor HTML-centric elements, where the HTML template is the 
core of the component, surrounded by CSS and JavaScript logic, on our side we do favor a slightly different approach
where the JavaScript logic is at the core of the element, and depends on the HTML template and CSS. We think this approach
has several advantages including:

  * from an MVC perspective, makes the Controller (the element's javascript) the central piece, while the View
  (HTML, CSS) is plugged into the element and can possibly differ while the JavaScript logic stays identical.
  * having ability to leverage [RequireJS](http://requirejs.org), the widely adopted [AMD](http://requirejs.org/docs/whyamd.html) loader 
   to benefit from an ecosystem of plugins allowing you to extend what your Web Component can consume in terms of dependencies
   as well as benefit from a full–fledged build system to easily create single file download for your element.

### Advanced features

Finally, while Web Components are definitely enough to build a UI element from scratch, when looking into building
enterprise class components you might want to abstract yourself a little bit from the low-level Web Components 
specifications and build on top of a set of higher level interfaces that provide you with the services you are 
looking for, rather than redesigning them each time you build a new element.

For this reason, delite provides a set of features on top of standards that make easier for you to build advanced UI elements. 
Typical examples are JavaScript AMD modules for managing your element selection state, access to data or form value. We
also provide theming management, allowing your element to automatically switch CSS theming based on its runtime
environment.

### In practice

With that said, let's be a bit more concrete and have a quick look at how a delite element is built:

The main file is an AMD module that will be loaded by RequireJS and contains the element logic as well as links
to its HTML template and CSS dependencies:

```js
define(["delite/register", "delite/Widget", "delite/handlerbar!template.html", "delite/css!style.css"],
  function(register, Widget, template) {
  return register("my-element", [HTMLElement /* you might extend another HTML element*/, Widget], {
    property: "default",
    template: template
  });
});
```

The template is just using the standard template tag to provide the rendering of the element. Handlebar-like syntax
can be use to provide reactive binding between the element properties and the template:

```html
<template>
  <label>{%raw%}{{property}}{%endraw%}</label>
</template>
```

More advanced binding can be achieved by leveraging alternate binding libraries in your component and template such as <a href="https://github.com/ibm-js/liaison">liaison</a>.

Finally the CSS is obviously providing the styling for the element:

```css
my-element {
  background-color: blue
}
```

The template and the CSS files are automatically loaded and applied when the widget is imported into the page, and 
you can then use it as any Web Component using its custom tag:

```html
<my-element property="value"></my-element>
```

In summary we hope that delite will not only _let_ you build a UI element, but will **help** you building it. You can learn 
more about the principles behind delite on our [site](/delite/index.html) and generate your first element using our 
[Yeoman generator](/delite/getting-started.html).

## Deliteful

The second project I wanted to introduce today, [deliteful](/deliteful/index.html), is a set of standard widget elements
(progress bar, list, slider...) built on top of delite.

### Multichannel

All of deliteful's elements are designed as multi channel widgets that can render and perform well both on desktop and mobile devices.
They do leverage CSS as much as possible instead of relying on JavaScript. In particular their layouts are thought
from a responsive design perspective making sure they will adapt automatically to the various form factors.

### Enterprise Class Feature Set

All those elements are sharing the goodness of delite and providing enterprise class features like full globalization 
(including bidi support), accessibility support (aria, keyboard, screen reader, high contrast) and are thoroughly tested 
on a wide range of devices.

### Standard Web Components

All deliteful elements are delite elements and for this reason are just regular custom elements that you can use in 
your application as any Web Component:

```html
<d-slider id="slider" on-change="rating.value=event.currentTarget.value" min="0" max="5"></d-slider>
<d-star-rating id="rating" min="0" max="5" readOnly="true">
```

This will enable a full interoperability of these elements with other <a href="http://webcomponents.org/">Web Components</a>.

You can learn more about deliteful on our [site](/deliteful/index.html) and generate your first application based on 
deliteful elements using our [Yeoman generator](/deliteful/getting-started.html).

## Conclusion

These two projects are still in their early phases, and you should still use them with caution as things will still evolve
and APIs are definitely not engraved in stone. However if you are interested in the direction we are following, we will
definitely be happy to hear from you. So please don't hesitate to check the projects out and provide feedback, 
either here, on the issue tracker or the mailing lists.

From now on, we will try to regularly share news and technical insights about those two projects and 
our [other open source projects](https://github.com/ibm-js) on this blog. So keep your eyes open!
 


