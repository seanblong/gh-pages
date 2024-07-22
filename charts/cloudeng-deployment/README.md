# cloudeng-deployment

This template is intended to create a `deployment` workload along with other K8s
resources like a service account, service, horizontal pod autoscaler, etc.

## Parameters

### Global

The global section is a good place to expose common values
that require overrides between environments, e.g. featureFlagEnabled=false.
The only required value is the environment name.

| Name                          | Description                                                       | Value   |
| ----------------------------- | ----------------------------------------------------------------- | ------- |
| `global.env`                  | the environment name                                              | `""`    |
| `global.projectID`            | the GCP project ID, required when setting up GCP service accounts | `""`    |
| `global.enableDatadogJavaAPM` | enable Datadog Java APM                                           | `false` |

### Deployment Parameters

The following parameters are common in orchestrating a deployment,
including image version, resource requirements, service accts, and services/ingress.

| Name                                  | Description                                                                                               | Value                            |
| ------------------------------------- | --------------------------------------------------------------------------------------------------------- | -------------------------------- |
| `env`                                 | the environment name                                                                                      | `{{ .Values.global.env }}`       |
| `image.repository`                    | deployment's image repository (full path minus the tag)                                                   | `""`                             |
| `image.tag`                           | deployment's image tag                                                                                    | `""`                             |
| `image.pullPolicy`                    | deployment's image pull policy                                                                            | `IfNotPresent`                   |
| `imagePullSecrets`                    | credentials for                                                                                           | `cluster-artifactory-creds`      |
| `nameOverride`                        | overrides the name of the deployment                                                                      | `""`                             |
| `fullnameOverride`                    | overrides the full name of the deployment                                                                 | `""`                             |
| `podLabels`                           | additional labels as needed (note: default selector labels are automatically added)                       | `{}`                             |
| `podAnnotations`                      | additional annotations as needed                                                                          | `{}`                             |
| `initContainers`                      | add additional init containers to the deployment pods                                                     | `[]`                             |
| `sidecars`                            | add additional sidecar containers to the deployment pods                                                  | `[]`                             |
| `command`                             | command for running the container (ignored if not set).                                                   | `[]`                             |
| `args`                                | args for running the container (ignored if not set).                                                      | `[]`                             |
| `safeToEvict`                         | whether or not the pod is safe to evict                                                                   | `true`                           |
| `requests_cpu`                        | The amount of CPU to request for the container                                                            | `100m`                           |
| `requests_memory`                     | the amount of memory to request for the container                                                         | `128Mi`                          |
| `limits_cpu`                          | the amount of CPU to limit for the container                                                              | `""`                             |
| `limits_memory`                       | the amount of memory to limit for the container                                                           | `""`                             |
| `extraResourcesRequests`              | extra resources requests for the container other than CPU and Memory (ignored if not set).                | `{}`                             |
| `extraResourcesLimits`                | extra resources limits for the container other than CPU and Memory                                        | `{}`                             |
| `podSecurityContext`                  | security context for the container                                                                        | `{}`                             |
| `securityContext`                     | security context for the container                                                                        | `{}`                             |
| `enableDatadogJavaAPM`                | enable Datadog Java APM                                                                                   | `false`                          |
| `linkerd.enabled`                     | enable Linkerd for deployment                                                                             | `false`                          |
| `linkerd.inject`                      | enable Linkerd annotation to enable a service mesh mode.  Accepted values: enabled, disabled, and ingress | `enabled`                        |
| `linkerd.configAnnotations`           | configuration annotations from Linkerd to inject in deployment pods                                       | `{}`                             |
| `replicaCount`                        | number of replicas to run (ignored if autoscaling.enabled is true)                                        | `1`                              |
| `autoscaling`                         | configuration for autoscaling the deployment                                                              | `{}`                             |
| `envVars`                             | environment variables to set in the container                                                             | `[]`                             |
| `envVarsCM`                           | configmaps to mount as environment variables                                                              | `[]`                             |
| `envVarsSecret`                       | secrets to mount as environment variables                                                                 | `[]`                             |
| `configMapMounts`                     | configmaps to mount as directories or files                                                               | `[]`                             |
| `secretMounts`                        | secrets to mount as directories or files                                                                  | `[]`                             |
| `existingClaims`                      | existing persistent volume claims to mount as directories                                                 | `[]`                             |
| `serviceAccount.projectID`            | GCP project where GCP SA will be bound if set to true                                                     | `{{ .Values.global.projectID }}` |
| `serviceAccount.create`               | create a service account                                                                                  | `true`                           |
| `serviceAccount.annotations`          | additional GKE SA annotations to add                                                                      | `{}`                             |
| `serviceAccount.name`                 | name of the service account to use                                                                        | `""`                             |
| `serviceAccount.gcp.create`           | create a GCP service account                                                                              | `false`                          |
| `serviceAccount.gcp.bind`             | bind the GCP service account to the GKE service account                                                   | `false`                          |
| `serviceAccount.gcp.name`             | name of the GCP service account to use (formatted {{ .Values.global.env }}-{{ .Values.gcp.name }})        | `""`                             |
| `serviceAccount.gcp.fullnameOverride` | exact name of the GCP service account to use                                                              | `""`                             |
| `serviceAccount.gcp.projectID`        | GCP project where GCP SA will be created if set to true                                                   | `{{ .Values.global.projectID }}` |
| `livenessProbe`                       | liveness probe for the container (will restart the container if it fails)                                 | `{}`                             |
| `readinessProbe`                      | readiness probe for the container (will mark the container as ready if it succeeds)                       | `{}`                             |
| `startupProbe`                        | startup probe for the container                                                                           | `{}`                             |
| `lifecycle`                           | lifecycle handlers for the container                                                                      | `{}`                             |
| `service.type`                        | type of service to create                                                                                 | `ClusterIP`                      |
| `service.ports`                       | ports to expose                                                                                           | `[]`                             |
| `ports`                               | ports to expose                                                                                           | `[]`                             |
| `ingress.enabled`                     | enable ingress for the deployment                                                                         | `false`                          |
| `ingress.canaryWeight`                | percentage of traffic (0-100) to send to this deployment as canary (-1 for disabled)                      | `-1`                             |
| `ingress.className`                   | ingress class to use                                                                                      | `"nginx"`                        |
| `ingress.annotations`                 | additional annotations to add to the ingress                                                              | `{}`                             |
| `ingress.hosts`                       | hosts to add to the ingress                                                                               | `[]`                             |
| `topologySpreadConstraints`           | topology spread constraints for the deployment                                                            | `[]`                             |
| `nodeSelector`                        | node selector for the deployment                                                                          | `{}`                             |
| `tolerations`                         | tolerations for the deployment                                                                            | `{}`                             |
| `affinity`                            | affinity for the deployment                                                                               | `{}`                             |
| `enableReloader`                      | adds Reloader annotation for deployment.                                                                  | `true`                           |
