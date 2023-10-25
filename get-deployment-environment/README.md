# Determine deployment enviroment based on branch

A GitHub Action to detect which environment should be used for a given branch instead of creating multiple workflows/steps.

## Usage

```yaml
- name: Get Environment
  id: get-environment
  uses: Diakrit/github-actions/get-deployment-environment@master

- run: echo ${{ steps.get-environment.outputs.env }}
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