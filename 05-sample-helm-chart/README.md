# Objects

**Release Object:**

Release.Name – The name of the Helm release.

Release.Namespace – The Kubernetes namespace where the release is deployed.

Release.IsUpgrade – Boolean indicating if the current operation is an upgrade.

Release.IsInstall – Boolean indicating if the current operation is a fresh install.

Release.Revision – The revision number of the release, incremented on each upgrade or rollback.

Release.Service – The name of the Helm service managing the release (usually "Helm").

**Chart Object:**

Chart.Name – The name of the Helm chart.

Chart.ApiVersion – The API version of the chart definition (e.g., v2).

Chart.Version – The version of the chart itself (defined in Chart.yaml).

Chart.Type – The type of the chart, typically "application" or "library".

Chart.Keyword – A list of keywords that describe the chart (used for discovery).

Chart.Home – The URL to the homepage for the chart (usually project or documentation site).

**Capabilities Object:**

Capabilities.KubeVersion – The version of the Kubernetes cluster Helm is interacting with.

Capabilities.ApiVersion – The default Kubernetes API version used by the chart templates.

Capabilities.HelmVersion – The version of Helm itself.

Capabilities.GitCommit – The Git commit hash of the Helm build (if available).

Capabilities.GitTreeState – Indicates if the Helm source tree was clean or dirty at build time.

Capabilities.GoVersion – The version of Go used to build the Helm binary.