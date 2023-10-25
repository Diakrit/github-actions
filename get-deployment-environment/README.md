# Determine deployment enviroment based on branch

A GitHub Action to detect which environment should be used for a given branch instead of creating multiple workflows/steps.

## Usage
Add an initial job at the beginning of the workflow to determine the environment as seen in the example below:

```yaml
jobs:
  get-environment:
    runs-on: ubuntu-latest
    outputs:
      env_name: ${{ steps.get-environment.outputs.env_name }}
    steps:
      - name: Determine deployment environment
        id: get-environment
        uses: Diakrit/github-actions/get-deployment-environment@main
```

To reference the output created in the first job each job that are dependant on the information requires to be run after the first job.
Example of how to reference and set the deployment environment name in a later job can be seen below:

```yaml
  deploy:
    needs: [get-environment, test, build]
    runs-on: ubuntu-latest
    environment:
      name: ${{ needs.get-environment.outputs.env_name }}
```

### Example of input overrides
Below example will override the environment name for develop branch to output `"staging"` environment and `"production"` for master/main branch instead of the default

```yaml
jobs:
  get-environment:
    runs-on: ubuntu-latest
    outputs:
      env_name: ${{ steps.get-environment.outputs.env_name }}
    steps:
      - name: Determine deployment environment
        id: get-environment
        uses: Diakrit/github-actions/get-deployment-environment@main
        with:
          develop-branch-env-name: "staging"
          master-branch-env-name: "production"
```

## Inputs

| Input                         | Description                                                  | Default              |
| ----------------------------- | ------------------------------------------------------------ | -------------------- |
| `feature-branch-env-name`     | Name of deployment environment for feature branches          | `"uat"`              |
| `develop-branch-env-name`     | Name of deployment environment for develop branch            | `"qa"`               |
| `master-branch-env-name`      | Name of deployment environment for master/main branch        | `"staging"`          |
| `tag-env-name`                | Name of deployment environment for tags and releases         | `"production"`       |

## Outputs

| Output                 | Description                                                                                |
| ---------------------- | ------------------------------------------------------------------------------------------ |
| `env_name`             | Name of environment to be used in later steps                                              |