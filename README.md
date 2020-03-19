# Using semantic-release with GitHub Actions

> This is a demo of how to automatically publish npm packages to the npm registry using GitHub Actions and `semantic-release`.

## What is semantic-release?

> semantic-release automates the whole package release workflow including: determining the next version number, generating the release notes and publishing the package. This removes the immediate connection between human emotions and version numbers, strictly following the Semantic Versioning specification.

See https://github.com/semantic-release/semantic-release#how-does-it-work

## What is GitHub Actions?

GitHub Actions is a code execution platform for doing CI/CD. It's similar to services like Travis CI or Circle CI, but it's built right into GitHub. 

Read more at https://github.com/features/actions or what a short demo at https://www.youtube.com/watch?v=TwkAD_yljVw

## Steps to set up Semantic Release on your GitHub repo

- [x] Only allow squash merging of pull requests
- [x] Install https://github.com/apps/semantic-pull-requests
- [x] Create npm token using `npm token create` or https://www.npmjs.com/settings
- [x] Add token to repo secrets as `NPM_TOKEN`
- [x] Add release workflow file to `.github/workflows/release.yml`
- [x] Set `version` to `0.0.0-development` in package.json
- [x] `npm i -D semantic-release`
- [x] Use semantic commit messages
- [x] Create a pull request

## Example Workflow

```yml
name: Release npm package

on:
  push:
    branches:
      - master

jobs:
  release:
    name: Release
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@master
      - uses: actions/setup-node@v1
        with:
          node-version: "12.x"
      - run: npm ci
      - run: npm run build --if-present
      - run: npm test
      - run: npx semantic-release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
```