---
title: Scaffolding ButtonComponent
description: Scaffolding a customized built-in element in Storybook
isPublicLesson: true
---

## Scaffolding ButtonComponent

If the following steps look familiar by now, that's because they resemble the same process for scaffolding `TextInputComponent` and `CardComponent` from the previous two chapters. First, you'll declare a new custom element, this time registering the component as a customized built-in element. Then, you'll import that component into a new story file where all the `ButtonComponent` stories will be declared. Throughout the chapter, you will code several stories to visualize the different variants of the `ButtonComponent`: primary, secondary, and icon, as well as at least one story that demonstrates the disabled state.

To get started, make a new TypeScript file called Button.ts in a new directory packages/component/src/button.

```bash
mkdir -p packages/component/src/button
```

In the input directory, make a new file and name it TextInput.ts.


```bash
touch packages/component/src/button/Button.ts
```

In Button.ts, export a new `class` named `ButtonComponent` that extends `HTMLButtonElement`. Give the `class` a `constructor`, making sure to call `super` so that when the component initializes, it also instantiates the constructor of the `HTMLButtonElement class`. 

{lang=javascript,line-numbers=off,crop-start-line=1,crop-end-line=5}
<<[Button.ts](./protected/src/Button.start.ts)

One difference you should notice by now between `ButtonComponent` and the last two components you coded is the `class` no longer extends from `HTMLElement` but instead extends from `HTMLButtonElement`, an interface that provides methods and properties common to the native `button` element. [Find a complete reference of `HTMLButtonElement` on MDN](https://developer.mozilla.org/en-US/docs/Web/API/HTMLButtonElement). 

There are at least two advantages gained by extending from `HTMLButtomElement` as opposed to `HTMLElement`. `HTMLButtonElement` natively participates in forms by implementing the `form`, `formAction`, `formEnctype`, `formMethod`, `formNoValidate`, and `formTarget` properties. If you had opted for a form-associated custom element, you would have to provide the `ElementInternals` interface to make the button participate in the form. In the above example, you don't have to do anything. You get form participation for free. By extending from a native element, the component inherits all the behaviors of that element. Another example may not be evident looking at the documentation for `HTMLButtonElement`. The button element includes features that promote accessibility out of the box, including the ability to press the button using the Enter key or Spacebar. Had you chosen to extend from `HTMLElement`, you would have to duplicate this functionality with a custom implementation.


Like the previous two components, you register the `ButtonComponent` on the `CustomElementRegistry`, this time passing in the optional third argument, an `Object` with `extends` property set to `button`. The `extends` property is necessary to tell the browser you're registering the element that progressively enhances a native element.


{lang=javascript,line-numbers=off,crop-start-line=7,crop-end-line=8}
<<[Button.ts](./protected/src/Button.start.ts)


Create a new file named Button.stories.js in the same directory as Button.ts. 

```bash
touch packages/component/src/button/Button.stories.js
```

Import `ButtonComponent` into Button.stories.js.

{lang=javascript,line-numbers=off,crop-start-line=4,crop-end-line=4}
<<[Button.stories.js](./protected/src/Button.stories.js)

Declare the story by exporting a default `Object` that sets the `title` property as `Components/Inputs/Button` and the `component` property as `in-button`.

{lang=javascript,line-numbers=off,crop-start-line=12,crop-end-line=15}
<<[Button.stories.js](./protected/src/Button.stories.js)

Before you declare the first story template, import `html` from the lit-html package since Storybook uses LitHTML to render Web Components.

{lang=javascript,line-numbers=off,crop-start-line=1,crop-end-line=1}
<<[Button.stories.js](./protected/src/Button.stories.js)

Declare a new `const` named `Template`, and make it equal to a `function` that returns `html`. Pass a new template string to `html`, and include a button element in the template, making sure to use start and end button tags. A quirky requirement of customized built-in elements is the requirement of declaring the `is` attribute on the button element, setting the name of the registered element, in this case, `in-button`. 

{lang=javascript,line-numbers=off,crop-start-line=17,crop-end-line=22}
<<[Button.stories.js](./protected/src/Button.stories.js)

To support the three button variants: primary, secondary, and icon, you could set an attribute and use `observedAttributes` and `attributeChangedCallback`, but that seems like overkill when you could achieve the same with a CSS classname. The variants, after all, are just various styles of the same button element. We need a way to set the variant in the story, so pass in the `variant` using destructuring to the `class` attribute. Finally, pass in a `label` in the same way, setting the content of the button element with the value of `label`.

Next, export a `const` named `Primary` and make it equal to `Template.bind({})`. Set the `Primary.args`, passing in the `variant` and `label`. This establishes the first story for the primary variant in Storybook.

{lang=javascript,line-numbers=off,crop-start-line=24,crop-end-line=28}
<<[Button.stories.ks](./protected/src/Button.stories.js)
