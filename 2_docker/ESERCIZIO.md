# Esercizio Azure DevOps Pipeline

## Scenario

Applicazione .NET 9 containerizzata.

Struttura repository:

.
├── src/
│   ├── Api/
│   │   ├── Api.csproj
│   │   └── Dockerfile
│
├── tests/
│   └── Api.Tests/
│       └── Api.Tests.csproj
│
└── azure-pipelines.yml

Obiettivo:
1. Build
2. Test
3. Build Docker
4. Push su ACR
5. Deploy su AKS

Pipeline esistente:

```yaml
trigger:
- main

pool:
  vmImage: ubuntu-latest

variables:
  imageName: myapp

steps:

- task: DotNetCoreCLI@2
  inputs:
    command: build
    projects: '**/*.csproj'

- task: Docker@2
  inputs:
    command: buildAndPush
    repository: $(imageName)
    Dockerfile: src/Api/Dockerfile
    containerRegistry: my-acr
    tags: latest

- script: |
    kubectl apply -f k8s/deployment.yaml
```

