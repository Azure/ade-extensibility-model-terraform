# Azure Deployment Environments - Leveraging ADE's Extensibility Model With Terraform
This repository contains the components of an ADE-compatible sample image (a Dockerfile and shell scripts for deployment and deletion) to deploy and delete environments based off of Terrafomrm Infrastructure-as-Code (IaC) templates.

Azure Deployment Environments(ADE) empowers development teams to quickly and easily spin-up app infrastructure with project-based templates that establish consistency and best practices while maximizing security, compliance, and cost efficiency. This on-demand access to secure environments accelerates the different stages of the software development lifecycle in a compliant and cost-efficient manner.

An Environment is a collection of Azure resources on which your application is deployed. For example, to deploy a web application, you might create an environment consisting of an App Service, Key Vault, Cosmos DB and a Storage account. An environment could consist of both Azure PaaS and IaaS resources such as AKS Cluster, App Service, VMs, databases, etc.

One of ADE's newest features is its' extensibility model, which allows customers to develop their own container images to deploy their infrastructure templates, allowing for customization of their deployment to use any type of Infrastructure-as-Code framework and perform additional operations to aid deployment or deletion of a customer's environment. Documentation for this new feature can be found [here](https://learn.microsoft.com/en-us/azure/deployment-environments/how-to-configure-extensibility-generic-container-image).

This repository offers sample code for leveraging the new extensibility model feature with the Terraform CLI inside a customer's own container image, as well as offering a GitHub Action for customers to build and push an image to a provided Azure Container Registry (ACR). Customers can reference a provided ACR image link within an environment definition in ADE to deploy or delete an environment with the provided image. 

## GitHub Action Prerequisites
In order to use the workflow, you will need to:
- Fork this repository into your personal account
- Allow GitHub Actions to connect to Azure via an Microsoft Entra ID application's federated credentials through OIDC. You can find more documentation about this process [here](https://learn.microsoft.com/en-us/azure/developer/github/connect-from-azure?tabs=azure-cli%2Clinux)
- Set up Repository Secrets for your repository containing your Microsoft Entra ID application's application ID set as AZURE_CLIENT_ID, the subscription ID set as AZURE_SUBSCRIPTION_ID, and the tenant ID set as AZURE_TENANT_ID
- Set up Repository Variables for your repository containing your personal Azure Container Registry (ACR) name, your preferred repository name, and your preferred tag for the created image. You can modify your variables between workflow runs to push the generated image to different registries, repositories and tags.

## Utilizing the Created Image Within ADE
Once you have set up your repository and ran the 'build-and-push-image' workflow, you'll simply need to update your environment definition's manifest file (environment.yaml or manifest.yaml) and specify the created image as the definition's 'runner' property, shown below:
```yaml
runner: "{YOUR_REGISTRY}.azurecr.io/{YOUR_REPOSITORY}:{YOUR_TAG}"
```

For additional documentation around Azure Deployment Environments and its' extensibility model, view the Deployment Environments GitHub repo [here](https://github.com/Azure/deployment-environments/blob/main/documentation/custom-image-support/README.md) or MSLearn documentation [here](https://learn.microsoft.com/en-us/azure/deployment-environments/how-to-configure-extensibility-generic-container-image).
