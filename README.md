# Codr CI/CD

This repository contains reusable CI/CD workflows to be reused throughout the rest of Codr's code respositories.

Please refer to the `.github/workflow-templates/` directory for examples of how the pre-built workflow files. These workflows can be called by any Codr repository and should be used to reduce code duplication and excessive maintenance keeping all workflows the same across dozens of repos.

## Workflows

`node-test.yml` - This workflow builds and tests the nodejs code without an artifact. It's designed for testing the microservice code and requires some secrets and environment variables in order to work.

`node-build-artifact` - Builds the code and uploads the artifact to be reused in future steps.

`node-test-artifact` - Pulls the built code artifact and runs unit tests against it.

`node-publish-npm` - Pulls the built code artifact and publishes it to the npm registry.

`docker-build-and-publish` - Builds and publishes a microservice docker image to the `ghcr.io` registry.

## Example Usage

```yaml
name: Build and Publish NPM Module

on:
  workflow_dispatch:
  push:
    branches:
      - "**"
  pull_request:
    branches:
      - $default-branch
  release:
    types:
      - created

jobs:
  build:
    uses: CodrJS/cicd/.github/workflows/node-build-artifact.yml@main
  test:
    uses: CodrJS/cicd/.github/workflows/node-test-artifact.yml@main
    needs: build
  publish:
    uses: CodrJS/cicd/.github/workflows/node-publish-npm.yml@main
    needs: test
    secrets: inherit

```