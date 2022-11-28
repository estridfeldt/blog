---
layout: post
---

```bash
#! /bin/bash

cdk init --language=typescript
git branch -m main
npm i -D eslint @typescript-eslint/eslint-plugin @typescript-eslint/parser
npm i -D prettier eslint-config-prettier eslint-plugin-prettier
npm pkg set eslintConfig.root=true --json
npm pkg set eslintConfig.plugins[]=@typescript-eslint
npm pkg set eslintConfig.plugins[]="prettier"
npm pkg set eslintConfig.extends[]=eslint:recommended
npm pkg set eslintConfig.extends[]=plugin:@typescript-eslint/eslint-recommended
npm pkg set eslintConfig.extends[]=plugin:@typescript-eslint/recommended
npm pkg set eslintConfig.extends[]=plugin:prettier/recommended
npm pkg set eslintIgnore[]=node_modules
npm pkg set eslintIgnore[]=build
npm pkg set prettier.semi=false --json
npm pkg set prettier.trailingComma=all
npm pkg set prettier.singleQuote=true --json
npm pkg set prettier.printWidth=80 --json
npm pkg set scripts.lint="eslint . --ext .ts"
npm pkg set scripts.prettier-format="prettier 'src/**/*.ts' --write"
npm run lint -- --fix
npm pkg set jest.preset=ts-jest
npm pkg set jest.testEnvironment=node
npm pkg set jest.roots[]="<rootDir>/test/"
npm pkg set jest.testMatch[]="**/*.test.ts"
rm -rf jest.config.js
npm run test

cat <<- EOF > test/stack-test.test.ts
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
EOF

```
