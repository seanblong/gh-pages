# Tridev

![tridev](assets/tridev.jpeg)

> Three principal deities in Hinduism is known as the Trimurti,“त्रिमूर्ति”
> Each component of the term is broken down as follows:
> त्रि (tri) - meaning “three”
> मूर्ति (murti) - meaning “form” or “manifestation”
> Together, “Trimurti” (Tridev) signifies the three forms or manifestations,
> referring to in Hindu theology as Brahma (the Source/Creator), Vishnu
> (the Preserver/Indwelling-life) and Shiva (the Transformer Destroyer/Creator).

**Tridev** is a GitOps utility that creates ephemeral environments based on manifests
in this repository.

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
## remoteValues: []  # list of remote values files as a URL
releases: []
```

Upon merging into `dev` the above `tridev` manifest will be converted into individual
Argo `application` manifests under the `argo` directory (folder will be named by
the same as your environment, i.e. `tridev` file name).  The default branch for
the Argo files will be [deploy](https://github.com/dexcom-inc/platform-toolbox/tree/deploy).
This branch has few protections and the generated Argo files can be updated at will,
though keep in mind that changes made directly in that branch aren't guaranteed
to be preserved.

A GKE cluster will likewise be created, again named after the specified `env` and
each release will be deployed to that cluster.

Subsequent updates can be made to the `tridev` manifest and these will likewise
be pushed to the GKE cluster though no additional cluster will be created.

## Getting Started

There's a sample demo environment provided in the `tridev` directory.  The demo
includes many of the base tooling most clusters need including:

- Nginx Ingress
- Cert Manager
- External DNS

Copy this file and give it a unique name for your environment.  Create a PR and,
upon merge, the Github Action will run to generate a new cluster in the
[platform-cloud-engineering](https://console.cloud.google.com/kubernetes/list/overview?project=platform-cloud-engineering)
project.
