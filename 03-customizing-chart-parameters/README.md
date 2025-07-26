# Customizing chart parameters

When deploying WordPress, the application configuration is defined via YAML files. For example, the Deployment template below sets several environment variables, such as WORDPRESS_BLOG_NAME, which derives its value from the values.yaml file.

The default values in the values.yaml file are intended to work “out-of-the-box.” Overriding them allows you to optimize the deployment for your specific needs.

**Customizing Values Using Command Line Parameters**

Helm deploys the WordPress application immediately using a single command, with no interactive prompt to modify the values.yaml file. To override defaults at installation time, use the ```--set``` flag. For example, the command below changes the blog name from "User's Blog!" to "Helm Tutorial":

```helm install --set wordpressBlogName="Helm Learning" demo bitnami/wordpress```

You can include multiple ```--set``` flags to customize several parameters simultaneously.

**Using a Custom Values File**

When you need to override many default settings, using a custom values file is more efficient. Follow these steps to create and use a custom values file:

1. Create a file named custom-values.yaml with the desired parameters:

```
wordpressBlogName: Helm Tutorials
wordpressEmail: john@example.com
```

2. Install the chart and override the default values with your custom file:

```
helm install --values custom-values.yaml demo bitnami/wordpress
```

This method updates the chart's default settings with the parameters provided in your custom file.

**Modifying the Built-In values.yaml File**

If you prefer making permanent changes to the chart's default configuration, you can pull the chart locally, edit the built-in values.yaml file, and deploy from your modified version. Here are the steps:

1. Pull the chart in its compressed form:

```
helm pull bitnami/wordpress
```

2. To pull and untar the chart in one step, run:

```
helm pull --untar bitnami/wordpress
```

After untarring, a directory named ```wordpress``` will be created. List its contents to verify the structure:

```ls wordpress```

Open the ```values.yaml``` file in your favorite text editor, apply your desired changes, and then install the chart from this local directory:

```helm install demo ./wordpress```


# Helm Installation Methods

A quick reference guide to different ways to override values when installing a Helm chart.

| Method                           | Use Case                                              | Example Command                                                                 |
|----------------------------------|--------------------------------------------------------|----------------------------------------------------------------------------------|
| Override with `--set` flag   | Change a few parameters on the fly                    | `helm install --set wordpressBlogName="Helm Learning" demo bitnami/wordpress`  |
| Use a custom values file     | Override several defaults using a separate YAML file  | `helm install --values custom-values.yaml demo bitnami/wordpress`         |
| Modify the built-in `values.yaml` locally | Permanently adjust defaults and deploy from a local chart | `helm install demo ./wordpress`                                           |

