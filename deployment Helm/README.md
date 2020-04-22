# Deployment instructions for TESK

Helm chart and config to deploy the TESK service. Tested with Helm v3.0.0.

## Usage

First you must create a namespace in Kubernetes in which to deploy TESK. The
command below creates all deployments in the context of this namespace. How
the namespace is created depends on the cluster, so it is not documented here.

To deploy the application, first modify [`values.yaml`](values.yaml), then execute:

```bash
helm upgrade --install tesk . -f values.yaml
```

The first time running this command, the chart will be installed. Afterwards, it will apply any change in the chart.

##  Description of values

See [`values.yaml`](values.yaml) for default values.

| Key | Type | Description |
| --- | --- | --- |
| host_name | string | FQDN to expose the application |
| clusterType | string |type of Kubernetes cluster; either 'kubernetes' or 'openshift'|
| wes-tes_enabled | boolean | Enables the wes-tes deployment. |
| tesk.image | string | container image to be used to run TESK |
| tesk.port | integer | |
| tesk.taskmaster_image_version | string | |
| tesk.taskmaster_filer_image_version | string | |
| tesk.debug | boolean | Activates the debugging mode |
| tesk.k8s_namespace | string | Namespace to deploy TESK, used both for openshift and kubernetes deployments |
| transfer.wes_base_path | string | WesElixir locally Change the value of $wesBasePath in minikubeStart accordingly |
| transfer.tes_base_path | string | |
| transfer.pvc_name | string | |
| auth.mode | string | Can be 'noauth' to disable authentication, or 'auth' to enable it |
| auth.env_subgroup | string | Can be 'EBI' or 'CSC' |
| service.type | string | Can be 'NodePort' or 'ClusterIp' |
| service.node_port | integer | Only used if service.type is 'NodePort', specifies the port |
| ftp.active | boolean | Activates or disables the local ftp |
| ftp.virtualboxip | string | IP for the endpoint of the ftp |
| ftp.username | string | Username of the ftp server |
| ftp.password | string | Password of the ftp server |
| kubernetes.nginx_image | string | Image to use for the nginx ingress |
| kubernetes.external_ip | string | We used externalIP to expose Ingress on 80/443 port. On OpenStack internal IP of masternode (10.x.x.x) worked for us. Could be any node, but calls to the service have to be using it. In our case DNS entry is assigned to master's external IP. Use NodePort as an alternative.|
| kubernetes.node_port | integer | |
| kubernetes.tls_secret_name | string |  If no TLS secret name configured, TLS will be switched off. A template can be found at [deployment/tls_secret_name.yml-TEMPLATE](deployment/tls_secret_name.yml-TEMPLATE). |
| kubernetes.scope | string | The following variables are specific to each deployment. Use "Cluster" if you want Ingress to listen to all namespaces (requires ClusterAdmin). Leave it blank if you want Ingress to listen only to its own namespace. |

