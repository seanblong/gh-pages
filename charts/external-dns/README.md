# external-dns

ExternalDNS is a Kubernetes addon that configures public DNS servers with information
about exposed Kubernetes services to make them discoverable.

## Parameters

### Global

Globally applicable overrides for Dexcom Helm charts.
The only required value is the environment name.

| Name                          | Description                                                               | Value |
| ----------------------------- | ------------------------------------------------------------------------- | ----- |
| `global.env`                  | the environment name                                                      | `""`  |
| `global.projectID`            | the GCP project ID, required when setting up GCP service accounts         | `""`  |
| `clusterSecretStore`          | The name of the secret store to use for storing secrets                   | `""`  |
| `cloudflareAPITokenSecretKey` | The keyname of the secret in the secretstore for the cloudflare api token | `""`  |

### Bitnami External-DNS

This section exposes the overrides for the Bitnami's external-dns
chart. See: https://github.com/bitnami/charts/tree/external-dns/8.1.0/bitnami/external-dns

| Name           | Description                                  | Value |
| -------------- | -------------------------------------------- | ----- |
| `external-dns` | Overrides for the Bitnami external-dns chart | `{}`  |
