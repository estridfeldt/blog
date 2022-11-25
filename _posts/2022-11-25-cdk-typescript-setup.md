---
layout: post
---

- Install cdk
- Bootstrap
- Add linter
- Add prettier
- Config EsLint and Prettier
- Fix jest config

## Install cdk

`npm i -g aws-cdk`

## Bootstrap cdk typescript project

```
mkdir sample-project
cd sample-project
cdk init --language=typescript
git branch -m main
```

## Add linter

`npm i -D eslint @typescript-eslint/eslint-plugin @typescript-eslint/parser`

## Add prettier

`npm i -D prettier eslint-config-prettier eslint-plugin-prettier`

## Config EsLint and Prettier

In package.json, add the following content at the end of the file

```
...,
"eslintConfig": {
    "root": true,
    "parser": "@typescript-eslint/parser",
    "plugins": [
      "@typescript-eslint",
      "prettier"
    ],
    "extends": [
      "eslint:recommended",
      "plugin:@typescript-eslint/eslint-recommended",
      "plugin:@typescript-eslint/recommended",
      "plugin:prettier/recommended"
    ]
  },
  "eslintIgnore": [
    "node_modules",
    "build"
  ],
  "prettier": {
    "semi": false,
    "trailingComma": "all",
    "singleQuote": true,
    "printWidth": 80
  },
...
```

## Script section

Replace scripts section in package.json with this content:

```
  "scripts": {
    "lint": "eslint . --ext .ts",
    "build": "tsc",
    "watch": "tsc -w",
    "test": "jest",
    "cdk": "cdk",
    "prettier-format": "prettier 'src/**/*.ts' --write"
  },
```

## Format code

`npm run lint`

Should show a lot of errors

`npm run lint -- --fix`

## Setup jest config

Remove the file jest.config.js

Add the following to package.json

```
  "jest": {
    "preset": "ts-jest",
    "testEnvironment": "node",
    "roots": [
      "<rootDir>/test/"
    ],
    "testMatch": [
      "**/*.test.ts"
    ]
  }
```

Run the tests

`npm run test`

## Create a snapshot test

Create a file in the test dir called stack-test.test.ts with the following content

```javascript
import * as cdk from "aws-cdk-lib";
import { Template } from "aws-cdk-lib/assertions";
import * as SampleProject from "../lib/sample-project-stack";

describe("Stack test", () => {
  it("should verify stack", () => {
    const app = new cdk.App();

    const stack = new SampleProject.SampleProjectStack(app, "MyTestStack");

    const template = Template.fromStack(stack);
    expect(template.toJSON).toMatchSnapshot();
  });
});
```

Run the tests

`npm run test`
A snapshot directory is now created
