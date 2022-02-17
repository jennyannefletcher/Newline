---
title: Testing ButtonComponent
description: Writing integration and snapshot tests for customized built-in elements
isPublicLesson: true
---

# Testing ButtonComponent

We could setup an integration test for `ButtonComponent`, but that kind of end to end testing is reserved for part three of this book when you develop a login form in the context of an application. Snapshot tests provide the most value at this point in time. After developing several interaction states for the `ButtonComponent`, you can validate the styling of the button most effectively with snapshots.


Ensure Storybook is running in a Terminal window.


```bash
yarn storybook
```

With Storybook running generate snapshots for the component in another terminal window.

```bash
yarn test:snapshot:ci
```

When running the snapshots you'll notice a failure in the Form snapshot. Open the image file that displays the differences between the previous snapshot and current one in the directory packages/component/src/input/__snapshots__/__diff_output__. Upon inspection, you'll realize the diff is because you added a bottom margin with CSS to the form controls in the layout. No harm, no foul. This change was intentional. To fix the snapshots, delete the __diff_output__ directory and delete the existing snapshot.

```bash
rm -rf packages/component/src/input/__snapshots__/__diff_output__
rm packages/component/src/input/__snapshots__/web-components-storyshots-test-js-storyshots-components-inputs-text-input-form-1-snap.png
```

Run the snapshot tests again. Since you deleted the offending snapshot, a new file will be generated in its place.

```bash
yarn test:snapshot:ci
```


After snapshots have been generated for each story, add them to your commit.


Before concluding this chapter, check if any tests have been broken due to the most recent changes. You did refactor the code in packages/component/src/input/TextInput.stories.js quite a bit to include the `HTMLButtonElement` element in the template.

```bash
yarn test:e2e
```

A test failure happens in the `TextInputComponent` test suite due to a timeout. The test cannot find an element with the class name `.submit`. That happened because we failed to give the `button` element this class name in packages/component/src/input/TextInput.stories.js.

```bash
  1) TextInputComponent
       should display error message on blur:
     AssertionError: Timed out retrying after 30000ms: Expected to find element: `.submit`, but never found it.
      at Context.eval (http://localhost:6006/__cypress/tests?p=cypress/integration/TextInput.spec.js:104:36)
```

Update `FormTemplate` in packages/component/src/input/TextInput.stories.js with the class name the test expects.

```html
<button class="primary submit" is="in-button" type="submit">Submit</button>
```

Run the tests again to observe `All specs passed!`.

```bash
yarn test:e2e
```

Commit your changes to complete this section. Now you've created snapshots for `ButtonComponent` let's review the content of Chapter Three.
