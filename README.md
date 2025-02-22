# tailwindcss-classnames ![NPM](https://img.shields.io/npm/l/tailwindcss-classnames) [![npm bundle size](https://img.shields.io/bundlephobia/minzip/tailwindcss-classnames)](https://bundlephobia.com/result?p=tailwindcss-classnames) [![npm version](https://img.shields.io/npm/v/tailwindcss-classnames.svg)](https://www.npmjs.com/package/tailwindcss-classnames)

Functional typed classnames for TailwindCSS

[TailwindCSS](https://tailwindcss.com/) is a CSS library that has gained a lot of traction. The developer experience is pretty epic and you ensure a low footprint on your css by composing existing classes for most of your css.

## So why mess with it?

TailwindCSS is based on strings and with some nice tooling on top like [TailwindCSS VSCode extension](https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss) you get a pretty descent experience. That said, there are limitations to a purely declarative approach of strings. When using **tailwindcss-classnames** you will get additional power in the form of:

- **Validation of classnames**: You can not write the wrong classname, cause the API only allows you to insert valid classnames
- **Functional approach**: Since we are working in Typescript we get more freedom in using functional powers like composition and dynamic composition
- **Defining by variables**: Even though it is nice to write TailwindCSS inline with your elements, it does not scale. You want to move definitions outside of the component for reusability and composition

You can not get this experience using pure TailwindCSS and the VSCode extension, but you do get it with **tailwindcss-classnames**.

[![Edit tailwindcss-classnames](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/elegant-lederberg-sih5r?fontsize=14&hidenavigation=1&theme=dark)

## Install

**⚠️ NOTE:** This package releases versions match with TailwindCSS versions except for [semver _patch_](https://semver.org/) releases

Please [follow the guide](https://tailwindcss.com/docs/installation/) to set up **TailwindCSS**. Now do:

```
npm install tailwindcss-classnames
```

The project is literally the [classnames](https://www.npmjs.com/package/classnames) project with custom typing and functions for applying pseudo elements. That means it arrives at your browser at approximately **484b** minified and gzipped ([bundlephobia](https://bundlephobia.com/result?p=tailwindcss-classnames)).

## Create classes

```js
import {classnames} from 'tailwindcss-classnames';

classnames('border-none', 'rounded-sm');
```

The arguments passed to **classnames** is typed, which means you get discoverability. You can even search for the supported classes:

![DISCOVER](/discover.png)

## Dynamic classes

Since we are using **classnames** you can also add your classes dynamically:

```js
import {classnames} from 'tailwindcss-classnames';

classnames('border-none', 'rounded-sm', {
  ['bg-gray-200']: true,
});
```

## Composing classes

Even though **classnames** just returns a string, it is a special typed string that you can compose into other definitions.

```js
import {classnames} from 'tailwindcss-classnames';

export const button = classnames('border-none', 'rounded-sm');

export const redButton = classnames(button, 'bg-red-100');
```

## Using with React

Since React has excellent typing support I want to give an example of how you could use it.

```tsx
// styles.ts
import {classnames} from 'tailwindcss-classnames';

export const form = classnames('container', 'w-full');

export const button = classnames('border-none', 'rounded-sm');

export const alertButton = classnames(button, 'bg-red-100');

export const disabled = classnames('opacity-25', 'bg-gray-100');

export const submitButton = (disabled: boolean) =>
  classnames(styles.button, {
    [styles.disabled]: disabled,
  });

// App.tsx
import * as React from 'react';
import * as styles from './styles';

export const App: React.FC<{disabled}> = ({disabled}) => {
  return (
    <form className={styles.form}>
      <button type="submit" className={styles.submitButton(disabled)}>
        Submit
      </button>
      <button className={styles.alertButton}>Cancel</button>
    </form>
  );
};
```

## Using the CLI to generate custom types

The types included in this package are the default tailwindcss classes, but if you modified your tailwind config file and/or want to add external custom classes, you can use the CLI tool to do this.

Simply run `npx tailwindcss-classnames`.

## CLI arguments:

```
  -i, --input <input>    Name or relative path of the TailwindCSS config file
  -o, --output <output>  Name or relative path of the generated types file **(optional, default: "tailwindcss-classnames.ts")**
  -x, --extra <extra>    Name or relative path of the file with the custom extra types **(optional)**
  -h, --help             display help for command
```

## Example of CLI usage:

If you want to add types from external file named `my-custom-classes.ts`
containing the following code:

```ts
type TCustomClasses =
  | "red-button"
  | "app-sidebar"
  | "app-navbar"
  | ...

// Note that you must provide a default export of the type
export default TCustomClasses;
```

You will execute the CLI with the following arguments:

```bash
npx tailwindcss-classnames -i path/to/tailwind.config.js -o path/to/output-file.ts -x my-custom-classes
```

Also, you can add it to your `package.json` scripts for convenience:

```json
"scripts": {
  "generate-css-types": "tailwindcss-classnames -i relative/path/to/tailwind.config.js"
}
```
