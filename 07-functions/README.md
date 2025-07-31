# Helm Functions

When you combine a template with values defined in a values.yaml file, Helm generates a valid manifest file. However, if a field in values.yaml is not set, the corresponding section in the manifest may be omitted.

Eg: If the repository name is not set in values.yaml, the rendered manifest will lack an image value, leading to pod startup failures.

To resolve this issue, the chart can define default values. If the user does not provide a value in values.yaml (or via the command line), Helm can fall back to a default specified using a function. 

Helm functions behave similarly to those in conventional programming languages: they process an input and return a transformed output. For instance, the upper function converts a string to uppercase, and the trim function removes surrounding whitespace. Consider these examples:

```
upper("helm")  ➞  "HELM"
trim(" helm ")  ➞  "helm"
```

Helm also provides other useful functions like quote (which surrounds a string with quotes) and replace (which substitutes characters within a string). Here are a few examples:

```
{{ upper .Values.image.repository }}
{{ quote .Values.image.repository }}
{{ replace "x" "y" .Values.image.repository }}
{{ shuffle .Values.image.repository }}
```

These string manipulation functions are just a subset of what Helm offers. Additional functions cover areas such as cryptography, date handling, dictionary operations, Kubernetes object management, networking, type conversion, regular expressions, and URL handling.

**Using the Default Function**

If the value is not supplied in values.yaml or via the command line, the rendered manifest will omit an image, potentially causing a pod to fail. To ensure that your deployment always includes a valid image, use the default function. By specifying a default value (enclosed in quotes to treat it as a string), Helm will use "nginx" if no repository value is provided.

