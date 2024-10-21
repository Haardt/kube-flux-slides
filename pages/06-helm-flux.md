---
transition: slide-left
layout: center
background: './images/manifests.webp'
---

# How to manage all this files?

---
layout: default
transition: slide-left
---

# **What is Helm?**

- Helm is a **package manager** for Kubernetes, used to define, install, and manage Kubernetes applications as **charts**.
- A Helm chart is a collection of files that describe a related set of Kubernetes resources.

---
layout: default
transition: slide-left
---

# **Helm Charts**

- **Helm charts** are reusable Kubernetes resource definitions that define an application or set of services.
- Charts can be easily shared and versioned, allowing developers to install applications with a single command.

---
layout: default
transition: slide-left
---

# **Chart Structure**
A typical Helm chart has the following structure:
```
chart-name/
  ├── Chart.yaml           # Metadata about the chart (name, version, etc.)
  ├── values.yaml          # Default configuration values for the chart
  ├── templates/           # Kubernetes manifest templates (e.g., deployment, service)
  └── charts/              # Dependencies (other charts)
```

---
layout: default
transition: slide-left
---
# **Helm Commands**

Some of the most important Helm commands include:
- **`helm install`**: Installs a chart into a Kubernetes cluster.
- **`helm upgrade`**: Upgrades a release with a new chart version or new values.
- **`helm uninstall`**: Uninstalls a release from the Kubernetes cluster.
- **`helm list`**: Lists installed Helm releases.
- **`helm repo add`**: Adds a Helm chart repository.
- **`helm repo update`**: Updates the local list of charts from the repositories.

---
layout: default
transition: slide-left
---
# **Helm Repositories**

- Helm repositories are online collections of Helm charts.
- You can add repositories (e.g., **ArtifactHub**, the official public Helm chart repository) using the `helm repo add` command.
- Charts can be published to custom or public repositories for sharing.

---
layout: default
transition: slide-left
---
# **Helm Releases**

- A **release** is an instance of a Helm chart running in your Kubernetes cluster.
- Every time you install or upgrade a chart, Helm creates or updates a release.

---
transition: slide-left
layout: two-cols
---
# **Values and Configuration**
- Helm uses a `values.yaml` file to define the default configuration of the chart.
- Users can override the values by passing a custom `values.yaml` file or using the `--set` flag with key-value pairs when installing/upgrading.

```bash
helm install my-release my-chart \
            --values custom-values.yaml
```
<br>

### **Templating Engine**

- Helm uses the Go templating engine to allow dynamic values and logic in Kubernetes manifests.
- The **`templates/`** directory support variable substitution, loops, conditionals, and functions.

::right::
## Template example

```yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  # The release name is used dynamically
  name: {{ .Release.Name }}-app  
  labels:
    app: {{ .Values.app.name }}  
    environment: {{ .Values.app.environment | default "production" }}  
spec:
  replicas: {{ .Values.replicaCount }}
#  .....
  containers:
        - name: {{ .Values.app.name }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"  #

```
---
layout: default
transition: slide-left
---

# **Helm Packaging**
- You can package your Helm chart into a `.tgz` file using the `helm package` command, which can then be uploaded to a Helm repository for sharing.

## **Helm Chart Dependencies**

- Helm charts can have dependencies on other charts. Dependencies are managed in the `Chart.yaml` and installed automatically when the main chart is installed.


## **Release Management**
- Helm manages the complete lifecycle of an application from installation, upgrades, rollbacks, and uninstallation.


---
layout: default
transition: slide-left
---
# **Helm Rollback**
- Helm supports rollback to previous chart releases, allowing you to revert your application to a known working state if something goes wrong.

## **Helm Hooks**

- Helm supports **hooks**, which allow you to run specific Kubernetes resources at different lifecycle stages of your release (e.g., pre-install, post-upgrade).


```bash
helm rollback my-release 1
```

---
layout: default
transition: slide-left
---

# **Conclusion**

## **Deploying Applications** 

Easily deploy complex applications like databases, monitoring systems, or web services.

## **Managing Application Configurations**

Centralize application configuration and easily apply changes.
## **Application Versioning**

Keep track of application versions and roll back to previous versions if necessary.

---
layout: center
transition: slide-left
background: ./images/chaos-with-helm.webp
---

# Should you use Helm?

<div v-click>

# NO!

</div>


<div v-click>

# not without flux (GitOps) :)!

</div>


---
layout: left
transition: slide-left
---
# GitOps

- **Infrastructure as Code (IaC)**: GitOps manages infrastructure and application deployments using version-controlled code in Git repositories.
- **Git as the Source of Truth**: All changes to infrastructure and applications are made through Git, ensuring a clear audit trail and version history.
- **Automated Deployments**: Changes pushed to the repository trigger automated deployment processes, keeping environments in sync with the Git state.
- **Continuous Reconciliation**: Kubernetes <span v-mark.underline.orange>operators</span> continuously monitor the system state and reconcile it to match the desired state defined in Git.

This approach ensures <span v-mark.underline.orange>consistency</span>, <span v-mark.underline.orange>reliability</span>, and <span v-mark.underline.orange>easy rollbacks</span>.

---
layout: left
transition: slide-left
---
# DORA

The **DORA Metrics** assess software development and delivery performance through four key metrics:

- **Deployment Frequency**: Measures how often code is deployed to production, indicating delivery speed.
- **Lead Time for Changes**: Tracks the time from code commit to production, reflecting development efficiency.
- **Change Failure Rate**: Shows the percentage of deployments causing production issues, indicating release stability.
- **Mean Time to Recovery (MTTR)**: Measures how quickly teams recover from failures, reflecting system reliability.

These metrics help optimize <span v-mark.underline.orange>DevOps</span> practices for faster, more stable software delivery.

---
layout: left
transition: slide-left
---
# Flux

---
layout: left
transition: slide-left
---
# Flux manifests

---
layout: left
transition: slide-left
---
# Flux HelmRelease Manifest

---
layout: left
transition: slide-left
---
# Flux 1

---
layout: left
transition: slide-left
---
# Flux 2

---
layout: left
transition: slide-left
---
# Flux SOPS




