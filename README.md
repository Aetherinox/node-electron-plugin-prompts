<div align="center">
<h6>Create customized dialog prompts using templates.</h6>
<h1>🔅 Electron Prompts & Dialogs 🔅</h1>
<br />

<p>

**Electron Dialogs** is a forked version of [custom-electron-prompt](https://github.com/Aetherinox/node-electron-plugin-prompts), which has been modified to add new functionality, fix bugs, and maintain dependency updates.

<br />

This node package was originally developed for [Ntfy Desktop](https://github.com/Aetherinox/ntfy-desktop)

</p>

<br />

<div align="center">

<!-- prettier-ignore-start -->
[![Version][github-version-img]][github-version-uri]
[![Version][npm-version-img]][npm-version-uri]
[![Build Status][github-tests-img]][github-tests-uri]
[![Downloads][github-downloads-img]][github-downloads-uri]
[![Size][github-size-img]][github-size-img]
[![Last Commit][github-commit-img]][github-commit-img]
[![Contributors][contribs-all-img]](#contributors-)
<!-- prettier-ignore-end -->

</div>

<br />

<div align="center">

<img src="https://raw.githubusercontent.com/Aetherinox/node-electron-plugin-prompts/main/screenshots/General/1.png" width="630">

<br />

<img src="https://raw.githubusercontent.com/Aetherinox/node-electron-plugin-prompts/main/screenshots/General/2.png" width="630">

</div>

</div>

<br />

---

<br />

- [About](#about)
- [Example of a Simple Prompt from Input Type](#example-of-a-simple-prompt-from-input-type)
- [Usage](#usage)
  - [Simple Input Example](#simple-input-example)
- [Special Prompt Types](#special-prompt-types)
  - [keybind](#keybind)
  - [counter](#counter)
  - [select](#select)
  - [multiInput](#multiinput)
- [Options object (optional)](#options-object-optional)
  - [⚠️ New options :](#️-new-options-)
  - [Original options:](#original-options)
  - [parentBrowserWindow (optional)](#parentbrowserwindow-optional)
  - [customScript (optional)](#customscript-optional)
  - [Custom/Extra Button (optional)](#customextra-button-optional)
- [Contributors ✨](#contributors-)


<br />

---

<br />

## About
The `Electron Dialogs` node package allows you to generate dialog boxes / prompts for your electron powered projects.

There are currently **5 types** available:

- Input
- Keybind
- Counter
- Select
- MultiInput

There is also an option for a button with user-defined `onclick` function.

<br />

> [!NOTE]
> Disclaimer:
> This node package has had two previous owners:
> - [custom-electron-prompt](https://github.com/Aetherinox/node-electron-plugin-prompts)
> - [electron-prompt](https://github.com/p-sam/electron-prompt)

<br />

---

<br />

## Example of a Simple Prompt from Input Type

![](https://github.com/Aetherinox/node-electron-plugin-prompts/blob/main/screenshots/Input/Input.png)
![](https://github.com/Aetherinox/node-electron-plugin-prompts/blob/main/screenshots/Input/InputDark.png)

<br />

---

<br />

## Usage

* 1: Install the npm package to your project directory with
  ```bash
  npm install electron-plugin-prompts
  ```
  or
  ```bash
   yarn add electron-plugin-prompts
   ```
  or
  ```bash
   bun add electron-plugin-prompts
   ```

<br />

* 2: Import prompt

  ```javascript
  const prompt = require('electron-plugin-prompts')
  ```

* 3: Create a prompt

  ```javascript
  prompt([options, parentBrowserWindow])
  ```

   calling the prompt function returns a Promise

   Promise resolve returns the input or returns null if prompt was canceled

       On error, Prompise reject returns custom error message

<br />

### Simple Input Example

```javascript
const prompt = require('./prompt');

prompt({
    title: 'Prompt example',
    label: 'URL:',
    value: 'http://example.org',
    inputAttrs: {
        type: 'url'
    },
    type: 'input'
})
.then((r) => {
    if(r === null) {
        console.log('user cancelled');
    } else {
        console.log('result', r);
    }
})
.catch(console.error);
```

<br />

---

<br />

## Special Prompt Types

<br />

### keybind

Create a prompt with possibly multiple keybind selects

Must specify `keybindOptions` with valid entries in format:

```javascript
keybindOptions: [
    { value: "copyAccelerator", label: "Copy", default: "Ctrl+C" }
    { value: "pasteAccelerator", label: "Paste", default: "Ctrl+V" }
]
```

<br />

Return an array made of objects in format

`{value: "copyAccelerator", accelerator: "Ctrl+Shift+Insert"}`

where `accelerator` is the input for the `value` you registered

 <details>
  <summary> Code Example </summary>

 ```javascript
const kb = ($value, $label, $default) => { return { value: $value, label: $label, default: $default } };
prompt({
	title: "Keybinds",
	label: "Select keybind for each method",
	type: "keybind",
	value: "2", // Doesn't do anything here
	keybindOptions: [
		{ value: "volumeUp", label: "Increase Volume", default: "Shift+PageUp" },
		kb("volumeDown", "Decrease Volume", "Shift+PageDown"),
		kb("playPause", "Play / Pause") // (null || empty string || undefined) == no default
	],
	resizable: true,
	customStylesheet: "dark",
}, win).then(input => {
	if (input)
		input.forEach(obj => console.log(obj))
	else
		console.log("Pressed Cancel");
})
	.catch(console.error)
 ```
 </details>

 <details>
  <summary> Screenshots </summary>

![](https://github.com/Aetherinox/node-electron-plugin-prompts/blob/main/screenshots/Keybind/Keybind3.png)
![](https://github.com/Aetherinox/node-electron-plugin-prompts/blob/main/screenshots/Keybind/Keybind.png)

![](https://github.com/Aetherinox/node-electron-plugin-prompts/blob/main/screenshots/Keybind/KeybindDark3.png)
![](https://github.com/Aetherinox/node-electron-plugin-prompts/blob/main/screenshots/Keybind/KeybindDark.png)
</details>

<br />

### counter

Create a prompt for selecting numeric values, with integrated `+` and `-` buttons

You **can** specify `counterOptions` with valid entries in format:

```javascript
counterOptions: {
    minimum: 0, //defaults to null
    maximum: 250, //defaults to null
    multiFire: true //default to false
}
```

 minimum and maximum of numeric counter, and multifire indicate if continuous input is enabled.

 <details>
  <summary> Code Example </summary>

 ```javascript
prompt({
	title: "Counter",
	label: "Choose a number:",
	value: "59",
	type: "counter",
	counterOptions: { minimum: -69, maximum: null, multiFire: true },
	resizable: true,
	height: 150,
	width: 300,
	customStylesheet: "dark",
}, win).then(input => console.log(`input == ${input}`)).catch(console.error)
 ```
 </details>

 <details>
  <summary> Screenshots </summary>

![](https://github.com/Aetherinox/node-electron-plugin-prompts/blob/main/screenshots/Counter/Counter.png)
![](https://github.com/Aetherinox/node-electron-plugin-prompts/blob/main/screenshots/Counter/CounterDark.png)
</details>

<br />

### select

Create a prompt with a dropdown select menu.

Must specify selectOptions with valid entries in **one** of the following format:

```javascript
 selectOptions: ["thisReturn0", "thisReturn1", "thisReturn2"]
 selectOptions: {
    0: "thisReturn0",
    1: "thisReturn1",
    2: "imSelected",
    potato: "thisReturnPotato"
 }
```

<details>
  <summary> Code Example </summary>

 ```javascript
prompt({
	title: "Select",
	label: "Choose an option:",
	type: "select",
	value: "2",
	selectOptions: ["thisReturn0", "thisReturn1", "imSelected", "thisReturn3"],
	// 	selectOptions: {0: "thisReturn0", 1: "thisReturn1", 2: "imSelected" , potato: "thisReturnPotato"},
	resizable: true,
	height: 150,
	width: 300,
	customStylesheet: "dark",
}, win).then(input => console.log(`input == ${input}`)).catch(console.error)
 ```
 </details>

 <details>
  <summary> Screenshots </summary>

![](https://github.com/Aetherinox/node-electron-plugin-prompts/blob/main/screenshots/Select/SelectClosed.png)
![](https://github.com/Aetherinox/node-electron-plugin-prompts/blob/main/screenshots/Select/SelectOpen.png)

![](https://github.com/Aetherinox/node-electron-plugin-prompts/blob/main/screenshots/Select/SelectDarkClosed.png)
![](https://github.com/Aetherinox/node-electron-plugin-prompts/blob/main/screenshots/Select/SelectDarkOpen.png)
</details>

<br />

### multiInput

Create a prompt with multiple inputs. Select inputs can also be used.

Returns an array with with input in same order that was given to the options, for example:
multiInputOptions: [{usernameOptions}, {passwordOptions}] could return ["Jack", "61523"]

Must specify multiInputOptions with valid entries in the following format:

```javascript
 multiInputOptions: [{myinputoptions1}, {myinputoptions2}]
```

<details>
  <summary> Code Example </summary>

 ```javascript
prompt({
    title: "credentials",
    label: "Login Info:",
    type: "multiInput",
    multiInputOptions:
        [
            {
                inputAttrs:
                {
                    type: "email",
                    required: true,
                    placeholder: "email"
                }
            },
            {
                inputAttrs:
                {
                    type: "password",
                    placeholder: "password"
                }
            },
            {
                selectOptions: { na: "North America", eu: "Europe", other: "Other" },
                value: "2"
            }
        ],
    resizable: true,
    width: 300,
    height: 225,
}, win).then(input => console.log(`input == ${input}`)).catch(console.error)
 ```
 </details>

 <details>
  <summary> Screenshots </summary>

With `selectOptions`:

![](https://github.com/Aetherinox/node-electron-plugin-prompts/blob/main/screenshots/multiInput/multiInputSelect.png)

Without `selectOptions`:
*This screenshot also contains a custom button.*

![](https://github.com/Aetherinox/node-electron-plugin-prompts/blob/main/screenshots/multiInput/button.png)

</details>

<br />

---

<br />

## Options object (optional)

### ⚠️ New options :

| Key                | Explanation                                                                                                                                                                                                                                                    |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| frame              | (optional, boolean) Wether to create prompt with frame. Defaults to true.                                                                                                                                                                                      |
| customScript       | (optional, string) The local path of a JS file to run on preload. Defaults to null.                                                                                                                                                                            |
| enableRemoteModule | (optional, boolean) Wether the prompt window have remote modules activated, Defaults to false.                                                                                                                                                                 |
| customStylesheet   | (optional, string) The local path of a CSS file to customize the style of the prompt window, **you can use just "dark" to use the premade dark skin**. Defaults to null.                                                                                       |
| type               | (optional, string) The type of input field, either 'input' for a standard text input field or 'select' for a dropdown type input or `counter` for a number counter with buttons. or `keybind` for an electron accelerator grabber. or **`multiInput` to use more than 1 input in a prompt** Defaults to 'input'.  |
| counterOptions     | (optional, object) minimum and maximum of counter, and if continuous input is enabled. format: `{minimum: %int%, maximum: %int%, multiFire: %boolean%`. min+max values defaults to null and multiFire defaults to false.                                       |
| keybindOptions     | (optional, object)  Required if type=keybind. represent an array of objects in format: `{type: %string%, value: %string%, default: %string%}`. `default` has to be a valid accelerator to work                                                                 |
| multiInputOptions     | (optional, object) an Array of objects having options for every input, format: `[{inputAttrs:{type:'email'}},{inputAttrs:{type:'password'}}]`, `[object, object]` to use it without passing any options simply `[{},{},{}]`, just create x amount of empty objects to add x inputs.                                       |
| button       | (optional, object) adds a button after the success(OK) with a custom label, onClick and attributes. Object format: `{label: 'myLabel', click: () => alert("click"), attrs: {style: 'background: black'}}`, `{label: %string%, click: %function%, attrs: %object%}`|

<br />

### Original options:

| Key            | Explanation                                                                                                                                                                                                                    |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| title          | (optional, string) The title of the prompt window. Defaults to 'Prompt'.                                                                                                                                                       |
| label          | (optional, string) The label which appears on the prompt for the input field. Defaults to 'Please input a value:'.                                                                                                             |
| buttonLabels   | (optional, object) The text for the OK/cancel buttons. Properties are 'ok' and 'cancel'. Defaults to null.                                                                                                                     |
| value          | (optional, string) The default value for the input field. Defaults to null.                                                                                                                                                    |
| type           | (optional, string) The type of input field, either 'input' for a standard text input field or 'select' for a dropdown type input or 'counter' for a number counter with buttons. Defaults to 'input'.                          |
| inputAttrs     | (optional, object) The attributes of the input field, analagous to the HTML attributes: `{type: 'text', required: true}` -> `<input type="text" required>`. Used if the type is 'input'.                                       |
| selectOptions  | (optional, object) The items for the select dropdown if using the 'select' type in the format 'value': 'display text', where the value is what will be given to the then block and the display text is what the user will see. |
| useHtmlLabel   | (optional, boolean) Whether the label should be interpreted as HTML or not. Defaults to false.                                                                                                                                 |
| width          | (optional, integer) The width of the prompt window. Defaults to 370.                                                                                                                                                           |
| minWidth       | (optional, integer) The minimum allowed width for the prompt window. Default to width if specified or default_width(370).                                                                                                      |
| height         | (optional, integer) The height of the prompt window. Defaults to 130.                                                                                                                                                          |
| minHeight      | (optional, integer) The minimum allowed height for the prompt window. Default to height if specified or default_height(160).                                                                                                   |
| resizable      | (optional, boolean) Whether the prompt window can be resized or not (also sets useContentSize). Defaults to false.                                                                                                             |
| alwaysOnTop    | (optional, boolean) Whether the window should always stay on top of other windows. Defaults to false                                                                                                                           |
| icon           | (optional, string) The path to an icon image to use in the title bar. Defaults to null and uses electron's icon.                                                                                                               |
| menuBarVisible | (optional, boolean) Whether to show the menubar or not. Defaults to false.                                                                                                                                                     |
| skipTaskbar    | (optional, boolean) Whether to show the prompt window icon in taskbar. Defaults to true.                                                                                                                                       |

If not supplied, it uses the defaults listed in the table above.

<br />

### parentBrowserWindow (optional)

The window in which to display the prompt on. If not supplied, the parent window of the prompt will be null.

<br />

### customScript (optional)

Create the script with the following template:

```javascript
module.exports = () => {
    // This function will be called as a preload script
    // So you can use front features like `document.querySelector`
};
```

<br />

### Custom/Extra Button (optional)

Adds an extra/custom button with special functionalities other than success or error. Passing a `label` will update the button's innerHTML, `click` should be a funtion which will execute **onclick**, lastly `attrs` should contain all the attributes that should be added to the button such as custom styles.

 <details>
  <summary> Code Example </summary>


```javascript
await prompt({
            title: 'Login credentials',
            label: 'Credentials',
            value: 'http://example.org',
            inputAttrs: {
                type: 'url'
            },
            type: 'multiInput',
            multiInputOptions:
                [{
                    label: "username",
                    inputAttrs:
                    {
                        type: "email",
                        required: true,
                        placeholder: "email"
                    }
                },
                {
                    label: "password",
                    inputAttrs:
                    {
                        type: "password",
                        placeholder: "password"
                    }
                }],
            // customStylesheet: "dark",
            button:
            {
                label: "Autofill",
                click: () =>
                {
                    document.querySelectorAll("#data")[0].value = "mama@young.com";
                    document.querySelectorAll("#data")[1].value = "mysecretrecipe";
                },
                attrs:
                {
                    abc: 'xyz'
                }
            }
        }));
```

</details>

 <details>
  <summary> Screenshots </summary>


![](https://github.com/amunim/custom-electron-prompt/blob/main/screenshots/multiInput/button.PNG)
</details>

<br />

---

<br />

## Contributors ✨

We are always looking for contributors. If you feel that you can provide something useful to this project, then we'd love to review your suggestion. Before submitting your contribution, please review the following resources:

- [Pull Request Procedure](.github/PULL_REQUEST_TEMPLATE.md)
- [Contributor Policy](CONTRIBUTING.md)

<br />

Want to help but can't write code?

- Review [active questions by our community](https://github.com/Aetherinox/node-electron-dialogs/labels/❔%20Question) and answer the ones you know.

<br />

The following people have helped keep this project going:

<br />

<div align="center">

<!-- ALL-CONTRIBUTORS-BADGE:START - Do not remove or modify this section -->
[![Contributors][contribs-all-img]](#contributors-)
<!-- ALL-CONTRIBUTORS-BADGE:END -->

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tbody>
    <tr>
      <td align="center" valign="top"><a href="https://github.com/Aetherinox"><img src="https://avatars.githubusercontent.com/u/118329232?v=4&s=40" width="80px;" alt="Aetherinox"/><br /><sub><b>Aetherinox</b></sub></a><br /><a href="https://github.com/Aetherinox/electron-dialogs/commits?author=Aetherinox" title="Code">💻</a> <a href="#projectManagement-Aetherinox" title="Project Management">📆</a></td>
    </tr>
  </tbody>
</table>
</div>
<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->
<!-- ALL-CONTRIBUTORS-LIST:END -->

<br />
<br />

<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->

<br />

---

<br />

<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->

<!-- BADGE > GENERAL -->
  [general-npmjs-uri]: https://npmjs.com
  [general-nodejs-uri]: https://nodejs.org
  [general-npmtrends-uri]: https://npmtrends.com/@aetherinox/electron-dialogs

<!-- BADGE > VERSION > GITHUB -->
  [github-version-img]: https://img.shields.io/github/v/tag/aetherinox/node-electron-dialogs?logo=GitHub&label=Version&color=ba5225
  [github-version-uri]: https://github.com/Aetherinox/node-electron-dialogs/releases

<!-- BADGE > VERSION > NPMJS -->
  [npm-version-img]: https://img.shields.io/npm/v/@aetherinox/electron-dialogs?logo=npm&label=Version&color=ba5225
  [npm-version-uri]: https://npmjs.com/package/@aetherinox/electron-dialogs

<!-- BADGE > VERSION > PYPI -->
  [pypi-version-img]: https://img.shields.io/pypi/v/electron-dialogs
  [pypi-version-uri]: https://pypi.org/project/electron-dialogs/

<!-- BADGE > LICENSE > MIT -->
  [license-mit-img]: https://img.shields.io/badge/MIT-FFF?logo=creativecommons&logoColor=FFFFFF&label=License&color=9d29a0
  [license-mit-uri]: https://github.com/Aetherinox/node-electron-dialogs/blob/main/LICENSE

<!-- BADGE > GITHUB > DOWNLOAD COUNT -->
  [github-downloads-img]: https://img.shields.io/github/downloads/Aetherinox/node-electron-dialogs/total?logo=github&logoColor=FFFFFF&label=Downloads&color=376892
  [github-downloads-uri]: https://github.com/Aetherinox/node-electron-dialogs/releases

<!-- BADGE > NPMJS > DOWNLOAD COUNT -->
  [npmjs-downloads-img]: https://img.shields.io/npm/dw/%40aetherinox%2Felectron-dialogs?logo=npm&&label=Downloads&color=376892
  [npmjs-downloads-uri]: https://npmjs.com/package/@aetherinox/electron-dialogs

<!-- BADGE > GITHUB > DOWNLOAD SIZE -->
  [github-size-img]: https://img.shields.io/github/repo-size/Aetherinox/node-electron-dialogs?logo=github&label=Size&color=59702a
  [github-size-uri]: https://github.com/Aetherinox/node-electron-dialogs/releases

<!-- BADGE > NPMJS > DOWNLOAD SIZE -->
  [npmjs-size-img]: https://img.shields.io/npm/unpacked-size/@aetherinox/electron-dialogs/latest?logo=npm&label=Size&color=59702a
  [npmjs-size-uri]: https://npmjs.com/package/@aetherinox/electron-dialogs

<!-- BADGE > CODECOV > COVERAGE -->
  [codecov-coverage-img]: https://img.shields.io/codecov/c/github/Aetherinox/node-electron-dialogs?token=MPAVASGIOG&logo=codecov&logoColor=FFFFFF&label=Coverage&color=354b9e
  [codecov-coverage-uri]: https://codecov.io/github/Aetherinox/node-electron-dialogs

<!-- BADGE > ALL CONTRIBUTORS -->
  [contribs-all-img]: https://img.shields.io/github/all-contributors/Aetherinox/node-electron-dialogs?logo=contributorcovenant&color=de1f6f&label=contributors
  [contribs-all-uri]: https://github.com/all-contributors/all-contributors

<!-- BADGE > GITHUB > BUILD > NPM -->
  [github-build-img]: https://img.shields.io/github/actions/workflow/status/Aetherinox/node-electron-dialogs/package-release.yml?logo=github&logoColor=FFFFFF&label=Build&color=%23278b30
  [github-build-uri]: https://github.com/Aetherinox/node-electron-dialogs/actions/workflows/package-release.yml

<!-- BADGE > GITHUB > BUILD > Pypi -->
  [github-build-pypi-img]: https://img.shields.io/github/actions/workflow/status/Aetherinox/node-electron-dialogs/release-pypi.yml?logo=github&logoColor=FFFFFF&label=Build&color=%23278b30
  [github-build-pypi-uri]: https://github.com/Aetherinox/node-electron-dialogs/actions/workflows/release-pypi.yml

<!-- BADGE > GITHUB > TESTS -->
  [github-tests-img]: https://img.shields.io/github/actions/workflow/status/Aetherinox/node-electron-dialogs/package-tests.yml?logo=github&label=Tests&color=2c6488
  [github-tests-uri]: https://github.com/Aetherinox/node-electron-dialogs/actions/workflows/package-tests.yml

<!-- BADGE > GITHUB > COMMIT -->
  [github-commit-img]: https://img.shields.io/github/last-commit/Aetherinox/node-electron-dialogs?logo=conventionalcommits&logoColor=FFFFFF&label=Last%20Commit&color=313131
  [github-commit-uri]: https://github.com/Aetherinox/node-electron-dialogs/commits/main/

<!-- prettier-ignore-end -->
<!-- markdownlint-restore -->

