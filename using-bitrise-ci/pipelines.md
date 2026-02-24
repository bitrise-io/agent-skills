# Building Pipelines

Pipelines allow creating complex processes from workflows with dependencies, parallel or sequential execution, sharding and sharing files. Documentation is available [here](https://docs.bitrise.io/en/bitrise-ci/workflows-and-pipelines/build-pipelines/configuring-a-bitrise-pipeline.html).

## Execution order

**Parallel execution**: Workflows that don't depend on each other can run concurrently
```yaml
pipelines:
  tests:
    workflows:
      test_ios:
        depends_on: [setup]
      test_android:
        depends_on: [setup]
      # test_ios and test_android run in parallel after setup completes
```

**Sequential execution**: Chain dependencies for ordered execution
```yaml
pipelines:
  ci:
    workflows:
      build:
        depends_on: [setup]
      test:
        depends_on: [build]
      deploy:
        depends_on: [test]
# Executes: setup → build → test → deploy
```

## Passing Data Between Workflows

Workflows do not have access to the data of other workflows, as they are running in separate virtual machines. To share artifacts or other data across workflow boundaries in pipelines, use the following pattern:

```yaml
workflows:
  build_app:
    steps:
      - xcode-archive@5: {}
      - deploy-to-bitrise-io@2:
          inputs:
            - deploy_path: $BITRISE_IPA_PATH

  test_integration:
    depends_on: [build_app]
    steps:
      - pull-intermediate-files@1: {}
      - script@1:
          inputs:
            - content: |
                # Access artifacts from previous workflow
                echo "Using artifacts from $BITRISE_PULL_ARTIFACTS_DIR"
```

Artifacts uploaded via `deploy-to-bitrise-io` in one workflow can be pulled by dependent workflows using `pull-intermediate-files`. Environment variables set in pipeline stages are accessible to all workflows in that pipeline.

## Test Sharding

Split test execution across parallel workflows for faster results:

```yaml
workflows:
  test-without-building:
    depends_on: [build-for-testing]
    parallel: 5
```
Each copy receives two new Environment Variables:
- `$BITRISE_IO_PARALLEL_INDEX`: a zero based index for each copy of the Workflow.
- `$BITRISE_IO_PARALLEL_TOTAL`: the total number of copies.

XCode test and Gradle runner also support automatic shard calculation, see [this page](https://docs.bitrise.io/en/bitrise-ci/testing/test-sharding.html) for details.

## Stages (legacy)

Older, existing configuration might be using [stages](https://docs.bitrise.io/en/bitrise-ci/workflows-and-pipelines/build-pipelines/pipelines-with-stages.html). Example:

```yaml
pipelines:
  pipeline-successful:
    stages:
    - stage-successful-1: {}
    - stage-successful-2: {}
    - stage-successful-3: {}
stages:
  stage-successful-1:
    workflows:
    - test-1: {}
  stage-successful-2:
    workflows:
    - build-1: {}
    - build-2: {}
  stage-successful-3:
    workflows:
    - deploy-1: {}
    - deploy-2: {}
workflows:
  test-1: {}
  build-1: {}
  build-2: {}
  deploy-1: {}
  deploy-2: {}
```

Workflows in a stage run in parallel. Stages run sequentially, after all workflows in the previous stage have completed.
