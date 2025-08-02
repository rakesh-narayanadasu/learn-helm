# with Blocks

```with``` blocks in Helm charts can reduce repetition and improve template readability. By setting appropriate scopes, you can manage complex configurations more efficiently.

In this version, each reference begins at the root (.), and you must explicitly traverse the hierarchy, which can lead to repetitive expressions such as ```.Values.app.ui.bg```

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-appinfo
data:
  background: {{ .Values.app.ui.bg }}
  foreground: {{ .Values.app.ui.fg }}
  database: {{ .Values.app.db.name }}
  connection: {{ .Values.app.db.conn }}
```

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-appinfo
data:
{{- with .Values.app }}
  {{- with .ui }}
  background: {{ .bg }}
  foreground: {{ .fg }}
  {{- end }}
  {{- with .db }}
  database: {{ .name }}
  connection: {{ .conn }}
  {{- end }}
  release: {{ $.Release.Name }}
{{- end }}
```

*Note that you can use ```$.Release.Name``` to access the release name from the root context ($), as the current scope has been altered.*