---
layout: post
title: Consolidation Milestone
author: cjolif
---

I'm pleased to announce a new milestone for delite, deliteful and requirejs-dplugins as well as a few other projects. 
Here are a few highlights about this release:

* We are now consistently using ES6 Promises across the various projects. We achieved that through the 
[Promise](http://ibm-js.github.io/requirejs-dplugins/docs/0.5.0/Promise.html) 
plugin of the [requirejs-dplugins](http://ibm-js.github.io/requirejs-dplugins/index.html) project which delegates either 
to the native ES6 implementation or to the [Lie](https://github.com/calvinmetcalf/lie) JavaScript implementation 
depending on native browser support presence.
* We extended the use of jQuery for DOM manipulation from delite to deliteful.
* We also added a first delite [tutorial](http://ibm-js.github.io/tutorial/docs/0.6.0/index.html) on how to build you own custom element widget with delite.
* We improved documentation and added a documentation page on [styling deliteful elements](http://ibm-js.github.io/deliteful/docs/0.6.0/styling.html) 
* We have various bidirectional text improvements both at delite & deliteful level.
* Combobox is finally converging with keyboard support as well as multi channel policy support based on the new 
channelBreakpoints module that allows you to define the default breakpoints used in the application.
* There were various cleanups on form widget base classes as well as on the CSS of the deliteful widgets (more to come)
* We now have in the handlebars plugin a requireAndCompile() method for code that needs to load the templates manually 
  rather than using handlebars! as a plugin
  
As you might have noticed we did not add new custom elements in this milestone and we concentrated on consolidating 
what existed before. Hopefully next milestone should bring us new widgets like [TabBar](https://github.com/ibm-js/deliteful/issues/176) or 
[DropDown button/menu](https://github.com/ibm-js/deliteful/issues/426).

Thanks to anyone that contributed to this milestone, in particular, Adrian, Bill, Clement, Damien, Ed, Lee, Semion and Youcef!

<!--more-->

Note that we have a number of incompatibilities:

  * at delite level, the move to ES6 Promise has forced changes on the `delite-display-load` event API for `DisplayContainer`
  * still for delite, the `Widget.startup()` method has been deprecated. For programmatically created widgets we encourage
  you to use `placeAt()` instead.
  * HasDropDown no longer sets CSS classes for the arrow direction
  * at deliteful level, the [SwapView]() viewstack property has been renamed to viewStack (camel case).
 
If you need more details please checkout the [delite](https://github.com/ibm-js/delite/releases/tag/0.6.0) & 
[deliteful](https://github.com/ibm-js/deliteful/releases/tag/0.6.0) release notes on github.




