# Named Templates

use named templates to eliminate repetitive code in your Helm charts. When creating Kubernetes manifests, you might notice that labels or other blocks often repeat across multiple objects.

*Keep your Helm charts DRY (Don't Repeat Yourself) by consolidating common code blocks in a helper file.*

**Moving Common Labels to a Helper File**

To address this redundancy, you can transfer the shared lines to a helper file (commonly named ```_helpers.tpl```). The underscore in the filename tells Helm to ignore this file when generating Kubernetes manifests. 

```
{{- define "labels" }}
  app.kubernetes.io/name: {{ .Release.Name }}
  app.kubernetes.io/instance: {{ .Release.Name }}
{{- end }}
```

```
apiVersion: v1
kind: Service
metadata:
  name: {{ .Release.Name }}-nginx
  labels:
    {{- template "labels" . }}
spec:
  ports:
    - port: 80
      targetPort: http
      protocol: TCP
      name: http
  selector:
    app: hello-world
```

*Notice that appending a dot (.) to the template call passes the current context into the helper file. Without it, the helper template wouldn’t have access to critical values like .Release.Name*

**Using the include Function for Proper Indentation**

To resolve the indentation problem, use the ```include``` function instead of ```template```. The ```include``` function allows the output to be piped to other functions like ```indent```.