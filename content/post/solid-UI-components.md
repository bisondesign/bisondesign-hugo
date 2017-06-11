+++
date = "2017-06-07T20:21:11-05:00"
title = "SOLID Web UI Components"
draft = false
tags = ["UX","Design"]
image = "/images/jj-thompson-142854.jpg"
comments = true	# set false to hide Disqus
share = true	# set false to hide share buttons
+++

### Introduction

The five fundamental principles of Object Oriented Design known by the acronym
SOLID are well known in the software community ([SOLID][solid]). Originally
expressed by Robert C. Martin, these principles will allow a system to be more
easily maintained and extended over time. A side effect of these principles is
that the resulting system is loosely coupled. 

Similar principles would help the maintainability and evolution of complex UI
components in a multi vendor environment. Components are part of the view of an
MV* framework. The job of the view is to expose the model in the UI. The use of
composition in UI component design supports rapid development. It is one of the
many benefits obtained from using UI frameworks with existing libraries of
components. However the direct use of simpler components in a composite
component can lead to tight coupling and fragility. Also, the desire to reuse
components that are close but not a perfect match for requirements can lead to
the copying of those components, making upgrades difficult.

SOLID Web UI design patterns would address these issues. 

### SOLID in UI Component Design
Here are five principles whose initials form the acronym SOLID.  Each 
has its own acronym as well. 

Initial | Acronym | Concept
------- | ------- | -------
S | SRP |Single responsibility principle
O | OCP |Open/closed principle
L | LSP |Liskov substitution principle
I | ISP |Interface segregation principle
D | DIP |Dependency inversion principle

The argument from object oriented design is meant as an analogy. Still many of
the same principles apply to UI components. Just like complex classes are built
up from simpler classes, complex components can be built from simpler ones.
While [atomic][atomic] approaches to UI design are not new, they have different
goals.  Decomposition is a response to complexity.  SOLID design is a response 
to fragility in the face of change and builds on decomposition. 

The first three SOLID principles moderate the behavior of components and their
extensions.

* __Single responsibility principle.__ Components that take responsibility for
more than one thing can end up doing nothing well (explaining why there aren't
that many successful brain surgeon barbers). Complex UI's should be built up
from simple components by composition.

* __Open for extension but closed for modification.__ Once released, vendors
must never modify a component's behavior (contract). For the sake of reuse
components must also support extension.

* __Liskov substitution Principle.__ Components should support extensions.
Extension developers must ensure that the extended object still satisfies the
behavior contract of the base. If not the extension will seem buggy.

The last two SOLID principles support reuse through abstraction. Abstraction is
indirection based on type. Unlike objects, which have one interface, components
have two interfaces. The type of data that can be bound to a component and the
binding behavior (e.g. one-way vs two-way) supported by the component define
its model-facing interface. The configuration of the component, its supported
events, and the view contexts in which it can be used define its view-facing
interface. In theory a component could be replaced by any other that supports
the same two interfaces.

* __Interface segregation principle.__ If vendors combine simple components into
more complex components then the “interfaces” that define the bindings should
be factored. Broad interfaces are a pain to implement if all you need is a
narrow one. Customizers will be forced to create facades to bind to instead
of binding to the model directly.

* __Dependency inversion principle.__ Composition is a great way of building up
complexity. Dependency inversion means identifying dependencies with interfaces and
satisfying them only at run time (referred to as 'wiring' the entity). The

### SOLID UI Component Use Case

Frameworks like Angular 2.0, Aurelia, and React use components to support the
creation of Web UIs. HTML 5 supports the creation of Web Components natively.
So why do we need the five SOLID patterns to govern our use of web components?
In most cases they are not needed since Web UIs are small and static. 

A more disciplined approach is needed when a library of components is created
by a vendor and reused at scale by the customers of that vendor. The same is
true when the vendor is one department of an organization creating a library to
be used by other departments. In both of these cases updating the library must
be seamless, from the point of view of the "customers".

This is where SOLID shines.  These principles allow maximum reuse with minimal 
coupling between the components as a means to productivity and robustness.  

Reuse comes in two dimensions. The first is akin to "inheritance" when a
component is extended to add features or modify presentation without breaking
the contract of the base component. In this case the extension can act as a
plug-in replacement for the base.

Extension can be done with direct references to the base component to keep the
implementation simple. Avoiding indirection is a pragmatic decision since
indirection always comes at a price (e.g., cognitive complexity and performance
to name a few).  Extensions are typically done at a lower level, for example
when creating new widgets for a library.

The second type of reuse is when simpler components are combined into more
complex components. This is a more typical scenario done at a higher level when
pages or mashups are being designed. In this scenario the references to the
inner components should remain abstract allowing the wiring to a concrete
component to be done on initialization. The decision can vary by context and
configuration and even by data type.  So for example, a table of items in a
shopping cart can choose a "template" to instantiate based on the type of item.

In both case (extension and composition) the bindings of the inner components
to the model are left open to be hooked up dynamically to the model at run
time.  View facing configuration (related to layout) should be hard-coded. 

### Implementation

The ability to create components is built into HTML 5. [Web Components][wc] are
a group of standards proposed as a W3C specification. They allow you to create
new HTML elements that are reusable, composable, and encapsulated. How do you
write SOLID UI components as HTML 5 web components?

You will want to start by splitting your components in to modules. When
projects get to a certain size, they will have to deal sooner or later with the
'dependency hell' problem. This happens when two different versions of the same
library are needed at the same time.

Binding is always done on the client, as that is where the events are triggered
and consumed.  Static binding is too restrictive as well, as the shopping cart 
example showed. 

There are two distinct paths we can pursue for wiring (creating the view tree
with dependency resolution) depending on where it will take place. It can be
done on the client using templates, or it can be done dynamically on the
server as data is added to the model that needs to be displayed in the view. I
prefer a template approach on the client. Templates can be stored in HTML, the
approach chosen by Aurelia. 

```html
<template id="aboutOurTeam">
  <h1>About</h1>
  <hr>
  <p class="lead">${aboutMessage}</p>
  <hr>
  <h2>Our Team</h2>
  <div repeat.for="p of profiles">
    <compose model.bind="p" view-model="profile"></compose>
  </div>
</template>
```

Here the page [template][template], given a binding context that includes a 'profiles'
array can loop over the entries of this array and for each include a 'profile'
view model binding it's context to the given entry. The context for the template is 
established when it is adopted into the DOM tree of the view. 

```javascript
 var template = document.querySelector('#aboutOurTeam');
 for (var i = 0; i < data.length; i += 1) {
   var cat = data[i];
   var clone = template.content.cloneNode(true);
   var cells = clone.querySelectorAll('td');
   // ... establish context and recursively compose children
   template.parentNode.appendChild(clone);
 }
```

The `compose` element in the example above is a [custom web element][custom]
defined by Aurelia.

How is this approach SOLID? 

* SRP: The 'AboutOurTeam' view model has a single responsibility. Composition
and binding allow the templates themselves to remain simple. Here the focus is
on the headings, message, and the list of team members.

* OCP: Extending a template can be done by means of the [Shadow DOM][shadow], 
another HTML 5 feature. This allows an element's content to be replaced by 
completely encapsulated HTML markup that has access to the original element.  

    > Aside. Suppose you created a widget that contained an `h2`, a `p` and a text
    `input` field to allow users to rate the content (the text can be `*` to `*****`).
    Now you wanted to replace the input field with a star rater. It is possible to
    shadow the original widget, keeping the `h2` and `p` visible while displaying a star
    rater widget whose value feeds the invisible text field.

* LSP: Careful delegation to the base of an extended component, as in the above
example, ensures that the original contract is honored.  The component interacts
with the user in a different way - its view-facing interface has changed - but it
will still be plug-in compatible with the original widget component. 

* ISP: Models are JavaScript objects. They can be decomposed into arrays, maps,
or primitive values (string, number, or boolean). Loops decompose arrays. Name
bindings decompose maps. Finally, primitives can have only a finite set of
semantic types (e.g. number can represent an integer, decimal, money,
percentage, distance, time, etc). These few semantic types make it possible to
support a finite set of primitive interfaces for data binding.

* DIP: The 'AboutOurTeam' view model had a dependency on the `profile` view model. 
This dependency may resolve to a concrete type (`profile.html` and `profile.js` in
aurelia).  But you can imagine a system where additional logic was involved in 
selecting a view model using additional information about the data instance to be
bound and also the context.  In particular resolution can take into consideration: 

    * Context in the view
    * The type of data being bound (e.g. an extension of `profile`)
    * User based configuration

### Missing Details

__Dynamic Behavior__. I have not touched on the more dynamic aspects of the UI
such as binding and support for events. Needless to say this alone is a vast
area already well covered by many frameworks. For example a [reactive][redux]
approach that eschews binding and relies for events alone would work well. But
other approaches would also work.

__Extension__. The approach given here needs a more concrete description of how
to use the shadow DOM to extend components without modification (OCP). Some
methods to simplify the repetitive aspects of extension and to preserve the
contract of the base (LSP) would be needed as well.

__Base Properties__. All components should derive from a common base class to
supply the view-facing interface. Common properties would include an id, a
name, x, y & z coordinates, width and height, and a boolean for visibility.

__Events__.  Events (e.g., onClick) that are specific to a component type (the
[EventTarget](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget),
e.g. `button`) will be present on all instances of that type. Listeners must
register with the target and implement the event listener interface:

```javascript
interface EventListener {
  void handleEvent(in Event evt);
}
```

All the information needed by the listener will be present in the
[Event](https://developer.mozilla.org/en-US/docs/Web/API/Event) object,
including the target object. Event listeners can subscribe to one or more
events. Likewise, one or more listeners can subscribe to any given event
(making it a multi-cast event).

__Other Properties__.  The properties of the base and derived component
share these characteristics:

1. they can be bound for reading (if binding is supported)
1. they are exposed as attributes of the element
1. they are exposed programmatically via setter and getter methods
1. they can be tested in css for conditional formatting

__Composition__. The method by which multiple components are composed into a
compound component should be defined. The interface of the compound component
is the union of the interfaces of the children components that are bindable
from the aggregate.  

__Dependency Injection__.  The DIP is already implemented in some systems via
dependency injection.  This is the case with Aurelia for example.  In others
it will require explicit wiring code.  The wiring should check compatibility
of a component with the interface that it must satisfy. Wiring can still be
done manually but the exact mechanism must be explained. 

### Conclusion

In this blog I introduced the concept of SOLID Web UI components. I explained
how these five principles borrowed from software design can be applied to
component design and when they should be applied. By following these principles
a custom UI will be less likely to break when the underlying libraries are
upgraded. Finally, I showed how extensibility can be integrated into a UI
component library using some HTML 5 features and existing frameworks. 

In the future I would like to implement http://todomvc.com/ using 
this approach. 

[atomic]: http://patternlab.io/
[solid]: https://en.wikipedia.org/wiki/SOLID_(object-oriented_design)
[wc]: https://en.wikipedia.org/wiki/Web_Components
[wc2]: http://w3c.github.io/webcomponents/explainer/
[jspm]: http://jspm.io/
[jspm2]: http://blog.angular-university.io/introduction-to-es6-modularity-the-jspm-package-manager-and-the-systemjs-loader/
[compose]: https://wpdevkvk.wordpress.com/2016/09/18/building-apps-with-aurelia-5-using-compose-element-to-implement-mvvm/
[template]: https://html.spec.whatwg.org/multipage/scripting.html#the-template-element
[custom]: https://w3c.github.io/webcomponents/spec/custom/
[redux]: http://redux.js.org
[shadow]: https://w3c.github.io/webcomponents/spec/shadow/
