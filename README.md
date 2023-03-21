# paigo-prometheus-sample

This repository contains instructions to use agent-based usage measurement and collection of Paigo. Paigo leverages Prometheus for agent-based integration method. There are two steps in agent-based integration method: usage measurement and usage collection.
- **Usage Measurement**: The usage data can be measured from any program compatible with Prometheus Exporter protocol, including Prometheus official exporters, Community-supported exporters, 3rd-party exporters or any custom exporters. See a sample list of available exporter here.
- **Usage Collection**: The usage data can be collected to Paigo backend with Prometheus agent, which takes data from exporters. Paigo API has a Prometheus protocol compatible endpoint that listens for usage data.

[Full documentation](https://docs.paigo.tech/measure-usage-and-collect-data/agent-based-method) of agent-based usage measurement and collection.

## Deploy Agent and Exporter

To follow the sample deployment, the following components are required in the environment.
- [Helm](https://helm.sh/docs/intro/install/)
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

This repository contains a sample deployment to leverage Prometheus to gather the usage of Elastic Kubernetes Service on AWS, and collect them to Paigo API. However, Paigo API can receive a wide range of usage data that are in Prometheus protocol. In the sample deployment, there are two components installed:
- Usage Measurement is done by deploying [Kube State Metrics](https://github.com/kubernetes/kube-state-metrics)
- Usage Collection is done by deploying [Prometheus Agent](https://prometheus.io/docs/introduction/overview/).

### Steps to Deploy

- Clone the repository
 ``` sh
 git clone git@github.com:paigo-tech/paigo-prometheus-sample.git
 ````
- Replace `<Fill_ME_IN>` with your client ID and secret ID for the Paigo API in the `chart/values.yaml` file
- (OPTIONAL) Authenticate with a Kubernetes cluster with the following command so that the deployment of next step will be authenticated. This step is required for Kubernetes cluster on AWS EKS.
 ``` sh
 aws eks update-kubeconfig --name your-cluster-name-here
 ```
- Install agent to the Kubernetes cluster
 ``` sh
 helm upgrade paigo-agent ./chart --install
 ```
- Verify the agent is running successfully with the following command.
 ``` sh
 kubectl get pods -n paigo-billing
 ```
There should be a pod running with name string starting with `paigo-agent-` and the status should be **RUNNING**.

## Required Tagging Schema

Exporters and agents collect a large volume of data and send over to Paigo API. However, not all data is counted as usage record for billing or pricing. In order for the data collected by exporter to be linked to a dimension in Paigo and attribute the usage to customers correctly, additional metadata is required. Label the pods running in Kubernetes cluster with the following schema in order for the agent and Paigo Usage Measurement and Collection engine to treat the data as usage record.

- `paigoDimensionId`: REQUIRED. The **Dimension ID** of the dimension this usage record is associated with, assigned by Paigo during dimension creation. Example: `e8366954-6f36-47e9-8431-ac95f88b5cc7`.
- `paigoCustomerID`: REQUIRED. The **Customer ID** of a customer this usage record attributes to, assigned by Paigo during customer creation. Example: `e8366954-6f36-47e9-8431-ac95f88b5cc7`.

Using the following sample command to assign tags to pods of Kubernetes cluster:

```sh
kubectl label pods $REPLACE_WITH_POD_NAME \
  paigoDimensionId=$REPLACE_WITH_DIMENSION_ID \
  paigoCustomerId=$REPLACE_WITH_CUSTOMER_ID
```

 See docs for more information on the [required tagging schema](https://docs.paigo.tech/measure-usage-and-collect-data/agent-based-method#required-tagging-schema). 

## Undeploy Agent and Exporter

Use the following sample command to stop and undeploy the package of agent and exporter:

```sh
helm uninstall paigo-agent
```

# Support

Email [team@paigo.tech](mailto:team@paigo.tech) for support or questions.
