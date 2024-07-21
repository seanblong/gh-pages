---
layout: page
---


# Getting Started

## Configuration

The first, and arguably most important, configuration is the name of the environment.
This is derived from the name of your `yaml` file.  Names can be lowercase alphanumeric
and include hyphens, but no other characters are allowed and names must start with
a letter and be no more than 40 chars.

The rest of the `tridev` configuration is presented below:

```yaml
## @param clusterSetup [default: automatic]  determines how cluster is maintained.  Supply "manual" if cluster is already created.
## DO NOT CHANGE!  Set to manual for Shanghaied setups
##
# clusterSetup: automatic

## @param projectID [default: platform-cloud-engineering]  GCP project ID to create the cluster and Crossplane-managed resources
##
# projectID: platform-cloud-engineering

## @param releaseChannel [default: regular]  Release channel used to create the GKE autopilot cluster
## Allowed values: rapid, regular, or stable.  This can be modified later.
##
# releaseChannel: regular

## @param lifecycle [default: * * * * *]  Cron expression to establish lifecycle of GKE autopilot cluster
## Cron expression will always ignore minutes argument, it only considers hour and above
## arguments. Every hour at minute 00 a scheduled job will evaluate all the environments
## lifecycle expressions; environments whose lifecycle is out of range will be automatically
## destroyed (if cluster exists) and for those environments whose cluster hasn't been created
## and they are inside the date-time range expressed in their lifecycle, those will be
## automatically created.
## CAUTION: Every environment using a lifecycle expression different from the default
## value "* * * * *", which allows an environment to be running 24/7, as soon as they
## are outside of time range, they will be destroyed along with every workload and disks
## that were used inside of them.
# lifecycle: * * * * *

## @param releases [array]  List of Helm chart releases to install into cluster
##
## name: "Required: name of the chart."
## version: "Required: version of the chart."
## alias: "Optional: Alias name for a chart.  Helpful when installing same chart multiple times."
## namespace: "Optional: namespace to install the chart (default: Default)."
## repo: "Optional: Artifactory repo URL (default: https://dexcom.jfrog.io/artifactory/dexcom-helm-dev-local)."
## wave: "Optional (integer): Argo sync wave to install the chart (default: 0)."
## values: | # optional override values file as a string.
##     example:
##       replicas: 2
## remote

## Building your first Tridev environment

To build your first environment it is recommended to grab an existing environment
for reference.  The Cloud Engineering team typically has an environment with most
of the Kuberenetes add-ons, e.g., `external-dns`, `nginx-ingress`, etc., preconfigured
and this file can be referenced to start.

- Step 1: Inside the `tridev` folder on the `dev` branch create/copy a new `yaml`
  file.  Whatever you name the file will be the name of the environment and
  cluster.

- Step 2: Add any Helm chart releases to the file under the `releases` section.
  See [Configuation](#configuration) for full instructions, but minimally you'll
  need a chart name and version.  By default Tridev will pull from
  `dexcom.jfrog.io/dexcom-helm-dev-virtual`, but this can be overriden by
  supplying a `repo` override.

  ```yaml
  releases:
    - name: my-chart
      version: 1.0.1
  ```
