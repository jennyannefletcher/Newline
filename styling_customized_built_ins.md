---
title: Styling ButtonComponent
description: How to style customized built-in elements
isPublicLesson: true
---

## Styling the ButtonComponent

While extending the `HTMLButtomElement` has benefits, it means that Shadow DOM is not available in the context of the `ButtonComponent`. That's because the native button element cannot have a `ShadowRoot`, so we lose the ability to encapsulate styling for the customized built-in element. This means you'll have to rely on CSS styles in a `<style>` tag external to the button element. Since you won't be able to target the button with `:host`, specify a unique CSS class name that can be used to select the custom element. The only place to do this safely in a customized built-in element is the `connectedCallback` lifecycle.

In Button.ts, add the `connectedCallback` method and use the `add` method on `classList` to give the button a class name when added to DOM.

{lang=javascript,line-numbers=off,crop-start-line=5,crop-end-line=7}
<<[Button.ts](./protected/src/Button.class.ts)

Inspect the element with Dev Tools and notice the `primary` and `in-button` class names are set, the first by Storybook and the second through the `connectedCallback`. The element should have the `is` attribute set to `in-button`, the name of the registered customized built-in element.

![](./public/assets/class.png)

### Primary

You don't have to give up modern conveniences like CSS-in-JS even though the CSS has to be declared in an external `<style>` tag that can be accessed by instances of the `ButtonComponent`. In Button.ts, declare a `const` named `buttonStyles` and make it equal to a template string. Select the primary button variant with the selector `.in-button.primary` and set the styling per the mockup in Figma. 

{lang=javascript,line-numbers=off,crop-start-line=1,crop-end-line=15}
<<[Button.ts](./protected/src/Button.head.ts)

To get the button styles in DOM, add a new method on the `ButtonComponent class` called `addStylsheet`.

{lang=javascript,line-numbers=off,crop-start-line=25,crop-end-line=34}
<<[Button.ts](./protected/src/Button.head.ts)

In this method, create a new style element using `document.createElement` and set the `id` attribute to `in-button-style`. Using `textContent`, set the `buttonStyles` you declared earlier as the content of the style element, then append the style element to the `document.head`. When there are multiple instances of the `ButtonComponent` in the same view, we don't want to keep adding the same stylesheet to the `document.head`, so implement a conditional that prevents this from happening.

When you are finished coding the `addStylesheet`, make sure to call the method in the `connectedCallback`. 

{lang=javascript,line-numbers=off,crop-start-line=21,crop-end-line=24}
<<[Button.ts](./protected/src/Button.head.ts)

In Storybook, you should see a button that is styled as the primary variant.

![](./public/assets/primary.png)

Inspect the`<head>` tag in Dev Tools and observe a new `<style>` tag with the CSS declared as `buttonStyles` injected into the style element. If you have trouble seeing the tag, ensure the JavaScript context is set to storybook-preview-iframe in Dev Tools.

![](./public/assets/head.png)

That's it for the default primary styles. Style the secondary and icon variants before you work on the styles for each button state: disabled, active, and focused. That'll save time because looking at the Figma, it appears all the buttons share the styling for the various states, while they differ in their default styles.

### Secondary

Declare another story in Button.stories.js for the secondary button variant. Name the story `Secondary` and make sure to call `Template.bind({})`. Pass in `variant` and `label` through `Secondary.args`, setting the `variant` to `secondary`. 

{lang=javascript,line-numbers=off,crop-start-line=30,crop-end-line=34}
<<[Button.stories.js](./protected/src/Button.stories.js)

In Figma, the secondary buttons appear to have a white background with a blue border. The label is the same color as the border. Update `buttonStyles` with the CSS for the secondary button. To select secondary buttons, use the `.in-button.secondary` selector.

In Button.ts, style the element based on the mockup in Figma by adding the secondary styles to `buttonStyles`.

{lang=javascript,line-numbers=off,crop-start-line=16,crop-end-line=28}
<<[Button.ts](./protected/src/Button.variants.ts)

When you are finished, the default secondary button should be styled like the example below.

![](./public/assets/secondary.png)

### Icon

Scaffolding the icon button variant requires a little more work than primary and secondary. You'll need a way to source an SVG to place in the button in lieu of the text label. FontAwesome is installed in the repository to help you achieve this goal. In Figma, the example icon button is used throughout the application as the "close" button. This icon could be used to close dialogs and menus. The closest icon FontAwesome provides is a "plus" icon, so you'll need to use CSS to rotate the "plus" 45 degrees to display a "cross." You can achieve this style through a CSS classname modifier specific to the close button. While all icon variants should use the CSS classname `icon`, this specific icon will require the CSS classname `icon-close`. 

FontAwesome provides a package called `@fortawesome/fontawesome-free`, which is already installed in the repository and integrated with Storybook. In Button.stories.js, use the `window.FontAwesome.icon` method to select the "plus" SVG. Set a `let` named `icon` to the SVG returned by `window.FontAwesome.icon`.

{lang=javascript,line-numbers=off,crop-start-line=6,crop-end-line=10}
<<[Button.stories.js](./protected/src/Button-svg.stories.js)

If you inspect `icon`, you'll observe the `Object` provides an interface for consuming an SVG. The SVG can be accessed via `icon.node[0]`. In case you need to process the SVG later, set a new `const` named `svg` to the SVG node. 

{lang=javascript,line-numbers=off,crop-start-line=12,crop-end-line=12}
<<[Button.stories.js](./protected/src/Button-svg.stories.js)

Scaffold the Icon story, making sure to export a `const` named `Icon`. This time, specify the `variant` as `icon icon-close`. Since `variant` merely sets the `class` attribute on the button element in `Template`, the element can now be styled with both classnames. Set the `label` property to the `svg`. The SVG node will be inserted into the content of the button element using LitHTML.

{lang=javascript,line-numbers=off,crop-start-line=38,crop-end-line=42}
<<[Button.stories.js](./protected/src/Button-svg.stories.js)

Now that everything is scaffolded for the icon variant, proceed to style the button. In `buttonStyles`, select the icon variant with `.in-button.icon` CSS selector, and while referencing Figma, style the element in the CSS declaration block.

{lang=javascript,line-numbers=off,crop-start-line=30,crop-end-line=46}
<<[Button.ts](./protected/src/Button.variants.ts)

To ensure the SVG fits within the dimensions of the 44px button, select the `svg` with a CSS selector and style its `width` and `height` to `100%`. Since the icon variant has some padding, the SVG will actually have a smaller display. To transform the "plus" into a "cross", select the `svg` contained by the element with the class name `.icon-close` and use CSS `transform` to rotate the SVG `45deg`.

{lang=javascript,line-numbers=off,crop-start-line=55,crop-end-line=62}
<<[Button.ts](./protected/src/Button.style.ts)

When you have finished, the icon variant should appear by default as it does in Figma.

![](./public/assets/icon.png)

### Interaction States

Referencing [Figma](https://www.figma.com/file/QXGa6qN6AqgeerCtS28I8z/Web-Components-Book-Design-Library?node-id=0%3A1) again, you notice each button variant shares the same interaction states. Let's start by styling the focus state. To style the focus state, select each of the variants with the `:focus` CSS pseudo selector. In the CSS declaration block, style the focus state as it appears in Figma.

{lang=javascript,line-numbers=off,crop-start-line=64,crop-end-line=71}
<<[Button.ts](./protected/src/Button.style.ts)

The active state is styled in the same way, this time selecting each variant with the `:active` pseudo selector. Style the active state according to the mockup.

{lang=javascript,line-numbers=off,crop-start-line=73,crop-end-line=80}
<<[Button.ts](./protected/src/Button.style.ts)

All the variants should now have the focus and active states styled. Test out these new behaviors in Storybook by navigating to the `Primary`, `Secondary`, and `Icon` stories. Click on the whitespace in the Storybook canvas and then focus on the button element by pressing the Tab key. With the button focused, press the Spacebar or Return key on the keyboard to see the active state appear.

Buttons have a disabled state as well, usually set with the `disabled` attribute on the button element. To achieve the disabled state in Storybook, declare a new template in Button.stories.js named `DisabledTemplate`. This template should look remarkably similar to `Template` except it has the disabled attribute set.

{lang=javascript,line-numbers=off,crop-start-line=44,crop-end-line=56}
<<[Button.stories.js](./protected/src/Button-svg.stories.js)

In Button.ts, style the different variants using the CSS attribute selector `[disabled]` and style the disabled state per the mockup in Figma.

{lang=javascript,line-numbers=off,crop-start-line=89,crop-end-line=97}
<<[Button.ts](./protected/src/Button.style.ts)

When the button is disabled, the element should not appear focused or active. This means you need a way to override the focus and active styles declared earlier. Using CSS specificity, select each of the variants with `:focus` and `:active` pseudo selectors, this time increasing the specificity of the selection by adding the `[disabled]` attribute selector. Style the disabled button like the mockup. 

{lang=javascript,line-numbers=off,crop-start-line=99,crop-end-line=108}
<<[Button.ts](./protected/src/Button.style.ts)

When you have finished styling the disabled state, the button should appear as below.

![](./public/assets/disabled.png)

So far, coding the `ButtonComponent` has been an exercise in styling the different states of the button with CSS. Since the button element cannot use Shadow DOM to encapsulate CSS styles, you injected a stylesheet into the `<head>` and selected CSS classnames applied to the instance of `ButtonComponent` to style the variants of the button. It's worth noting the CSS for the `ButtonComponent` could be part of a global stylesheet, but as you'll observe in later chapters, when the `ButtonComponent` is a child of another component's `ShadowRoot`, the shadow boundary will block any global styling. You'll account for this behavior in later chapters. 

Since the `ButtonComponent` extends from the `HTMLButtonElement`, you haven't had to implement behaviors of the button element. If the `ButtonComponent` had extended from the `HTMLElement` you would have had to implement event listeners to provide the same accessibility behaviors of the `HTMLButtonElement`, namely enabling users to activate the button when pressing the Select or Return keys on the keyboard.
