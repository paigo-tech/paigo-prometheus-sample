# paigo-prometheus-sample

This repository contains instructions to use agent-based usage measurement and collection of Paigo.
Specifically, there are two components to set up:
- Usage Measurement is done by deploying [Kube State Metrics](https://github.com/kubernetes/kube-state-metrics)
- Usage Collection is done by deploying [Prometheus Agent](https://prometheus.io/docs/introduction/overview/).

[Full documentation](https://docs.paigo.tech/measure-usage-and-collect-data/agent-based-method) of agent-based usage measurement and collection.

### How to Deploy

-   Clone the repository
    ``` sh
    git clone git@github.com:paigo-tech/paigo-prometheus.git
    ````
-   Replace `<Fill_ME_IN>` with your client ID and secret ID for the Paigo API in the `chart/values.yaml` file
-   Authenticate with a Kubernetes cluster with the following command so that the deployment of next step will be authenticated. (AWS CLI required).
    ``` sh
    aws eks update-kubeconfig --name your-cluster-name-here
    ```
-   Install agent to the Kubernetes cluster
    ``` sh
    helm upgrade paigo-agent ./chart --install
    ```


### Measuring and Monitoring Usage
In order to see usage reflected in paigo, pods must be labeled with relevant `customerId`, and `dimensionId`.

The following command can be used in order to quickly label pods for testing. 
```sh
kubectl label pods $REPLACE_WITH_POD_NAME \
  paigoDimensionId=$REPLACE_WITH_DIMENSION_ID \
  paigoCustomerId=$REPLACE_WITH_CUSTOMER_ID
```

 See docs for more information on the [required tagging schema](https://docs.paigo.tech/measure-usage-and-collect-data/agent-based-method#required-tagging-schema). 


# Support

Email [team@paigo.tech](mailto:team@paigo.tech) for support or questions.
