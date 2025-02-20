# Operator-sandbox
Operator Sandbox

### Installation
This page provides instructions on installing the Datadog Agent in a Kubernetes environment through Datadog Operator

### Minimum Agent and Cluster Agent versions
Some features related to later Kubernetes versions require a minimum Datadog Agent version.

- KUBERNETES VERSION 1.16.0+ | 1.21.0+ | 1.22.0+
- AGENT VERSION	7.19.0+ | 7.36.0+ | 7.37.0
- CLUSTER AGENT VERSION	1.9.0+ | 1.20.0+ | 7.37.0+
- https://docs.datadoghq.com/containers/cluster_agent/#minimum-agent-and-cluster-agent-versions
	            	      	                               	             
The Datadog Operator is a way to deploy the Datadog Agent on Kubernetes and OpenShift. It reports deployment status, health, and errors in its Custom Resource status, and it limits the risk of misconfiguration thanks to higher-level configuration options.

### Prerequisites
Using the Datadog Operator requires the following prerequisites:
Requires Helm (https://helm.sh/) and the kubectl CLI (https://kubernetes.io/docs/tasks/tools/#kubectl).

### Deploy an Agent with the Operator
To deploy the Datadog Agent with the operator in the minimum number of steps, see the datadog-operator Helm chart. Here are the steps:

### Install the Datadog Operator: https://artifacthub.io/packages/helm/datadog/datadog-operator
`helm repo add datadog https://helm.datadoghq.com`
`helm install my-datadog-operator datadog/datadog-operator`

### Create a Kubernetes secret with your API and app keys
`kubectl create secret generic datadog-secret --from-literal api-key=<DATADOG_API_KEY> --from-literal app-key=<DATADOG_APP_KEY> Replace <DATADOG_API_KEY> and <DATADOG_APP_KEY> with your Datadog API and application keys`

Replace <DATADOG_API_KEY> and <DATADOG_APP_KEY> with your Datadog API and application keys

### Create a file with the spec of your Datadog Agent deployment configuration. The simplest configuration is as follows:

``` 
apiVersion: datadoghq.com/v2alpha1
kind: DatadogAgent
metadata:
  name: datadog
spec:
  global:
    clusterName: <CLUSTER_NAME>
    site: <DATADOG_SITE>
    credentials:
      apiSecret:
        secretName: datadog-secret
        keyName: api-key
```
Replace <CLUSTER_NAME> with a name for your cluster.
Replace <DATADOG_SITE> with your Datadog site. Your site is datadoghq.com. (Ensure the correct SITE is selected on the right).

Deploy the Datadog Agent with the above configuration file:

`kubectl apply -f /path/to/your/datadog-agent.yaml`

### Cleanup
The following command deletes all the Kubernetes resources created by the above instructions:
`kubectl delete datadogagent datadog`
`helm delete my-datadog-operator`

For further details on setting up Operator, including information about using tolerations, refer to the Datadog Operator advanced setup guide:https://docs.datadoghq.com/agent/guide/operator-advanced/

### Uninstall
```
kubectl delete datadogagent datadog
helm delete datadog-operator
```
