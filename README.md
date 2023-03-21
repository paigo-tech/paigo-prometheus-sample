# paigo-prometheus

This repository contains configuration for a [prometheus](https://prometheus.io/docs/prometheus/latest/getting_started/) agent to work with paigo, and a deployment of [kube-state-metrics](https://github.com/kubernetes/kube-state-metrics). Following the below instructions you can deploy a paigo agent to a cluster.

## Whats deployed with the chart

-   Prometheus Agent
-   Kube State metrics

### How to deploy

-   Clone the repository
-   replace `<Fill_ME_IN>` with your client id and secret for the Paigo API in the `values.yaml` file
-   Run `helm upgrade paigo-agent ./chart --install` while authenticated to your kubernetes cluster. To authenticate with a cluster in AWS you can make use of the following command, assuming you have the [AWS CLI](https://aws.amazon.com/cli/) installed.

```sh
aws eks update-kubeconfig --name your-cluster-name-here
```


### Measuring and Monitoring Usage
In order to see usage reflected in paigo, pods must contain relevant `customerId`, and `dimensionId` labels.

The following command can be used in order to quickly label pods for testing. 
```sh
kubectl label pods $REPLACE_WITH_POD_NAME \
  paigoDimensionId=$REPLACE_WITH_DIMENSION_ID \
  paigoCustomerId=$REPLACE_WITH_CUSTOMER_ID
```

 See our docs for more information on the [required tagging schema](https://docs.paigo.tech/measure-usage-and-collect-data/agent-based-method#required-tagging-schema). 



# Support

Reach out to `team@paigo.tech` for support or questions.