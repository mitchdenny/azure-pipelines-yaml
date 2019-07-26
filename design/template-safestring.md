# Template ```safestring``` and ```uniquestring``` functions

This is the proposal for the addition of two new functions that would be supported during the template evaluation phase of starting an Azure Pipelines run. The two functions are:

```
${{safestring(mandatory, optional1, optional2, ...)}}
```

... and:


```
${{uniquestring(mandatory, optional1, optional2, ...)}}
```

To understand why these functions would be useful, consider the following scenario. I have a template which is used to sign and build multiple components.

```yaml
parameters:
  Packages: []

stages:
- stage: Sign
  jobs:
  - job: SignComponents
    steps:
    - script: |
        echo "Configuring GPG."
    - ${{ each package in parameters.Packages }}:
      - script: |
          echo "Doing some GPG signing for: ${{package}}"

- ${{ each package in parameters.Packages }}:
  - stage: Prerelease
    jobs:
    - job: PublishPackage
      
```