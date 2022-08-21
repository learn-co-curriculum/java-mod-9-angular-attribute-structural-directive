# Attribute and Structural Directives

## Learning Goals

- Explain attribute directives.
- Explain structural directives.

## Attribute Directives

Attribute directives are directives that can be applied to existing HTML
attributes to change their appearance and/or behavior.

Let's create a custom attribute directive that applies a background color to the
HTML element it's applied to.

We can generate a custom directive with the following command:

`ng generate directive highlight`

This asks the CLI to create a directive named "highlight" - the CLI then does
the following:

1. Creates a `ts` file for the directive test named
   `highlight.directive.spec.ts` (we'll ignore this for now)
2. Creates a `ts` file for the actual code for the directive named
   `highlight.directive.ts`
3. Updated our `app.module.ts` to add our new directive to the `declarations`
   array

Let's look at the code for the directive:

```typescript
import { Directive } from "@angular/core";

@Directive({
  selector: "[appHighlight]",
})
export class HighlightDirective {
  constructor() {}
}
```

This is a structure we're now familiar:

1. We define a class that has the name of the directive
2. Since this is a special entity that Angular wants to know things about, we
   use an annotation to decorate it
3. In the annotation, we specify the "selector" that will be used to let Angular
   know that we want this directive applied to a specific HTML element

How do we actually get the directive to do anything though?

Since we have told Angular this is a directive with the `@Directive` annotation,
we can ask Angular to inject a reference to the element that this directive is
attached to in its constructor:

1. We import `ElementRef` from Angular's core package:
   `import { Directive, ElementRef } from '@angular/core';`
2. We ask Angular to a variable of type `ElementRef` into the constructor:
   ` constructor(private el: ElementRef)`
3. We can then use this `el` variable to access the actual element and change
   any of its attributes
4. In this case, we choose to access its current style and give it a
   `backgroundColor` of `red`

Here is our updated `highlight.directive.ts` file with all those changes
applied:

```typescript
import { Directive, ElementRef } from "@angular/core";

@Directive({
  selector: "[appHighlight]",
})
export class HighlightDirective {
  constructor(private el: ElementRef) {
    this.el.nativeElement.style.backgroundColor = "red";
  }
}
```

> Note that when applying CSS styles directly in HTML, the naming convention is
> to separate words with "-", like "background-color", while when referring to
> CSS styles in TypeScript or JavaScript, the naming convention is to use camel
> casing, like "backgroundColor"

We are now ready to actually apply this directive - let's do so in our header
component:

```html
<div class="container">
  <div class="row">
    <div class="col-9 p-3">
      <h2 appHighlight>Welcome, {{ activeUser.firstName }}</h2>
    </div>
    <div class="col-3 p-3 float-end">
      <div class="float-end">
        <button class="btn btn-primary">Logout</button>
      </div>
    </div>
  </div>
</div>
```

With that directive applied, the "welcome" message will now have a red
background.

## Structural Directives

The directives we've learned about in this section so far have had the ability
to modify the attributes of the HTML element they're associated with.

Structural directives, on the other hand, have the ability to modify the DOM
completely by adding or removing entire elements from it. Those directives are
marked with a "*" in front of the actual directive name, like
`*ngFor`and `\*ngIf`.

Since we have already seen and used both these directives earlier in this
course, we won't be going over them in details here. One thing we didn't cover
before, however, is that structural directives cannot be combined in a single
element. So if some content in your view needs to be repeated _conditionally_,
then you will need to structure content in such a way that the repeated content
is inside a block that itself can be made conditional.
