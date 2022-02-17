---
title: Chapter Three Summary
description: Summarizing Customized built-in elements
isPublicLesson: true
---

# Summary

In this chapter, you learned how to implement `ButtonComponent` with customized built-in elements. By extending a native HTML element, you retained the accessibility characteristics of a button. The primary benefit of customized built-in elements is the ability to extend existing HTML elements, thus retaining their default behaviors. In the example of the button component, this meant you didn't have to attach event listeners to handle events from keyboard input because the button element natively submits a form using the select or enter keys. Retaining the default behavior of native elements comes at a cost. For those elements that cannot implement Shadow DOM, styling has to be handled via a `<style>` tag either in the `<head>`, or when applicable, the `ShadowRoot` of another custom element. In later chapters, you'll correct this behavior when you encounter it.

You also learned how form-associated custom elements participate in accessibility. Since `TextInput` implemented `formAssociated`, instances of the element can be associated with `label` elements like any native form control. When you used a screen reader to navigate the form, you found the user was missing context from the validation messages found in the `ShadowRoot`. To provide the validation messages to the screen reader, you implemented WAI-ARIA attributes.

So far, we've covered the three kinds of custom elements: autonomous, form-associated, and customized built-in elements. In the `FormCard` story, you've experienced how all three custom element specifications work together to provide a custom, accessible, and reusable interface. You also learned about the other technologies that comprise the set of specifications known as Web Components: Shadow DOM and HTML templates.

This marks the completion of Part 1 of this book. You may be thinking, "if I just learned about the full specification, why would I need to learn more?" 

In Part 2, you'll handle some of the quirks and features of custom elements in a more elegant way by developing a microlibrary that enables custom element development. In subsequent chapters, you'll implement standard user interface patterns with custom elements and learn how to distribute a UI component library. 

In Part 3, you'll develop a web application with custom elements, learning how to handle client-side routing and server-side rendering along the way.

