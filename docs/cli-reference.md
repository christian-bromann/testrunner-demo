---
id: cli-reference
title: Testrunner Toolkit CLI Reference
sidebar_label: CLI Reference
---

This page provides information for `saucectl`, the command line interface of the Sauce Labs Testrunner Toolkit.

## `saucectl`

[This repository](https://github.com/saucelabs/saucectl) contains the Go binary that you use to kick off tests.

## Commands

### `new`

```bash
saucectl new
```

This command will ask you to choose one of the frameworks:

* [Puppeteer](https://github.com/puppeteer/puppeteer)
* [Playwright](https://github.com/microsoft/playwright)
* [TestCafe](https://github.com/DevExpress/testcafe)
* [Cypress](https://github.com/cypress-io/cypress)
* [WebdriverIO](https://github.com/webdriverio/webdriverio)

After which, a `./sauce/config.yml` file and an example test under the tests directory will be created.

### `run`

```bash
saucectl run
```

This command will run the test based on the `./.sauce/config.yml` file.

## Flags

### `config`
```bash
saucectl run --config <path>
```

Using the `--config` flag will run the tests specified by that config file.

### `env`

```bash
saucectl run --env <key>=value1> --env <key2>=<value2> ...
```

Using the `--env` flag will define environment variables that are then available for use by the test framework.

### `parallel`

```sh
saucectl run --parallel=<true|false>
```

Using the `--parallel` flag allows the parallelization of tests across machines to be
turned on/off. 

`saucectl` will use CI provider specific clues from the environment and `saucectl` config
file to generate a `build ID`. This `build ID` is used a grouping mechanism to
synchronize the different machines that are running in the same pipeline to distribute
the tests. 

`saucectl` currently uses the following CI environment variables to generate a build ID.

| CI            | Environment Variables          | Current Limitations                                 |
|:-------------:|:------------------------------:|:---------------------------------------------------:|
| GitHub        | GITHUB_WORKFLOW, GITHUB_RUN_ID | Unable to re-run jobs. Must trigger a new pipeline. |
| GitLab        | CI_PIPELINE_ID, CI_JOB_STAGE   | Unable to re-run jobs. Must trigger a new pipeline. |
| Jenkins       | BUILD_NUMBER                   | None                                                |

The current parallelization feature is _highly experimental_ and may have limitations
with certain CI providers.

If your CI provider is not listed here, you will have to specify your own `build ID`.
Please consult the [`ci-build-id`](#ci-build-id) flag for this option.

### `ci-build-id`

```sh
saucectl run --ci-build-id <value>
```

Using the `--ci-build-id` flag will override the build ID that is otherwise determined
based on the CI provider. The config file hash will still be used in addition to this
provided CI build ID.

### `region`

```bash
saucectl run --region <region>
```

Using the --region flag will set the Sauce Labs region for the test execution. The region corresponds to the available regions at [saucelabs.com](https://app.saucelabs.com) and affects where your job information and assets are going to be stored.

### `timeout`
```bash
saucectl run --timeout <seconds>
```

Using the `--timeout` flag will set the test timeout for the [Testrunner framework](test-preparation.md#automation-framework-examples).

## Private registry
In case you need to use an image from a private registry you can use environment variables for authentification;

```bash 
export REGISTRY_USERNAME=registry-user
export REGISTRY_PASSWORD=registry-pass
```

and in your `config.yml`, add the image name to your registry like so:

```yml   
image:
  base: quay.io/saucelabs/stt-cypress-mocha-node
  version: latest
```

## Licensing

`saucectl` is licensed under the Apache License, Version 2.0. See [LICENSE](https://github.com/saucelabs/saucectl/blob/master/LICENSE) for the full license text.
