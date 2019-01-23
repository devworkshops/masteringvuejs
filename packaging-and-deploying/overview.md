# Continuous Integration

[YAML schema documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=vsts&tabs=schema#task)

[Azure Pipelines YAML Tasks](https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/index?view=vsts)

{% code-tabs %}
{% code-tabs-item title="azure-pipelines.yml" %}
```yaml
queue:
  name: Hosted VS2017
  demands: npm

trigger:
  - master

steps:
  - task: Npm@1
    displayName: "npm install"
    inputs:
      verbose: false

  - task: Npm@1
    displayName: "unit testing"
    inputs:
      command: custom
      verbose: false
      customCommand: "run test:unit"

  - task: Npm@1
    displayName: "e2e testing"
    inputs:
      command: custom
      verbose: false
      customCommand: "run test:e2e:ci"

  - task: Npm@1
    displayName: "build"
    inputs:
      command: custom
      verbose: false
      customCommand: "run build"

  - task: PublishBuildArtifacts@1
    displayName: "Publish Artifact: frontend"
    inputs:
      PathtoPublish: "dist"
      ArtifactName: frontend

```
{% endcode-tabs-item %}
{% endcode-tabs %}

