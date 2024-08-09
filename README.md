# Setup JS project using Yarn

This GitHub Action makes it easy to initialize a JavaScript project by setting up Node.js and installing
dependencies through Yarn. It also offers an optional caching feature to speed up the build process.

## Inputs

- `cache`: Enable or disable the caching of `node_modules/*` and `.yarn/cache/*`. By default, caching is
  enabled (`true`).

## Usage

Include this action in the GitHub Actions workflow to install and manage dependencies for a JavaScript project with
Yarn. Below is an example workflow demonstrating the use of this action in conjunction with `build` and `lint`
steps:

```yaml
name: Main Suite

on:
  pull_request:
    branches:
      - "**"
  push:
    branches:
      - main

concurrency:
  group: "${{ github.workflow }}-${{ github.event.pull_request.number || github.ref }}"
  cancel-in-progress: true  # Cancel in-progress runs when a new workflow with the same group name is triggered

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Yarn Project
        uses: team-monite/setup-yarn-project-action@v1
        with:
          cache: 'true'
      - name: Build
        run: yarn build

  lint:
    needs: build
    name: Linting
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Yarn Project
        uses: team-monite/setup-yarn-project-action@v1
      - name: Lint
        run: yarn lint
```

## Features

- Sets up Node.js 20.x.
- Installs dependencies using Yarn.
- Caches `node_modules` and `.yarn/cache` to reduce build times.
- Enables Turbo Repo Remote Cache for faster dependency builds.

## Requirements

- GitHub Actions enabled in your repository.
- A Yarn-based JavaScript project.
