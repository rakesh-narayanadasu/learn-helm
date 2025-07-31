# Helm Validation

After developing Helm chart, it is crucial to verify its functionality. There are three primary methods to validate your Helm chart before installation:
1. Linting – Checks that the chart and its YAML syntax are correct.
2. Template Rendering – Confirms that the templating logic generates the expected manifest.
3. Dry Run Install – Simulates an installation on Kubernetes to catch issues that only Kubernetes validation can reveal.

**1. Linting the Chart**

Linting helps catch formatting errors and typos (for example, misaligned spaces or incorrect variable names such as a misspelling of "release"). Use the following command to lint your chart:

```
$ helm lint ./nginx-chart
==> Linting ./nginx-chart/
[INFO] Chart.yaml: icon is recommended
[ERROR] templates/: template: nginx-chart/templates/deployment.yaml:4:19: executing "nginx-chart/templates/deployment.yaml" at <.Release.Name>: nil pointer evaluating interface {}.Name
[ERROR] templates/deployment.yaml: unable to parse YAML: error converting YAML to JSON: yaml: line 20: did not find expected '-' indicator
Error: 1 chart(s) linted, 1 chart(s) failed
```

The above output indicates:
* An error on line 4 due to a typo in the variable name.
* A YAML indentation issue on line 20.

After addressing these issues, re-run the lint command. A successful linting process will output:

```
$ helm lint ./nginx-chart
==> Linting ./nginx-chart/
[INFO] Chart.yaml: icon is recommended
1 chart(s) linted, 0 chart(s) failed
```

**2. Verifying Template Rendering**

Once linting confirms correct formatting, the next step is to ensure that the templating logic produces the intended Kubernetes manifest. This process renders placeholders such as .Release.Name and variables defined in the values file.

Run the command below to render the template locally:

```
helm template hello-world-1 ./nginx-chart
```

Use the debug flag to help diagnose the issue:

```
helm template ./nginx-chart --debug
```

**3. Simulating an Installation with a Dry Run**

Linting and template rendering catch many issues; however, they might not detect errors within the final manifest applied to Kubernetes. For example, if the Deployment spec mistakenly uses "container" instead of "containers", the issue won't be caught by earlier checks.

Consider the following incorrect manifest snippet:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-nginx
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: hello-world
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      container:  # Incorrect field; should be "containers"
        - name: hello-world
          image: {{ .Values.image }}
          ports:
            - name: http
              containerPort: 80
              protocol: TCP
```

Since the YAML structure and templating appear correct, neither linting nor template rendering flags this error. To validate the entire manifest against Kubernetes standards, execute the following dry-run command:

```
$ helm install hello-world-1 ./nginx-chart --dry-run
Error: unable to build kubernetes objects from release manifest: error validating "": error validating data: [ValidationError(Deployment.spec.template.spec): unknown field "container" in io.k8s.api.core.v1.PodSpec, ValidationError(Deployment.spec.template.spec): missing required field "containers" in io.k8s.api.core.v1.PodSpec]
```

*NOTE: Using the dry run option is instrumental in verifying that Kubernetes accepts your final manifests. This step minimizes the risk of encountering runtime errors during actual deployment.*

**Summary**
* Linting: checks for formatting and syntax errors.
* Template Rendering: confirms correct variable substitution and manifest creation.
* Dry Run Installation: validates the final manifest against Kubernetes APIs.

