# Helm-Kubernetes-Package-Manager

# Helm: A Package Manager for Kubernetes

Helm is a package manager for Kubernetes, similar to how you might use `apt` for Ubuntu or `npm` for Node.js. It simplifies the process of deploying and managing applications in a Kubernetes cluster.

## Core Concepts of Helm

### Charts

- **Chart** is a package of pre-configured Kubernetes resources. Think of it like a blueprint for an application. It includes everything you need to run an application or a service in Kubernetes, such as deployments, services, and configurations.
- Charts are reusable and can be shared with others. For example, there are official Charts for popular applications like MySQL, WordPress, and more.

### Releases

- When you install a Chart in your Kubernetes cluster, it creates a **Release**. A Release is a running instance of a Chart.
- You can install the same Chart multiple times, and each installation would be a separate Release with its own configuration.

### Values

- **Values** are the configurable settings for a Chart. They allow you to customize the behavior of the application without changing the Chart itself.
- For example, you can specify different resource limits, environment variables, or even a custom image for the application.

### Templates

- Charts use **Templates** to generate the Kubernetes manifest files (like Deployment, Service, etc.). These templates are written in Go template language and are rendered using the values you provide.
- This makes Helm very flexible, as you can use the same Chart with different values to deploy slightly different versions of the same application.

## Why Use Helm?

- **Simplification**: Helm simplifies the deployment process by packaging all the resources into a single Chart.
- **Version Control**: Just like you have version control for code, Helm Charts can have versions. This allows you to upgrade, roll back, or even roll forward deployments easily.
- **Sharing and Reusability**: You can share your Charts with others or use Charts created by the community, saving time and effort.

## Example

Let's say you want to deploy a web application that includes a front-end, back-end, and a database. Instead of writing and managing all the Kubernetes YAML files manually, you can use a Helm Chart that packages everything together. You just need to install the Chart with `helm install`, and Helm will take care of deploying all the necessary resources in your cluster.

# Deploying a Web Application with Helm in Kubernetes

This guide walks through a simple example of how Helm works in a Kubernetes environment by deploying a basic web application.

## Scenario: Deploying a Web Application with Helm

Imagine you want to deploy a simple web application that consists of two components:

- **Frontend**: A static website running on Nginx.
- **Backend**: A Node.js application that serves as the API.

Without Helm, you would need to manually create and manage the Kubernetes YAML files for each component, which could be error-prone and time-consuming. With Helm, you can use a pre-packaged Chart that includes everything you need.

## Step 1: Install Helm

First, you need to install Helm on your local machine. You can do this with a simple command:

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```
## Step 2: Find a Helm Chart
Helm has a repository of Charts that you can use. For this example, let’s say there’s a Chart called my-web-app that packages both the frontend and backend.

You can search for available Charts using:
`helm search repo my-web-app`

Let’s assume you find a Chart that fits your needs.

## Step 3: Install the Chart
To deploy the web application, you simply run:

`helm install my-release my-web-app`

my-release: This is the name you give to your deployment (the Release).
my-web-app: This is the name of the Chart you’re using.

# What Happens Behind the Scenes:

Templates: Helm takes the templates inside the my-web-app Chart and uses them to generate the necessary Kubernetes YAML files.
Values: Helm uses default settings from the Chart's values.yaml file unless you override them.
Deployment: Helm applies the generated Kubernetes manifests to your cluster, creating a deployment for both the frontend and backend, along with services to expose them.

## Step 4: Customizing the Deployment
To change the number of replicas for the frontend to 3, you can pass a custom value:
`helm install my-release my-web-app --set frontend.replicaCount=3`

Helm will adjust the configuration and deploy the application with the specified settings.

## Step 5: Managing the Release
Helm keeps track of the deployed release, making it easy to manage:

Upgrade: To upgrade your application or change settings:
`helm upgrade my-release my-web-app`

Rollback: If something goes wrong, roll back to a previous version:
`helm rollback my-release 1`

Uninstall: To remove the application from your cluster:

`helm uninstall my-release`

Example in Action
Initial Installation:
`helm install my-release my-web-app`

This command deploys the web application with default settings.

Custom Installation with Values:\
`helm install my-release my-web-app --set frontend.replicaCount=3,backend.image.tag=v2`

This command customizes the installation by setting the frontend replicas to 3 and specifying that the backend should use the v2 image tag.

Upgrading the Release:
`helm upgrade my-release my-web-app --set frontend.replicaCount=5`

This command changes the number of frontend replicas from 3 to 5.

Rolling Back:
`helm rollback my-release 1`

If the upgrade causes issues, you can roll back to the first release.

