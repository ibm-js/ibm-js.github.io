---
layout: post
title: "ComboBox or Select: which one to pick?"
author: AdrianVasiliu
---

As announced in this [previous post](http://ibm-js.github.io/2014/10/24/0.4.0-counting.html), 
the [deliteful](http://ibm-js.github.io/deliteful) project now includes a preview of
the [ComboBox](http://ibm-js.github.io/deliteful/docs/master/ComboBox.html) widget. 
This post aims to:

* Explain how the new widget complements the existing 
[Select](http://ibm-js.github.io/deliteful/docs/master/Select.html) widget.
* Provide a guidance about the criteria for choosing which of the two widgets fits better 
depending on application needs.

The widgets `ComboBox` (custom tag: `d-combo-box`) and `Select` (custom tag: `d-select`)
share a number of characteristics:

* Allow to select one or more items among a number of options (in single or multiple 
selection modes).
* Benefit from [delite](http://ibm-js.github.io/delite)'s 
[CustomElement](http://ibm-js.github.io/delite/docs/master/CustomElement.html) and
[Widget](http://ibm-js.github.io/delite/docs/master/Widget.html) infrastructure.
* Store-based: render data provided by a data store object.
* Form-aware: inherit from [FormWidget](http://ibm-js.github.io/delite/docs/master/FormWidget.html).
* Multi-channel responsive: they render differently on mobile than desktop, thus
providing a user-experience adapted to each. (For `ComboBox`, among other improvements
that will come in a future release, the multi-channel policy and in particular its 
configurability will be revisited; see [delite #366](https://github.com/ibm-js/deliteful/issues/366)).
* Can be used both programmatically and in markup (including for specifying the options, 
thanks to the new [`d-store`](http://ibm-js.github.io/deliteful/docs/master/Store.html)
custom element.

The two widgets also differ by other features; only `ComboBox` provides:

* Powerful 
[rendering customization mechanism](http://ibm-js.github.io/deliteful/docs/master/list/List.html#customRenderers)
for the options and 
[categorization](http://ibm-js.github.io/deliteful/docs/master/list/List.html#categories).
* [Interactive filtering](http://ibm-js.github.io/deliteful/docs/master/ComboBox.html#using)
of the options by typing one or more characters (optionally).

<!--more-->

The key differenciator of the two widgets is the fact that `Select` leverages the
HTML `select` element, while `ComboBox` leverages (for the popup) the 
[List](http://ibm-js.github.io/deliteful/docs/master/list/List.html) widget.
The use of the HTML `select` brings to the `Select` widget the handling of the popup 
natively by browsers. This has significant advantages: the rendering adapts automatically
depending on browsers and platforms in an optimal manner; for instance, Safari 
on iOS provides a native spinwheel-like control; also, no JavaScript code is needed
for handling device orientation changes, or for adaptation to screen size. This makes
the `Select` widget very light, performant, and robust. On the other side, this also brings
limitations. For instance, on many browsers, if the options change dynamically in 
the DOM, the popup closes itself, thus it is not possible to implement a dynamic
filtering depending on characters the user would type while the popup is open. Also,
the HTML `select` element supports text-only rendering for the options.

These limitations are avoided by `ComboBox` by leveraging the 
[List](http://ibm-js.github.io/deliteful/docs/master/list/List.html) widget for
rendering the options. The downside is that it is heavier (in terms of code size and
memory footprint), less robust and less performant (JavaScript code is used for handling
the positioning and sizing of the popup).

Finally, here is a quick guide for picking the most appropriate widget depending
on the needs of your app:

* Pick `Select` if:
  * You need text-only rendering of options, and
  * You do not need the interactive filtering capabilities, that is typing characters to
  filter the shown options depending on whether they match (for a relatively small number
  of options this feature is usually not mandatory; on mobile it can be more annoying to get
  the keyboard open than to just touch the option you need). 
* Pick `ComboBox` if:
  * You need full customization of the rendering of options (text-only is not good enough), or
  * The number of options is high hence you need the interactive filtering capabilities.

