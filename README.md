# learn-helm

What is Helm?

Helm simplifies deployment on Kubernetes by allowing you to package all your Kubernetes resources (like Deployments, Services, ConfigMaps, etc.) into one reusable unit called a chart. Think of it as the "installer" for complex Kubernetes apps.

**Real-World Analogy: Installing WordPress on Kubernetes**
Suppose you want to deploy WordPress on Kubernetes. To do this manually, you’d have to:
* Create a Deployment for WordPress
* Create a Service for WordPress
* Create a Deployment for MySQL
* Create a PersistentVolumeClaim
* Create ConfigMaps or Secrets
* Link everything together correctly

That’s a lot of YAML files and configuration!

Instead, with Helm:

You run:

```helm install my-wordpress bitnami/wordpress```

And Helm takes care of installing everything needed in one go the right order, with templates, defaults, and customization options.

**Helm Concepts**

A typical Helm chart directory structure includes:
* A templates directory containing all templated resource manifests.
* A values.yaml file that defines configuration parameters.
* A Chart.yaml file holding chart metadata.
* Optionally, a charts directory for dependencies and files such as a README or license.

| Term            | Description                                                                                  |
| --------------- | -------------------------------------------------------------------------------------------- |
| **Chart**       | A Helm package that contains all necessary Kubernetes resource definitions (YAML templates). |
| **Release**     | A specific deployed instance of a chart in your Kubernetes cluster.                          |
| **Revision**     | A revision is a version of a release, updated over time.                              |
| **Repository**  | A place where Helm charts are stored and shared (e.g., Bitnami, ArtifactHub).                |
| **Values.yaml** | A file where you define custom configuration for charts (overrides default values).          |
| **Templates**   | Helm uses Go templating to let you dynamically render Kubernetes manifests.                  |

**Multiple Environments**

```
helm install <release-name> <chart-name>
helm install my-first-site bitnami/wordpress
helm install my-second-site bitnami/wordpress
```
Release names are how Helm tracks individual deployments. So, by changing just the release name, you can reuse the same chart for different apps or environments (dev, staging, prod, etc.).

