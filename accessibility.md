---
title: Making the form accessible
description: How to make custom elements accessible 
isPublicLesson: true
---

## Making the form accessible

The decisions we've made in developing the login form have so far resulted in half-baked accessibility for users who rely on the keyboard to navigate. The `TextInputComponent` has some accessibility limitations. We've provided a CSS classname for visual feedback, but this poses a problem for the visually impaired, who can't see the feedback when the input becomes invalid. Furthermore, unless we provide some additional context using WAI-ARIA, screen readers can't access validations appended to the Shadow DOM.

While the `ButtonComponent` is useable via keyboard input since it extends from the `HTMLButtonElement`, the addition of an SVG in the icon variant prevents visually impaired users from understanding the context of the button element. Screen readers rely on the content of the button to provide a label, which the primary and secondary button variants already handle. The icon variant, however, lacks a text label that screen readers can analyze and read aloud to the user. 

In this section, you'll use WAI-ARIA attributes to solve two distinct problems with the form:
- screen readers cannot analyze the button label in the icon variant 
- screen readers cannot read the validation messages so that users who rely on screen readers can understand what is needed to validate each form field

First, you'll add the `ButtonComponent` to the `Form` story in TextInput.stories.js so that you can use the `FormTemplate` as your development environment. After a brief introduction on using a screen reader during development, you'll use WAI-ARIA attributes to provide all the necessary context the screen reader needs to navigate the form.

### Button Form

In this chapter, you've developed the `ButtonComponent` in isolation, so let's see what happens when you integrate the component into the existing Form story in TextInput.stories.js.

Import the `ButtonComponent` into TextInput.stories.js.

{lang=javascript,line-numbers=off,crop-start-line=3,crop-end-line=3}
<<[TextInput.stories.js](./protected/src/TextInput.stories.js)

Swap the `input type="submit"` in `FormTemplate` with the `button`. Set the `is` attribute to `in-button` on the button element in the `FormTemplate` so that the browser can interpret the buttons as a customized built-in element. Add the appropriate classnames to the submit button. According to Figma, the submit button is a primary variant of the `ButtonComponent`. 

{lang=javascript,line-numbers=off,crop-start-line=156,crop-end-line=156}
<<[TextInput.stories.js](./protected/src/TextInput.stories.js)

That's it! Since the `ButtonComponent` is a customized built-in element, integration was a snap. The elements in the layout appear very close vertically and don't have the same margin between them as the mockup in Figma. To fix this issue with the layout, add a global style to packages/style/style.css. Select each instance of the `TextInputComponent` with a CSS classname: `.form-control` and set the `margin-bottom`.

```css
.form-control {margin-bottom: var(--margin-md);}  
```

For the remainder of this chapter, use the `Form` and `Icon` stories as a test bed for providing accessibility features to the `ButtonComponent` and the `TextInputComponent`.  

### Using a screen reader
 
 Screen readers are applications that blind, visually impaired, and dyslexic people use to interact with operating systems. Using a synthesized voice and other visual cues, screen readers provide contextual information about what the user currently has focused on the user interface. NVDA is the most popular screen reader application for Windows, while VoiceOver is built into MacOS and other Apple operating systems.

 If you've never used a screen reader application before, follow the steps below to install and open NVDA or VoiceOver for Windows and MacOS, respectively.
 
#### Windows

NVDA, short for NonVisual Desktop Access, is a Windows application available for free from the Microsoft Store. To install NVDA, sign into the Microsoft Store in the Microsoft Store application.

![](./public/assets/nvda-microsoft-store.png)

After you have signed in to the Microsoft Store, search for NVDA, select the application in the search results, and click the **Install** button. Follow the on-screen prompts to install NVDA. 

Once NVDA is installed, access NVDA application preferences in the Windows Taskbar.

![](./public/assets/nvda.png)

NVDA uses a synthesized voice to announce contextual information about the currently focused user interface for visually impaired and blind users in Microsoft Windows.

To stop NVDA at any time, click the NVDA icon in the Windows Taskbar and choose **Exit** from the dialog.

#### MacOS

VoiceOver is a screen reader provided by Apple Inc. in all of its operating systems. In MacOS, access VoiceOver in the System Preferences Accessibility Pane. When you click **Enable VoiceOver**, a contextual overlay appears in the lower left of the screen. VoiceOver announces what is currently in focus through a synthesized voice. 

![](./public/assets/voiceover.png)

During development, you may have to change context rapidly between IDE and browser. If the synthesized voice becomes a burden, you can turn it off by clicking **Open VoiceOver Utility...** in the System Preferences Accessibility Pane. When the VoiceOver Utility Pane appears, select **Speech** in the Sidebar, and click **Mute Speech**. 

![](./public/assets/voiceover-turn-off-voice.png)

Another option you may find useful during development is the ability to hide the VoiceOver cursor. In your IDE, the VoiceOver cursor displays an overlayed cursor for the visually impaired. However, for some users this can interfere with typing.

Once you have a screen reader installed, proceed to the following sections. If you've never used a screen reader before, hopefully you'll find a process for developing with a screen reader that works best for you. 

### Providing context to the icon variant

Earlier in this chapter, you developed the icon variant of the `ButtonComponent`. However, since the label is effectively replaced by an SVG, screen readers can no longer analyze the content of the button element. 

Depending on your operating system, activate NVDA or VoiceOver and leave them running when testing your changes during development for the remainder of this chapter.

If you haven't already done it, activate the screen reader application and navigate to the `Primary` button story in Storybook. Click on the whitespace in the Storybook canvas to focus it, then press the Tab key to focus the primary button. Observe how the screen reader announces the label, followed by the element name.

![](./public/assets/voiceover-submit.png)

Navigate to the `Icon` story and focus the close button. Notice how the screen reader only provides the element because there is no contextual information that labels the button element.

![](./public/assets/voiceover-icon.png)

To provide context for the screen reader, you'll have to use WAI-ARIA attributes, namely `aria-labelledby` and `hidden`. 

Update the `Icon.args` to support the SVG and the label. This change will conflict with other stories in the same file. Make a new template for the icon variant named `IconTemplate` based off the existing `Template` so that you don't interfere with other stories in Button.stories.js. 

{lang=javascript,line-numbers=off,crop-start-line=25,crop-end-line=25}
<<[Button.stories.js](./protected/src/Button-a11y.stories.js)

Don't forget to update the `Icon` story, which should now reference `IconTemplate`.

{lang=javascript,line-numbers=off,crop-start-line=54,crop-end-line=54}
<<[Button.stories.js](./protected/src/Button-a11y.stories.js)

Update `Icon.args` to support both a label and SVG, making sure the destructured properties in `IconTemplate` match the names of each property on `Icon.args`. Set the value of the `label` property to `Close`, and move the SVG to a new property named `svg`.

{lang=javascript,line-numbers=off,crop-start-line=55,crop-end-line=59}
<<[Button.stories.js](./protected/src/Button-a11y.stories.js)

Update `IconTemplate` to passing the `svg` where `label` was once in the template.

{lang=javascript,line-numbers=off,crop-start-line=31,crop-end-line=32}
<<[Button.stories.js](./protected/src/Button-a11y.stories.js)

Next, create a `<span>` tag before the `svg` and set the span element's content with the `label`. To hide the span visually, add the `hidden` attribute, which tells the browser to hide this element from view. Add the `id` attribute to the span element, setting the `id` to `close-button-label`. This `id` is required for the WAI-ARIA attribute `aria-labelledby`, which needs a unique `id`. 

On the button element, set `aria-labelledby` to the `id` of the span, linking the two elements together for screen readers. 

{lang=javascript,line-numbers=off,crop-start-line=25,crop-end-line=32}
<<[Button.stories.js](./protected/src/Button-a11y.stories.js)

Finally, to prevent screen readers from analyzing the SVG, set the `aria-hidden` attribute to `true` on the svg element in Button.stories.js.

{lang=javascript,line-numbers=off,crop-start-line=11,crop-end-line=11}
<<[Button.stories.js](./protected/src/Button-a11y.stories.js)

Navigate to the Icon story in Storybook again and focus the icon button with the screen reader activated. Notice how the screen reader now acts as though the button is labeled directly, as is the case of primary and secondary buttons.

![](./public/assets/voiceover-icon-correct.png)

`aria-labelledby` provides a pointer to an element with an `id` that contains the label for the button element. You hid the span element from sight using `hidden` and used the `aria-hidden` attribute to hide the SVG from screen readers. A combination of all of these WAI-ARIA attributes allows the icon variant of the `ButtonComponent` to work with a screen reader.

### Providing validation messages to screen readers

The validation messages that appear when the `TextInputComponent` is invalid are custom implemented. Browsers don't have a native container for validation messages that screen readers automatically analyze. When the validation messages appear visually, we must provide the same contextual information for the screen reader. We can achieve this through the use of WAI-ARIA attributes `aria-describedby`, `aria-role`, `aria-live`, and `aria-invalid`. 

In the `TextInputComponent` template, set an `id` attribute to `message` on the messages container. Link the input element to the messages container by setting the `aria-describedby` attribute to the same id: `message`. Since the messages container is just a `div`, give the screen reader more information about this element. `aria-role` set to `alert` notifies the screen reader this element contains an alert notification. When the `aria-live` attribute is set to `assertive`, it gives the messages contained in the div element precendence over other announcements made by the screen reader. 

{lang=javascript,line-numbers=off,crop-start-line=70,crop-end-line=77}
<<[TextInput.ts](./protected/src/TextInput.ts)

Navigate to the Form story and test the validations with a screen reader. Notice how the application reads back the messages. This is a huge win for accessibility because each validation message provides context on how the user can correct the form field. 

We can do a little bit more to enhance the functionality for screen readers during validation and improve the user experience at the same time. Right now, when the form field is invalid, the validation messages are visible until the input element is blurred. If the validations were cleared as the user was typing, it would allow the visually impaired to focus on what they are typing rather than listen to the feedback of the validation messages. When the user blurs, validation will kick in and announce validation messages again, but not until then.

There's also one other problem to solve. Currently, the input element receives a classname `error` to show the field is invalid, but there isn't similar feedback for the screen reader. Correct this behavior by finding where the `error` classname is set in validator.ts and use the `aria-invalid` attribute set to `true` to notify screen readers that the input is invalid.

{lang=javascript,line-numbers=off,crop-start-line=22,crop-end-line=29}
<<[validator.ts](./protected/src/validator.ts)

To clear out the validation messages as the user types, add a new `onkeyup` event listener in the `TextInputComponent connectedCallback` lifecycle. In the event listener callback, call a new method on the `TextInputComponent` called `onChange`.

{lang=javascript,line-numbers=off,crop-start-line=145,crop-end-line=151}
<<[TextInput.ts](./protected/src/TextInput.ts)

In the `onChange` method, clear out the content of the messages element, remove the `error` classname and `aria-invalid` attribute from the input, and optionally, update the form with the value of the input. 

{lang=javascript,line-numbers=off,crop-start-line=217,crop-end-line=222}
<<[TextInput.ts](./protected/src/TextInput.ts)

Test the new functionality in the `Form` story. When an instance of `TextInputComponent` is invalid and the user starts typing, the element should appear focused and no longer display validation messages until the user blurs and the form control is invalid. When development is complete you may turn off the screen reader.
