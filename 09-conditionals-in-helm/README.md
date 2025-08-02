# Conditionals in Helm

Conditionals in Helm as used to include or exclude sections of your templates based on values specified in the values file. With these techniques, you can add optional configurations such as custom labels, control whitespace, and conditionally render entire objects like service accounts all of which add flexibility to your deployments.

**Example: Adding Optional Labels**

Consider a basic Helm chart with a simple service template. Initially, you might have a values.yaml file with minimal configuration and a corresponding service.yaml template. Now, suppose different releases require different labels—for example, adding an organizational label to group objects. However, when the orgLabel value is optional, you want these label lines to appear only when the value is provided. Similar to conditionally executing code in programming languages such as Python:

```
orgLabel = "payroll"
if orgLabel:
    print(orgLabel)
```

In Helm charts, you can achieve the same behavior using if blocks. Here’s an updated version of the service.yaml file that conditionally adds the organization label:

```
# service.yaml
apiVersion: v1
kind: Service
metadata:
  name: {{ .Release.Name }}-nginx
  {{- if .Values.orgLabel }}    
  labels:
    org: {{ .Values.orgLabel }}
  {{- end }}
spec:
  ports:
    - port: 80
      name: http
  selector:
    app: hello-world
```

*Note: The dash (-) after the opening braces ({{-) ensures that Helm trims any preceding whitespace, preventing extra blank lines in the rendered output.*

**Using If, Else If, and Else Blocks**

Helm templates support conditional logic that includes ```if```, ```else if```, and ```else``` constructs. Similar to Python's conditional logic, you can use helper functions such as eq for equality comparisons. Consider this Python pseudocode:

```
orgLabel = "payroll"


if orgLabel:
    print(orgLabel)
elif orgLabel == "hr":
    print("human resources")
else:
    print("nothing")
```

You can achieve similar logic in a Helm template as follows:

```
# service.yaml
apiVersion: v1
kind: Service
metadata:
  name: {{ .Release.Name }}-nginx
  {{- if .Values.orgLabel }}
  labels:
    org: {{ .Values.orgLabel }}
  {{- else if eq .Values.orgLabel "hr" }}
  labels:
    org: human resources
  {{- end }}
spec:
  ports:
    - port: 80
      name: http
  selector:
    app: hello-world
```

In this template, if ```orgLabel``` is provided, its value is used. If it specifically equals ```"hr"```, the label is set to "human resources". This approach leverages Helm's helper functions for clear and concise conditional logic.

**Conditional Creation of Objects**

A common use case for Helm conditionals is to control the creation of certain **Kubernetes objects** based on configuration. For example, you might want to create a ServiceAccount only if it is explicitly enabled in your ```values.yaml``` file.

In your default values.yaml, you can add a section like this:

```
# Default values for nginx-chart.
serviceAccount:
  # Specifies whether a ServiceAccount should be created
  create: true
```

Then, wrap the entire ServiceAccount template in a conditional block to ensure it is created only when needed:

```
{{- if .Values.serviceAccount.create }}
apiVersion: v1
kind: ServiceAccount
metadata:
  name: {{ .Release.Name }}-robot-sa
{{- end }}
```

This technique provides significant flexibility, allowing users to control which resources are deployed according to their specific requirements.