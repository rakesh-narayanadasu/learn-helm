# Lifecycle management with Helm

Helm simplifies the entire lifecycle management of applications on Kubernetes. Helm tracks, upgrades, and rolls back releases, ensuring smooth and efficient management of your deployments.

**What Is a Release?**

When you install a Helm chart, a release is created. A release is similar to an application—it is a package or collection of Kubernetes objects. Helm meticulously keeps track of the objects associated with each release. This allows you to upgrade, downgrade, or uninstall a release independently without affecting other releases. For example, you can generate multiple releases from the same chart:

```
helm install my-first-site bitnami/wordpress
helm install my-second-site bitnami/wordpress
```

**Installing a Specific Chart Version**

Let’s create a new release to see lifecycle management in action. In this example, we install an older version of the NGINX chart by specifying the version option:

```helm install nginx-release bitnami/nginx --version 7.1.0```

After installation, you have an NGINX release named "nginx-release." Over time—say, two months—security vulnerabilities might be discovered that require patching. In your Kubernetes cluster, the NGINX release might comprise several objects. When you upgrade the Pods running NGINX, you might also need to update other Kubernetes objects (for example, by adding a new environment variable or secret) so that the manifest meets the requirements of the new version.

**Verifying the Deployed Version**

Helm streamlines the upgrade process by tracking all objects associated with a release, enabling upgrades with a single command. First, verify the version running in your Pod by listing all Pods and then inspecting the target Pod:

```
$ kubectl get pods
NAME                                   READY   STATUS          RESTARTS   AGE
nginx-release-687cdd5c75-ztn2n        0/1     ContainerCreating   0          13s
```

Once the Pod is running, describe it to confirm the image version:

```
$ kubectl describe pod nginx-release-687cdd5c75-ztn2n
Containers:
  nginx:
    Container ID:   docker://81bb5ad6b5...
    Image:          docker.io/bitnami/nginx:1.19.2-debian-10-r28
    Image ID:       docker-pullable://bitnami/nginx@sha256:2fcaf026b8acb7a...
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
```

**Upgrading the Release**

To upgrade to a newer version, run the following Helm upgrade command:

```
$ helm upgrade nginx-release bitnami/nginx
Release "nginx-release" has been upgraded. Happy Helming!
NAME: nginx-release
LAST DEPLOYED: Mon Nov 15 19:25:55 2021
NAMESPACE: default
STATUS: deployed
REVISION: 2
TEST SUITE: None
NOTES:
CHART NAME: nginx
CHART VERSION: 9.5.13
APP VERSION: 1.21.4
```

During this upgrade, Helm replaces the old Pod with a new one running the updated version (NGINX 1.21.4). You can verify the change by listing the Pods again and describing the new Pod.

**Tracking Release History**

Helm’s lifecycle management maintains a detailed history of each release state. After upgrading, list all releases to see the current revision:

```
$ helm list
NAME            NAMESPACE   REVISION    STATUS      CHART         APP VERSION
nginx-release   default     2           deployed    nginx-9.5.13  1.21.4
```

To view detailed information about each revision, use the Helm history command:

```
$ helm history nginx-release
REVISION    UPDATED                     STATUS      CHART         APP VERSION    DESCRIPTION
1           Mon Nov 15 19:20:51 2021   superseded  nginx-7.1.0   1.19.2        Install complete
2           Mon Nov 15 19:25:55 2021   deployed    nginx-9.5.13  1.21.4        Upgrade complete
```

**Rolling Back Upgrades**

If an upgrade introduces undesired changes, Helm supports rollbacks. For instance, to return to revision 1, execute:

```
$ helm rollback nginx-release 1
Rollback was a success! Happy Helming!
```

Even though the configuration reverts to that of revision 1, Helm creates a new revision (e.g., revision 3) with the same configuration, ensuring a complete record of all release states.

**NOTE:**

*Although Helm manages the configuration and manifest files for Kubernetes objects, it does not back up external data. If your application uses persistent volumes or an external database, a rollback will restore only the pods and configurations. For persistent data, consider using Chart Hooks or backup solutions.*

