---
layout: post
title: Custom Elements limitations and solutions
author: cjolif
---

Since [delite](http://ibm-js.github.io/delite/)'s inception we are leveraging Web Components and in particular [Custom Elements](http://w3c.github.io/webcomponents/spec/custom) specification. We are pretty happy with this choice that I exposed in my [introducing blog post](http://ibm-js.github.io/2014/07/18/delite-and-deliteful.html) and we hope this common Web extension mechanism will bring interoperability between the various UI frameworks & widget sets on the mid/long term. 

That said we are sometimes facing some limitations with custom elements that are annoying and might slow down its adoption. I wanted to highlight two of them in this blog post, hoping to bring some more awareness around those.

---

The most common one, that was already mentioned by others in the past, is the lack of namespacing or scoping for the elements. Let's say someone comes up with a `d-list` custom element just as [we did](http://ibm-js.github.io/deliteful/docs/0.6.0/list/List.html), then any application developer bringing the two pieces of JavaScript in the application will have a clash between the two elements. Fortunately an error will be thrown when the second element is registered and some workarounds at application level like redefinition of the `Document.registerElement()` method can deal with that. However that workarounds are not satisfactory as they don't really solve the root issue of the specification giving room to potential registration clashes. 

This lack of namespace management is kind of contradicting what I think should be the ultimate custom element role, that is interoperability.

For now we have decided to not deal with that issue at delite level except by using the `d-` prefix for all our components and hoping nobody else will use that same prefix.

---

Another less common limitation that we faced recently is the two steps process of custom elements interpretation. The elements are first seen as `HTMLUnknownElement`, and as soon as `Document.registerElement()` has been called they are upgraded to actual `HTMLElement` of their declared type. There are two issues behind that:

1. this process easily happens asynchronously (i.e. `Document.registerElement()` is called when the corresponding element JavaScript happens to be loaded so in an  order you don't necessarily control depending on your application setup)
2. during its creation/attachment process a given custom element might need to reach out other elements. 

For example:


```html
<my-data-store-element id="a-data-store-element"></my-data-store-element>
<my-ui-element ref="a-data-store-element"></my-ui-element>
```

will certainly have code like the following in `my-ui-element` implementation:

```js
attachedCallback: function () {
  var store = element.ownerDocument.getElementById(this.getAttribute("ref"));
  // get information and stuff on the store element
}
```

Depending on the indeterminate timing that custom elements are downloaded and registered, and `my-ui-element.attachedCallback()` might try to access the `a-data-store-element` before it has actually been upgraded to a custom element. For this reason the store element might not be "ready" to be used leading to incorrect setup of `my-ui-element`.

For this reason it would be useful to be notified when an element has really been created (and attached) as an actual custom element so that other elements can react to this. 

This one was actually hurting us in a very similar situation to the one I described just above. See for example:

<iframe width="100%" height="240" src="http://jsfiddle.net/ibmjs/yw9b3r8q/embedded/html,result,js,css" allowfullscreen="allowfullscreen" frameborder="0">
<a href="http://jsfiddle.net/ibmjs/yw9b3r8q/">checkout the sample on JSFiddle</a></iframe>

where `d-list` element connects to a `d-store` element.

As this was blocking us we decided to not wait on a possible specification improvement as for the namespace issue and at least for now implement our own mechanism. The solution here was to dispatch a [`"customelement-attached"`](http://ibm-js.github.io/delite/docs/api/0.4.0/delite/CustomElement.html#event:customelement-attached) event after execution of the `attachedCallback` of any delite element. This allows an element to wait for another element to have been upgraded to custom element and attached.

You can then modify the `attachedCallback()` above as follows:

```js
attachedCallback: function () {
  var store = element.ownerDocument.getElementById(this.getAttribute("ref"));
  if (store instanceof HTMLElement) {
    // get information and stuff on the store
  } else {
  	// wait for it to be ready and proceed
  	store.addEventListener("customelement-attached", function () {
  	    // get information and stuff on the store
  	});
  }
}
```

Ideally I think this should not be a delite-specific event but that Custom Elements specification should provide a similar event so that created & attached callback can notify other elements of their execution.

What do you think? Is that something you see missing in Custom Elements specification or is that just me needing this event to coordinate elements creation & attachment? 
