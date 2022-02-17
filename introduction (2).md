---
title: Customized built-in elements
description: How to make accessible custom elements
role: "INTRODUCTION"
isPublicLesson: true
---

# Introduction

The internet should be accessible to everyone, including people with disabilities. When coding for the web browser, developers should consider users that require assistive devices to interact with the content. It may even be required by law. To conduct business with the U.S. Federal Government or follow best practices in accordance with the European Trade Commission (EU), web developers must follow [Web Content Accessibility Guidelines](https://www.w3.org/TR/WCAG20/) defined by the W3C Web Content Accessibility Group (WCAG). 

In the previous chapter, I mentioned that `ElementInternals` provides the entire Accessibility Object Model (AOM) to a form-associated custom element. AOM is a browser specification that attempts to reduce the friction developers face when implementing accessible user interface components. AOM provides properties on the DOM node that allow developers to programmatically read and write accessibility-based properties. 

Prior to the addition of AOM, developers were limited to WAI-ARIA, a set of HTML attributes that provide context to assistive technologies.

```html
<div id="buttonLabel">Do you agree?</div>
<input type="checkbox" aria-labelled-by="buttonLabel">
<div role="button">Continue</div>
```

Web developers tend to choose the path of least resistance, whether pressured by product owners or through lack of knowledge. As an example, let's look at the `button type="submit"`. Developers may not choose the `button` element because it is cumbersome to override the default styling. For whatever reason, a developer may opt to use a `div` and apply the WAI-ARIA `role` attribute to notify assistive devices this div is actually a `button`. 

```html
<div role="button">Submit</div>
```

If you used the techniques in the previous two chapters to develop a new button component using autonomous custom elements, you'd declare a new `class` that extends from the `HTMLElement`, although the cost is similar to the example of the `div`. Since you're not implementing an `HTMLButtonElement`, you lose all the accessibility characteristics of a native button, namely the user's ability to press the button using only the keyboard. The `HTMLButtonElement` allows users to press Enter or the spacebar on their keyboard instead of a mouse click. To gain these same accessible functions, you'd have to implement them on the `class` that extends the `HTMLElement`. 

[Customized built-in elements](https://html.spec.whatwg.org/multipage/custom-elements.html#custom-elements-customized-builtin-example) allow you to inherit the characteristics of native browser elements by enabling an extension from their base `class`. By extending from the `HTMLButtonElement` instead of the `HTMLElement`, the component inherits all the behaviors of the native HTML button element.


```javascript
const ButtonComponent extends HTMLButtonElement
```

Registering customized built-in elements is done in the same way. Although calling `define` requires a third argument that specifies an `Object` that sets the `extends` property to the tag name of the element from which you are extending.

```javascript
customElements.define('in-button', ButtonComponent, {extends: 'button'});

```

To use the customized built-in element in a template, the element must use the required `is` attribute set to the name of the element in the `CustomElementRegistry`. This is mainly so that browsers can parse the custom element from the HTML source.

```html
<button is="in-button">Submit</button>
```

It is possible to dynamically create customized built-in elements with `document.createElement` by adding a second argument when calling the function.

```javascript
const button = document.createElement('button', { is: 'in-button' })
```

At first it may seem a little awkward customized built-ins require different syntactical sugar or they may behave differently from autonomous or form-associated custom elements. Yet, the use value of this kind of customized built-in element is undeniable. Inheriting user-agent supplied behaviors when developing custom elements ensures users are already familiar with the characteristics of the component. The complexity of the component source is reduced when developers don't have to reimplement user-agent based behaviors. Customized built-in elements are essential for accessibility because the component retains accessible behaviors supplied by the browser. 

## What you will build

The most obvious use case for a customized built-in element is a button. In this chapter, you'll code the `ButtonComponent`, a customized built-in element that is used throughout the application. The component must work in and out of the context of the `HTMLFormElement`. Looking at [Figma](https://www.figma.com/file/QXGa6qN6AqgeerCtS28I8z/Web-Components-Book-Design-Library?node-id=0%3A1), the `ButtonComponent` has three different variants: primary, secondary, and icon. The component has different states for hover, active, focus, and disabled.

![](./public/assets/figma.png)

This exercise mostly involves styling the different states of the `ButtonComponent`. Since you'll retain all the accessibility characteristics of a button, there's no need to include any logic on the `class` other than dealing with the various interaction states. If engineers wish to further customize the button, they can do so by extending the `ButtonComponent class`.

Only some customized built-in elements can encapsulate styling and template with Shadow DOM: `HTMLBodyElement`, `HTMLDivElement`, ` HTMLHeadingElement`, `HTMLParagraphElement`, and `HTMLSpanElement`. This means the CSS for most customized built-in elements must be in a `<style>` tag external to the element. You could, as a rule of thumb, add a CSS classname to customized built-in elements so that an external stylesheet can target them. Traditionally, you would declare the CSS in the `<head>`, but if the customized built-in element is used within the context of another element's `ShadowRoot`, that styling can't cross the shadow boundary, leaving the element unstyled. To work around this limitation, you'll code a function that traverses DOM for the nearest `ShadowRoot` and injects the styles from there.

After completing the styling for the `ButtonComponent`, you'll circle back and check if the form is accessible. Using a screen reader, you'll make the form accessible by utilizing the `Accessibility Object Model` on the `ElementInternals` interface or through the use of WAI-ARIA attributes. 


## What you will learn

- What is a customized built-in element?
- What are the benefits of a customized built-in element?
- What is the Accessibility Object Model? (AOM)
- How do I provide a label for screen readers?
- How do I use a screen reader to test for accessibility?
- How do I provide an accessible validation message for screen readers?
- Why doesn’t a customized built-in element work in Safari?


## Acceptance criteria

- The `ButtonComponent` displays primary, secondary, and icon buttons
- The `ButtonComponent` is styled properly in the context of another element's `ShadowRoot`, or via stylesheet declared in `<head>`
- The user can navigate the form using a screen reader


## Getting started

Navigate to the project directory.

```
cd fullstack-web-components
```

Checkout a new branch named chapter-3 from where you left off at the end of chapter 2. 

```
git checkout -b chapter-3
```

Run Storybook using `yarn`. Storybook should appear, and the development environment is ready.

```
yarn storybook
```
