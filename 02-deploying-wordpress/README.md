**Deploying a WordPress Website on Kubernetes**

1. Add the Repository

    Begin by adding the Bitnami chart repository to your local Helm configuration.

    ```helm repo add bitnami https://charts.bitnami.com/bitnami```

2. Install the Chart

    With the repository added, deploy the WordPress chart to your Kubernetes cluster.

    ```helm install demo-wordpress bitnami/wordpress```

3. Listing Releases

    To view all installed releases, use the helm list command.

    ```helm list```

4. Uninstalling the Release

    When you need to remove the WordPress deployment, Helm allows you to easily clean up all associated Kubernetes objects.

    ```helm uninstall demo-wordpress```

**Managing Helm Repositories**

* List Current Repositories:

    ```helm repo list```

* Update Repositories:
    Similar to a system package manager, refresh the local cache of repository information with:

    ```helm repo update```

* Search in Artifact Hub:

    ```helm search hub wordpress```

* Search in repositories:

    ```helm search repo wordpress```

