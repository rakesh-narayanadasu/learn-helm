# Pipelines

When working in a Linux environment, the output of the echo command simply prints the provided string. However, there are situations where you may want to take this output and process it with another command. This is where pipes (or pipelines) come into play.

For example:

```
$ echo "abcd"
abcd
$ echo "abcd" | tr a-z A-Z
ABCD
```

# Pipelines in Helm Templating

Helm templating allows you to efficiently manipulate variables using functions. Typically, functions are written with the function name preceding the variable names. 

For instance:

```
{{ upper .Values.image.repository }}
# Output: image: NGINX
```

An alternative and more common approach is to use pipelines by positioning the function name at the end of the variable. Pipelines allow you to chain multiple functions together, thereby enhancing readability and flexibility in your templates.

Consider the following example, which converts the repository value to uppercase and then encloses it in quotes:

```
{{ .Values.image.repository | upper | quote }}
# Output: image: "NGINX"
```

*Note: Remember that pipelines not only make your templating code cleaner but also allow you to perform complex transformations in a readable and efficient manner.*
